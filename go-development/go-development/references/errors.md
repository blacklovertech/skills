# Error Handling in Go

## The Core Mental Model

In Go, errors are values — they are returned from functions like any other return value. There is no exception mechanism, no try-catch, no stack unwinding on failure. This is a deliberate design choice. It means every function that can fail says so explicitly in its signature, and every caller must decide what to do with that failure.

This approach has a real cost: more lines of code at every error site. It has a larger benefit: error handling is explicit, local, and traceable. When you read a Go function, you know exactly where it can fail and what happens when it does. There are no hidden failure paths.

## The Cardinal Rules

Never ignore an error silently. The blank identifier `_` discards a value — using it to discard an error is almost always a bug. The one exception is when ignoring is genuinely correct and you can state why. In that case, write a comment:

```go
_ = logger.Sync() // best-effort flush on shutdown; ignore error
```

Anything else — if you are tempted to write `result, _ := doSomething()` because "it probably won't fail" — that is the bug that will page you at 3am.

Always add context when wrapping. A raw error like `sql: no rows in result set` gives the reader no information about what the code was trying to do. Wrap it:

```go
user, err := repo.GetUser(ctx, id)
if err != nil {
    return fmt.Errorf("loading user %d for checkout: %w", id, err)
}
```

Now the stack of wrapped errors reads like a story: `loading user 42 for checkout: getting user from db: sql: no rows in result set`. Each layer adds the context it knows.

## Wrapping and Unwrapping

Use `fmt.Errorf` with the `%w` verb to wrap an error. This preserves the original error as an unwrappable cause. Use `errors.Is(err, target)` to check if a wrapped error chain contains a specific sentinel error. Use `errors.As(err, &target)` to extract a specific typed error from the chain.

```go
// Define a sentinel error
var ErrNotFound = errors.New("not found")

// Wrap it deeper in the stack
return fmt.Errorf("getting product %s: %w", id, ErrNotFound)

// Check for it at the top level — works even through wrapping
if errors.Is(err, ErrNotFound) {
    http.Error(w, "product not found", http.StatusNotFound)
    return
}
```

Never check errors by comparing their string message. `strings.Contains(err.Error(), "not found")` breaks the moment the underlying error message changes. Use `errors.Is` or `errors.As` instead.

## Sentinel Errors vs Typed Errors

**Sentinel errors** are package-level error variables. They express a condition that callers need to branch on. Keep them in the same package as the code that produces them. Name them with an `Err` prefix: `ErrNotFound`, `ErrUnauthorized`, `ErrConflict`.

```go
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
)
```

**Typed errors** are structs that implement the `error` interface. Use them when the caller needs to extract data from the error — not just know that something went wrong, but know what specifically went wrong.

```go
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation failed on %s: %s", e.Field, e.Message)
}

// At the call site:
var valErr *ValidationError
if errors.As(err, &valErr) {
    // respond with the specific field that failed
}
```

Use typed errors for: validation failures (which field, what rule), HTTP errors (status code, message, request ID), database constraint violations (which constraint).

## Error Handling at Layer Boundaries

Each architectural layer has a different job with errors:

The **repository layer** converts database-specific errors (pgx errors, GORM errors) into domain errors. A `pgx.ErrNoRows` becomes `ErrNotFound`. A unique constraint violation becomes `ErrConflict`. Repository callers should not import pgx or GORM just to understand errors — they should only need the domain sentinels.

The **service layer** wraps errors with business context. It knows what the user was trying to do: `fmt.Errorf("placing order for user %d: %w", userID, err)`. It does not translate errors further — it adds the business operation context.

The **handler layer** translates domain errors into HTTP responses. It maps `ErrNotFound` to 404, `ErrConflict` to 409, `ErrUnauthorized` to 401. It logs unexpected errors (anything that is not a known domain sentinel) with the full wrapped chain, and returns a generic 500 response to the client without leaking internal details.

## Panic — When and Only When

Panic is for programmer errors: nil pointer dereferences, index out of bounds, failed type assertions on values that "can't" be the wrong type. It signals that the program has reached an impossible state.

Never use panic for runtime errors: network failures, invalid user input, database errors, file not found. These are expected failures in a long-running service. Return them as errors.

Recover from panics only at the top-level boundary — an HTTP middleware that catches panics to prevent a single handler crash from killing the whole process. Log the panic with a full stack trace, record it in Sentry, and return a 500 response. Do not use recover to implement error handling logic — that is what error returns are for.

```go
func recoveryMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        defer func() {
            if rec := recover(); rec != nil {
                slog.Error("panic recovered", "panic", rec, "stack", debug.Stack())
                http.Error(w, "internal server error", http.StatusInternalServerError)
            }
        }()
        next.ServeHTTP(w, r)
    })
}
```
