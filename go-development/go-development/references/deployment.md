# Go Build and Deployment

## Building Go Binaries

Go produces statically linked binaries by default — a single file with no external dependencies. This is one of Go's biggest operational advantages. The binary can be copied to any compatible Linux machine and run without installing a runtime, interpreter, or shared libraries.

The standard build command for a production Linux binary:

```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build \
  -ldflags "-s -w -X main.version=$(git describe --tags --always) -X main.buildTime=$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
  -o bin/server ./cmd/server
```

`CGO_ENABLED=0` disables C extensions, producing a truly static binary. Without this, some standard library functions use system C libraries, and the binary will not run in a scratch container.

`-s -w` strips the symbol table and debug information, reducing binary size by 30–50%.

`-X main.version=...` injects the git version tag as a string constant at build time. Use this to expose the version via a `/health` endpoint so you always know which version is running.

`GOOS=linux GOARCH=amd64` cross-compiles for Linux x86-64 from any development machine, including macOS with Apple Silicon.

## Dockerfile — Multi-Stage Build

Multi-stage Dockerfiles produce minimal images. The build stage uses the full Go SDK; the final stage contains only the binary.

```dockerfile
# Stage 1: Build
FROM golang:1.22-alpine AS builder

WORKDIR /app

# Cache dependencies before source code
COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags "-s -w -X main.version=$(git describe --tags --always)" \
    -o server ./cmd/server

# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12

WORKDIR /app

COPY --from=builder /app/server .

# Run as non-root user
USER nonroot:nonroot

EXPOSE 8080

ENTRYPOINT ["/app/server"]
```

The `distroless/static` image is ~2MB and contains only the bare minimum to run a static Go binary. No shell, no package manager, no utilities — a smaller attack surface. For services that need to run shell scripts or use tools like `curl` for health checks, use `alpine:3` as the final stage instead.

Copy `go.mod` and `go.sum` before the source code. Docker caches the `go mod download` layer as long as these files do not change. Source code changes do not invalidate the dependency cache, making builds significantly faster in CI.

## Configuration Management

Use environment variables for configuration — not config files embedded in the binary. Environment variables are the standard for containerized applications (12-factor app principle) and integrate naturally with Docker, Kubernetes, and ECS.

Parse configuration at startup into a typed struct. Use a library like `github.com/caarlos0/env` for struct-tag-based parsing with defaults and validation, or parse manually with `os.Getenv` for simpler services. Fail fast at startup if required configuration is missing — a service that starts with incomplete configuration will fail unpredictably later.

```go
type Config struct {
    DatabaseURL  string        `env:"DATABASE_URL,required"`
    Port         int           `env:"PORT" envDefault:"8080"`
    LogLevel     string        `env:"LOG_LEVEL" envDefault:"info"`
    JWTSecret    string        `env:"JWT_SECRET,required"`
    RedisURL     string        `env:"REDIS_URL"`
    SentryDSN    string        `env:"SENTRY_DSN"`
    ReadTimeout  time.Duration `env:"READ_TIMEOUT" envDefault:"15s"`
    WriteTimeout time.Duration `env:"WRITE_TIMEOUT" envDefault:"30s"`
}
```

Validate the configuration struct at startup and exit with a clear error message if required values are missing or invalid. A service that silently uses default values for required configuration is a production incident waiting to happen.

## Graceful Shutdown

Always implement graceful shutdown. When a SIGTERM signal arrives (the standard shutdown signal from Kubernetes, ECS, and most process managers), the service should stop accepting new connections, finish processing in-flight requests, flush buffered logs, and close database connections — then exit cleanly.

```go
// In main.go
server := &http.Server{...}

// Start server in background
go func() {
    if err := server.ListenAndServe(); err != http.ErrServerClosed {
        logger.Error("server failed", "error", err)
        os.Exit(1)
    }
}()

// Wait for shutdown signal
quit := make(chan os.Signal, 1)
signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
<-quit

logger.Info("shutting down server")
ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
defer cancel()

if err := server.Shutdown(ctx); err != nil {
    logger.Error("shutdown failed", "error", err)
}

// Close other resources: db pool, redis, flush logs
pool.Close()
logger.Info("server stopped")
```

The 30-second shutdown timeout should be longer than your longest expected request duration and shorter than the Kubernetes `terminationGracePeriodSeconds` (default 30s, but configure it higher for services with slow requests).

## Health Checks

Expose two health check endpoints:

`/healthz` (liveness): returns 200 if the process is running. Used by Kubernetes to decide whether to restart the pod. Should never fail for a running process unless it is in a truly unrecoverable state. Keep it trivially simple — just `w.WriteHeader(200)`.

`/readyz` (readiness): returns 200 if the service can handle traffic. Checks database connectivity, Redis connectivity, and any other critical dependencies. Returns 503 if any check fails. Kubernetes removes the pod from the load balancer rotation when this returns non-200, preventing traffic from reaching an unhealthy instance.

Include the version string in the response body for both endpoints — it makes debugging which version is running much easier.

## Makefile

Provide a Makefile for common development tasks. Every Go project should have:

```makefile
.PHONY: build test lint run docker

build:
	CGO_ENABLED=0 go build -ldflags "-s -w" -o bin/server ./cmd/server

test:
	go test -race -cover ./...

test-integration:
	go test -race -tags integration ./...

lint:
	golangci-lint run ./...

run:
	go run ./cmd/server

docker:
	docker build -t myservice:dev .

migrate-up:
	migrate -path migrations -database $$DATABASE_URL up

migrate-down:
	migrate -path migrations -database $$DATABASE_URL down 1

generate:
	go generate ./...
	sqlc generate
```

This makes the project self-documenting — a new developer can run `make` to see the available commands and get started without reading documentation.
