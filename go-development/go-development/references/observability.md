# Observability in Go: Logging, Tracing, Metrics

## Structured Logging with slog

Go 1.21 introduced `log/slog` as the standard structured logging package. Use it for all new Go code. Structured logging means every log entry is a collection of typed key-value pairs that can be parsed, indexed, and queried by log aggregation systems (Loki, Elasticsearch, Datadog Logs). A formatted string like `"user 42 logged in from 192.168.1.1"` cannot be queried efficiently. A structured entry `{user_id: 42, ip: "192.168.1.1", event: "login"}` can.

Initialize a logger at application startup and pass it as a dependency to every struct that needs to log. Never use the default `slog.Default()` logger in library code — accept a `*slog.Logger` as a parameter. Do not use package-level logger variables.

```go
// In main.go — choose the handler based on environment
var handler slog.Handler
if os.Getenv("ENV") == "production" {
    handler = slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{Level: slog.LevelInfo})
} else {
    handler = slog.NewTextHandler(os.Stdout, &slog.HandlerOptions{Level: slog.LevelDebug})
}
logger := slog.New(handler)
```

In production, JSON output (one JSON object per line) is machine-readable. In development, text output is human-readable. The switch is at initialization — the rest of the code is identical.

Use `logger.With(...)` to create a child logger with persistent attributes. A request handler creates a child logger with the request ID and user ID attached — every log entry from that request automatically includes those fields without passing them explicitly.

```go
reqLogger := logger.With(
    slog.String("request_id", requestID),
    slog.String("user_id", userID),
    slog.String("path", r.URL.Path),
)
// Pass reqLogger through the request lifecycle
```

Log at the appropriate level. Debug for detailed tracing useful during development. Info for normal operational events (server started, request processed, job completed). Warn for unexpected situations the service handles gracefully. Error for failures that require attention but do not stop the service. Fatal (log then exit) only for startup failures.

## Distributed Tracing with OpenTelemetry

OpenTelemetry (`go.opentelemetry.io/otel`) is the standard for distributed tracing in Go. It instruments your code with spans — units of work with a start time, end time, and attached attributes. Spans are nested into traces that show the full path of a request through your system.

Initialize the tracer provider once at startup, configured with an exporter (Jaeger, Tempo, Datadog, Honeycomb — all support the OTLP protocol):

```go
exporter, _ := otlptracehttp.New(ctx,
    otlptracehttp.WithEndpoint(os.Getenv("OTEL_EXPORTER_OTLP_ENDPOINT")),
)
provider := trace.NewTracerProvider(
    trace.WithBatcher(exporter),
    trace.WithResource(resource.NewWithAttributes(
        semconv.SchemaURL,
        semconv.ServiceName("my-service"),
        semconv.ServiceVersion(version),
    )),
)
otel.SetTracerProvider(provider)
```

Instrument inbound HTTP requests with the OpenTelemetry middleware for your router (available for Chi, Gin, Echo, and net/http). This creates a root span for each request and propagates the trace context through the `context.Context`.

Create child spans for significant operations within a request:

```go
func (r *UserRepository) GetByID(ctx context.Context, id string) (*User, error) {
    ctx, span := otel.Tracer("user-repo").Start(ctx, "GetUser")
    defer span.End()

    span.SetAttributes(attribute.String("user.id", id))

    user, err := r.query(ctx, id)
    if err != nil {
        span.RecordError(err)
        span.SetStatus(codes.Error, err.Error())
        return nil, err
    }
    return user, nil
}
```

Propagate the trace context to outbound HTTP calls by injecting the trace headers:

```go
req, _ := http.NewRequestWithContext(ctx, "GET", url, nil)
otel.GetTextMapPropagator().Inject(ctx, propagation.HeaderCarrier(req.Header))
```

Record errors on spans and set the span status to Error so the tracing UI highlights failed spans. Include the user ID, order ID, or other business identifiers as span attributes — they make traces searchable and meaningful.

## Metrics with Prometheus

Use the Prometheus client library (`github.com/prometheus/client_golang`) to expose metrics. Register metrics as package-level variables at startup, record values during operation, and expose them at the `/metrics` endpoint.

Define counters, histograms, and gauges for the key signals:

Request count (counter, labeled by method, path, status code). Request duration (histogram, labeled by method and path — use buckets appropriate for your SLAs: 0.01s, 0.05s, 0.1s, 0.5s, 1s, 2s, 5s). Active requests (gauge, incremented on receive, decremented on complete). Database query duration (histogram, labeled by operation). Cache hit/miss ratio (counter, labeled by cache name and result).

```go
var (
    httpRequestsTotal = promauto.NewCounterVec(prometheus.CounterOpts{
        Name: "http_requests_total",
        Help: "Total HTTP requests by method, path, and status",
    }, []string{"method", "path", "status"})

    httpRequestDuration = promauto.NewHistogramVec(prometheus.HistogramOpts{
        Name:    "http_request_duration_seconds",
        Help:    "HTTP request duration in seconds",
        Buckets: []float64{0.01, 0.05, 0.1, 0.5, 1, 2, 5},
    }, []string{"method", "path"})
)
```

Record metrics in middleware so all handlers are covered automatically:

```go
func metricsMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        wrapped := &statusRecorder{ResponseWriter: w, status: 200}
        next.ServeHTTP(wrapped, r)
        duration := time.Since(start).Seconds()

        httpRequestsTotal.WithLabelValues(r.Method, r.URL.Path, strconv.Itoa(wrapped.status)).Inc()
        httpRequestDuration.WithLabelValues(r.Method, r.URL.Path).Observe(duration)
    })
}
```

Expose metrics at `/metrics` using the Prometheus default handler: `http.Handle("/metrics", promhttp.Handler())`. Scrape this endpoint with a Prometheus server (or Grafana Cloud, Datadog Agent, etc.) at a 15-second interval.

## Correlating Logs, Traces, and Metrics

The three signals become most powerful when they are correlated. Include the trace ID in every log entry so you can jump from a log line to the full trace. Include the request ID in metrics labels sparingly (high cardinality labels are expensive in Prometheus).

```go
// In request middleware, after starting the span:
traceID := trace.SpanFromContext(ctx).SpanContext().TraceID().String()
reqLogger := logger.With(
    slog.String("trace_id", traceID),
    slog.String("request_id", requestID),
)
```

In Grafana, use the Loki and Tempo data sources together — click a trace ID in a log line to jump directly to the trace visualization.
