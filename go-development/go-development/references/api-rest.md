# REST API Patterns in Go

## Choosing a Router / Framework

**`net/http` (Go 1.22+)**: Go 1.22 added method and path parameter support to the standard library router: `mux.HandleFunc("GET /users/{id}", handler)`. For simple services with few routes and no complex middleware chains, this is the right choice — zero dependencies, maximum simplicity.

**Chi** (`github.com/go-chi/chi`): a lightweight router that is fully compatible with `net/http` interfaces. Adds clean middleware composition, URL parameter extraction, route grouping, and subrouters. The right choice for services that need structured middleware chains without a full framework.

**Gin** (`github.com/gin-gonic/gin`): a full framework with its own context type, binding/validation built in, a larger middleware ecosystem, and strong performance. The right choice when the team values ergonomics and the larger ecosystem, and is comfortable with the framework's opinions.

**Echo** (`github.com/labstack/echo`): similar ergonomics to Gin, slightly different API. Choose based on team familiarity; they are largely equivalent for most use cases.

The choice between Chi, Gin, and Echo is mostly a matter of team preference. The structural patterns below apply to all of them.

## Handler Structure

Handlers must be thin. A handler's responsibilities are precisely: parse the request (read body, extract path params, validate input), call a service method, write the response. If a handler contains business logic, that logic belongs in a service.

The dependency injection pattern makes handlers testable. Accept interface dependencies in a handler struct rather than calling package-level functions or using global variables.

```go
type UserHandler struct {
    service UserService // interface, not concrete type
    logger  *slog.Logger
}

func (h *UserHandler) GetUser(w http.ResponseWriter, r *http.Request) {
    id := chi.URLParam(r, "id")
    
    user, err := h.service.GetUser(r.Context(), id)
    if err != nil {
        h.handleError(w, r, err)
        return
    }
    
    writeJSON(w, http.StatusOK, user)
}
```

The `handleError` method maps domain errors to HTTP responses — it lives on the handler struct and encapsulates the translation logic for the entire handler group.

## Request Validation

Always validate input at the handler layer before it reaches the service. Use `go-playground/validator` for struct-tag-based validation, or validate manually for complex rules. Return a structured error response — never let an invalid-input panic propagate.

Parse the request body once, decode it into a typed struct, validate the struct, then pass the typed value to the service. Never pass `http.Request` to the service layer.

```go
type CreateUserRequest struct {
    Email    string `json:"email" validate:"required,email"`
    Name     string `json:"name" validate:"required,min=2,max=100"`
    Password string `json:"password" validate:"required,min=8"`
}

func (h *UserHandler) CreateUser(w http.ResponseWriter, r *http.Request) {
    var req CreateUserRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        writeError(w, http.StatusBadRequest, "invalid request body")
        return
    }
    if err := validate.Struct(req); err != nil {
        writeValidationError(w, err)
        return
    }
    // pass typed req to service
}
```

## Middleware

Middleware wraps handlers to add cross-cutting behavior. In Chi, middleware is applied with `r.Use(...)`. In Gin, with `router.Use(...)`. Order matters — middleware runs in the order it is added.

Standard middleware stack for a production service, in order:

Recovery (panic → 500 response), request ID injection (generate a UUID, set it in context and response header), structured logging (log request method/path/status/duration with the request ID), authentication (validate JWT, set user in context), rate limiting, CORS (if needed).

Write middleware that extracts a request ID from the `X-Request-ID` header (or generates one) and stores it in the context. All subsequent log entries for that request should include the request ID, enabling correlation across distributed services.

## Timeouts

Set timeouts on both the server and every outbound call. The defaults are dangerous.

Server timeouts (set on `http.Server`): `ReadTimeout` (time to read the full request including body, typically 15s), `WriteTimeout` (time to write the full response, typically 30s), `IdleTimeout` (time to keep idle connections alive, typically 60s). `ReadHeaderTimeout` separately from `ReadTimeout` protects against Slowloris attacks.

Outbound HTTP clients: always configure `Timeout` on the `http.Client`. A shared client with a timeout is created once and reused — never create an `http.Client` per request (it does not reuse connections). `5 * time.Second` is a reasonable default for internal services; `30 * time.Second` for slow external APIs.

Database: pass a context with timeout to every database call. `context.WithTimeout(ctx, 5*time.Second)` for queries, `30*time.Second` for transactions.

## Response Patterns

Write consistent JSON responses. Define helper functions that all handlers use — never let handlers construct raw JSON strings.

```go
func writeJSON(w http.ResponseWriter, status int, data any) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    json.NewEncoder(w).Encode(data)
}

type ErrorResponse struct {
    Error     string `json:"error"`
    RequestID string `json:"request_id,omitempty"`
}

func writeError(w http.ResponseWriter, status int, msg string) {
    writeJSON(w, status, ErrorResponse{Error: msg})
}
```

Map domain errors to HTTP status codes in a single place — the handler's `handleError` method or a shared `mapError` function. `ErrNotFound` → 404, `ErrConflict` → 409, `ErrUnauthorized` → 401, `ErrForbidden` → 403, `ValidationError` → 422. Everything else → 500. Never expose internal error messages in 500 responses to clients.

## Route Registration

Register routes explicitly and visibly. A function that takes a router and registers all the routes for a handler group makes the route map easy to find:

```go
func (h *UserHandler) RegisterRoutes(r chi.Router) {
    r.Route("/users", func(r chi.Router) {
        r.Use(h.authMiddleware)
        r.Get("/", h.ListUsers)
        r.Post("/", h.CreateUser)
        r.Get("/{id}", h.GetUser)
        r.Put("/{id}", h.UpdateUser)
        r.Delete("/{id}", h.DeleteUser)
    })
}
```

Call each handler's `RegisterRoutes` from `main.go` where the router is constructed. This makes the full route table visible in one place.
