---
name: go-development
description: >
  Expert skill for writing production-grade Go (Golang) code. Trigger for ANY of: Go project setup,
  Go modules, REST APIs (Gin, Echo, Chi, net/http), gRPC services, GraphQL in Go, CLI tools with Cobra,
  goroutines, channels, concurrency patterns, error handling, interfaces, generics, struct design,
  database access (sqlc, GORM, pgx, sqlx), PostgreSQL/MySQL/SQLite with Go, Redis, testing (unit,
  integration, table-driven, mocks), benchmarks, middleware, authentication (JWT, OAuth2, sessions),
  Docker for Go, deployment, observability (OpenTelemetry, Prometheus, Zap/slog logging), microservices,
  event-driven patterns, Go workspace setup, or any Go code review or refactor. Always use this skill —
  it contains Go idioms, project layout conventions, concurrency safety rules, and error handling
  patterns that prevent the most common Go mistakes.
---

# Go Development — Master Skill

**Go version target**: Go 1.22+ (generics, slog, range-over-func available)

This skill guides Claude through writing idiomatic, production-ready Go. Go has strong conventions that differ significantly from other languages — ignoring them produces code that compiles but that experienced Go developers would immediately flag in review. Read each section relevant to the user's request before writing code.

---

## How to Use This Skill

Identify which phase(s) apply to the user's request and read the corresponding reference file before writing any code.

Phase 1 — Project classification and layout  
Phase 2 — Go idioms and language patterns  
Phase 3 — Error handling (Go's most common source of bugs)  
Phase 4 — Concurrency  
Phase 5 — API layer (REST, gRPC, GraphQL)  
Phase 6 — Data layer (SQL, Redis, migrations)  
Phase 7 — Testing  
Phase 8 — Observability and logging  
Phase 9 — Build and deployment  

---

## Phase 1 — Project Classification and Layout

Before writing any code, classify the project type. This determines the directory structure, the right framework (if any), and the module layout.

**CLI tool**: A single binary that runs a command. Use Cobra for complex multi-command CLIs, plain `os.Args` or `flag` for simple single-command tools. No web framework needed. See `references/cli.md`.

**REST API / web service**: A long-running HTTP server. Choose the router/framework based on complexity — `net/http` with its new 1.22 routing patterns for simple services, Chi for middleware-heavy services without the overhead of a full framework, Gin or Echo when the team values ergonomics and a larger middleware ecosystem. See `references/api-rest.md`.

**gRPC service**: Protocol-buffer-defined service interface, code-generated from .proto files. Use `google.golang.org/grpc` with `connectrpc` for browser compatibility. See `references/api-grpc.md`.

**Background worker / event consumer**: A service that processes jobs from a queue (Redis, RabbitMQ, Kafka). No HTTP server, or a minimal one for health checks only. Concurrency patterns are central here.

**Library / package**: Code intended to be imported by other Go modules. The API surface is the package's exported types and functions. Extra care on naming, backward compatibility, and documentation.

**Standard project layout** for a service:

```
myservice/
├── cmd/
│   └── server/
│       └── main.go          # Entry point only — wire everything together here
├── internal/                # Not importable by external packages
│   ├── handler/             # HTTP handlers (thin — delegate to service)
│   ├── service/             # Business logic
│   ├── repository/          # Database access
│   ├── middleware/          # HTTP middleware
│   └── model/               # Domain types
├── pkg/                     # Importable utilities (only if genuinely reusable)
├── migrations/              # SQL migration files
├── config/                  # Config loading
├── Makefile
├── Dockerfile
├── go.mod
└── go.sum
```

The `internal/` directory is not a convention — it is enforced by the Go compiler. Packages inside `internal/` cannot be imported by code outside the module root. Use this aggressively for service internals. Only put things in `pkg/` if they are genuinely useful to multiple separate modules.

Keep `main.go` thin. Its job is to read config, construct dependencies (database, cache, logger), wire them into services and handlers, and start the server. Business logic in main.go is a red flag.

---

## Phase 2 — Go Idioms and Language Patterns

Read `references/idioms.md` for detailed patterns. Key principles to apply immediately:

**Naming**: Go names are short and context-dependent. A function that gets a user in a `user` package is `Get`, not `GetUser`. Acronyms are all-caps: `userID` not `userId`, `HTTPClient` not `HttpClient`. Unexported names are lowercase. Interfaces are named for what they do, often with an `-er` suffix: `Reader`, `Writer`, `Storer`, `Notifier`.

**Interfaces**: Define interfaces at the point of use, not at the point of implementation. A handler that needs to fetch users defines a `UserFetcher` interface with exactly the methods it needs — not the full `UserService` interface. Keep interfaces small; a one-method interface is ideal. Never define an interface "for future use" — wait until you have two concrete implementations.

**Struct design**: Embed types to compose behavior, not to inherit it. Put mutexes adjacent to the fields they protect and document the relationship. Use value receivers for small structs; pointer receivers when the method modifies state or when the struct is large. Be consistent within a type — don't mix value and pointer receivers.

**Zero values**: Design types so their zero value is useful. A `sync.Mutex` is unlocked by default. A `bytes.Buffer` is ready to write. Avoid patterns that require a constructor to produce a valid object when the zero value could work.

**Generics (Go 1.18+)**: Use generics when a function or type genuinely works identically for multiple types and the alternative is either code duplication or empty-interface gymnastics. Do not use generics to be clever — idiomatic Go prefers concrete types and clear code over abstract cleverness. Common good uses: typed collection wrappers, generic result/option types, generic sorting/filtering utilities.

**The blank identifier**: Use `_` for imports whose side effects are needed (`_ "github.com/lib/pq"`) and to explicitly discard values you know you do not need. Never use `_` to silently discard errors — that is always a bug waiting to happen.

---

## Phase 3 — Error Handling

Error handling is where Go differs most visibly from other languages, and where most Go mistakes happen. Read `references/errors.md` for full patterns.

Go errors are values. Returning an error is the normal control flow for functions that can fail — it is not an exceptional case. Every error return must be either handled, wrapped and returned, or explicitly and deliberately ignored with a comment explaining why.

**The cardinal rule**: Never ignore an error silently. `result, _ := someFunc()` is almost always wrong. If you genuinely cannot handle an error, log it and continue, or wrap it and return it to the caller. If ignoring is truly correct (e.g. a best-effort log flush on shutdown), add a comment: `_ = logger.Sync() // best-effort on shutdown`.

**Wrapping errors**: Use `fmt.Errorf("loading user %d: %w", id, err)` to add context as errors propagate up the call stack. The `%w` verb makes the original error unwrappable with `errors.Is` and `errors.As`. Add context at each layer that knows something useful: the repository layer says what database operation failed, the service layer says what business operation was attempted, the handler layer says what the user was trying to do.

**Sentinel errors vs typed errors**: Use sentinel errors (`var ErrNotFound = errors.New("not found")`) for simple conditions that callers need to check. Use typed error structs when callers need to extract data from the error (e.g. a validation error that carries which fields failed). Use `errors.Is` to check sentinel errors and `errors.As` to extract typed errors — never string-match error messages.

**Panic**: Panic is for programmer errors only — index out of bounds, nil dereference, invariant violations that should never happen in correct code. Never panic for runtime errors (network failures, invalid input, database errors). Recover from panics only at the top-level handler to prevent a single goroutine crash from taking down the whole server.

---

## Phase 4 — Concurrency

Read `references/concurrency.md` before writing any goroutine or channel code.

Go's concurrency primitives are powerful but require discipline. The most common bugs are goroutine leaks (goroutines that are started but never terminated), data races (concurrent access to shared memory without synchronization), and deadlocks (goroutines waiting on each other in a cycle).

**Start with the question**: does this actually need to be concurrent? Sequential code is easier to read, test, and debug. Use concurrency when there is genuine parallelism to exploit (CPU-bound work on multi-core) or when blocking on I/O would otherwise stall work that could proceed (fan-out HTTP calls, concurrent database queries).

**Goroutine ownership**: every goroutine must have a clear owner responsible for ensuring it terminates. Use `context.Context` for cancellation — pass it as the first argument to any function that starts a goroutine or blocks on I/O. When the context is cancelled, goroutines should return promptly. Use `sync.WaitGroup` to wait for a group of goroutines to finish before proceeding.

**Channel direction**: be explicit about channel direction in function signatures. A function that only sends uses `chan<- T`, a function that only receives uses `<-chan T`. This documents intent and prevents misuse.

**Prefer `sync` primitives for shared state**: when goroutines share a struct, protect it with a `sync.RWMutex`. Use `sync/atomic` for simple counters and flags. Use channels for passing ownership of data between goroutines — "do not communicate by sharing memory; share memory by communicating."

**Run the race detector**: always run tests with `-race`. The race detector finds data races at runtime that are invisible in code review. Make it part of the CI pipeline: `go test -race ./...`.

---

## Phase 5 — API Layer

Read `references/api-rest.md` for REST patterns, `references/api-grpc.md` for gRPC.

Key principles regardless of framework:

Handlers are thin. A handler's job is: parse the request, call a service method, write the response. Business logic in handlers is hard to test and impossible to reuse. A handler that is longer than 30 lines is usually doing too much.

Middleware handles cross-cutting concerns: authentication, request ID injection, structured logging, panic recovery, rate limiting, CORS. Middleware is composable — chain it explicitly so the order is visible.

Always validate and parse input at the boundary. Use a validation library (go-playground/validator, or manual Zod-equivalent struct validation) before passing data to the service layer. Return structured error responses — never leak internal error messages to the client.

Set timeouts on all outbound HTTP calls. The default `http.Client` has no timeout — a slow external service will hold your goroutine forever. Always configure `Timeout` on the client and `ReadTimeout`/`WriteTimeout`/`IdleTimeout` on the server.

---

## Phase 6 — Data Layer

Read `references/database.md` for SQL patterns, connection pooling, migration strategy, and Redis usage.

The choice between sqlc, GORM, and raw SQL matters significantly:

**sqlc**: generate type-safe Go code from hand-written SQL queries. You write SQL, sqlc generates the Go functions. Best choice when you want full control over queries, type safety at compile time, and no ORM magic. Requires a PostgreSQL/MySQL schema and query files. Recommended for most new projects.

**pgx directly**: the fastest PostgreSQL driver. Use it with the `pgxpool` connection pool. No query generation — you write both the SQL and the Go scanning code. Most appropriate when you need fine-grained control over PostgreSQL-specific features.

**GORM**: a full ORM. Reduces boilerplate for simple CRUD. Generates SQL from struct tags. The generated SQL is sometimes inefficient for complex queries. Best for prototypes and simple CRUD-heavy services where developer speed matters more than query control.

**sqlx**: a thin extension of `database/sql` that adds struct scanning and named parameters. A middle ground — you write SQL, it handles scanning. Good when your team knows SQL but finds raw `database/sql` scanning tedious.

Always use a migration tool (golang-migrate, goose, or Atlas). Never apply schema changes manually to a production database. Migrations must be reversible (have both up and down scripts). Run migrations at application startup in development; run them as a separate step before deployment in production.

---

## Phase 7 — Testing

Read `references/testing.md` for table-driven tests, mock patterns, and integration test setup.

Go testing is built into the standard library. `go test ./...` runs all tests. The conventions are strong and widely followed — departing from them makes your tests harder for other Go developers to read.

**Table-driven tests** are the Go standard for testing multiple inputs. Define a slice of test cases as a struct with name, input, and expected output. Loop over them with `t.Run(tc.name, ...)`. This produces clear output when a specific case fails and scales to many cases without repetition.

**Test file placement**: test files live alongside the code they test (`user_service_test.go` next to `user_service.go`). Use the `_test` package suffix (`package service_test`) for black-box testing of the public API. Use the same package name for white-box tests that need access to unexported identifiers.

**Interfaces enable testing**: the dependency injection pattern — accepting interfaces rather than concrete types — makes mocking trivial. Generate mocks with `mockery` or write them by hand for simple interfaces. Never mock types you don't own (standard library, third-party packages) — test against the real thing or use an integration test.

**`testify`**: the `github.com/stretchr/testify` library is near-universal in Go projects. Use `assert` for non-fatal checks (test continues after failure) and `require` for fatal checks (test stops immediately). This makes test failures much easier to read than raw `if got != want { t.Error... }`.

---

## Phase 8 — Observability and Logging

Read `references/observability.md` for structured logging with slog, OpenTelemetry tracing, and Prometheus metrics.

**Logging**: use Go 1.21's built-in `log/slog` for structured logging. Every log entry should be structured key-value pairs, not a formatted string. The log level, message, and context fields (request ID, user ID, trace ID) should be machine-parseable. Never use `fmt.Println` or `log.Printf` in production service code.

**Tracing**: use OpenTelemetry (`go.opentelemetry.io/otel`) for distributed tracing. Instrument every significant operation: inbound HTTP requests, outbound HTTP calls, database queries. Propagate the trace context through function calls via `context.Context`. Export traces to Jaeger, Tempo, or a managed service like Datadog or Honeycomb.

**Metrics**: use the Prometheus client library (`github.com/prometheus/client_golang`) to expose metrics. Instrument: request count and duration (histogram, labeled by route and status code), error rate, database connection pool size, queue depth (for workers). Expose them at `/metrics` with the standard Prometheus handler.

---

## Phase 9 — Build and Deployment

Read `references/deployment.md` for Dockerfile patterns, build flags, and release practices.

**Build**: `go build -o bin/server ./cmd/server` produces a static binary. Set `CGO_ENABLED=0` and `GOOS=linux` for a Linux binary that runs in a scratch or distroless Docker container without a C runtime. Inject version information at build time with `-ldflags "-X main.version=$(git describe --tags)"`.

**Docker**: Go binaries make for excellent minimal Docker images. Build in a `golang:1.22-alpine` stage, copy only the binary to a `gcr.io/distroless/static` or `alpine:3` final stage. The resulting image is typically 10–20 MB. See `references/deployment.md` for the full multi-stage Dockerfile pattern.

---

## Reference Files Index

| File | When to read it |
|---|---|
| `references/idioms.md` | Naming, interfaces, struct patterns, generics, Go-specific conventions |
| `references/errors.md` | Error wrapping, sentinel errors, typed errors, panic vs error |
| `references/concurrency.md` | Goroutines, channels, WaitGroup, context cancellation, race detector |
| `references/api-rest.md` | net/http, Chi, Gin, Echo — handler patterns, middleware, validation |
| `references/api-grpc.md` | Proto definitions, code generation, streaming, interceptors |
| `references/database.md` | sqlc, pgx, GORM, sqlx, migrations, connection pooling, Redis |
| `references/testing.md` | Table-driven tests, mocks, testify, integration tests, benchmarks |
| `references/observability.md` | slog structured logging, OpenTelemetry tracing, Prometheus metrics |
| `references/deployment.md` | Dockerfile, build flags, version injection, CI/CD, config management |
| `references/cli.md` | Cobra, flag, argument parsing, config files, shell completion |
