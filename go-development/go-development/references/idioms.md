# Go Idioms and Language Patterns

## Naming Conventions

Go naming is terse by design. Names are shorter than in other languages because Go relies on package context to provide meaning. A `User` type in package `auth` is referred to as `auth.User` at the call site — there is no need to repeat the package name in the type name itself.

**Variables and functions**: use camelCase. Short names for short-lived variables: `i` for a loop index, `r` for a reader, `w` for a writer, `ctx` for a context, `err` for an error. Longer names for longer-lived variables that need more context.

**Exported names**: capitalize the first letter. `UserService`, `GetByID`, `ErrNotFound`. Unexported names are lowercase: `userStore`, `parseToken`, `errTimeout`.

**Acronyms**: keep them consistent case. `userID` not `userId`. `URLParser` not `UrlParser`. `HTTPClient` not `HttpClient`. `parseJSON` not `parseJson`. The Go standard library is consistent about this — follow it.

**Interfaces**: name interfaces for what they do, typically with an `-er` suffix: `Reader`, `Writer`, `Closer`, `Storer`, `Notifier`, `Validator`. A one-method interface named for that method is the Go ideal. `io.Reader` reads. `io.Writer` writes. Your `UserStorer` stores users.

**Receivers**: use a short, consistent abbreviation of the type name. For a `UserService`, use `us` or just `s`. For a `Repository`, use `r` or `repo`. Never use `self` or `this` — those are not Go.

**Packages**: lowercase, single-word, no underscores. `userstore` not `user_store`. `httputil` not `http_util`. The package name is part of the API — callers will type `httputil.ParseRequest`, so `http.ParseHTTPRequest` would be redundant. Avoid package names that stutter with their contents: `auth.AuthService` is bad; `auth.Service` is good.

## Interfaces — Define at the Point of Use

The single most important Go interface rule: define interfaces where they are consumed, not where they are implemented. This is the opposite of Java/C# convention and surprises many developers coming from those languages.

A handler that sends emails does not import a `EmailService` interface from the email package. It defines its own interface in the handler package:

```go
// In package handler — define exactly what this package needs
type emailSender interface {
    Send(ctx context.Context, to, subject, body string) error
}
```

Any concrete type that has a `Send` method with this signature satisfies this interface automatically — no declaration needed. This means the handler package does not import the email package at all, which keeps the dependency graph clean and makes testing trivial: write a fake `emailSender` in the test file.

Keep interfaces small. A single method is ideal. Two or three methods is fine. An interface with ten methods is almost certainly a design problem — it is either trying to do too much or it is mirroring a concrete type instead of expressing a capability.

Never define an interface for a type that has only one implementation and no test doubles needed. The abstraction costs cognitive overhead and produces no benefit. Add the interface when you have a second implementation or when you need to inject a fake in tests.

## Struct Patterns

**Constructor functions**: use a `New` function (or `NewWithOptions`) when a struct needs initialization beyond what zero values provide. Return the concrete type from constructors within the same package; return an interface when the caller should only know the capability.

```go
func NewUserService(db *pgxpool.Pool, logger *slog.Logger) *UserService {
    return &UserService{db: db, logger: logger}
}
```

**Options pattern**: for constructors with many optional parameters, use functional options rather than a long parameter list or a separate config struct. Each option is a function that modifies the struct.

```go
type Option func(*Server)

func WithTimeout(d time.Duration) Option {
    return func(s *Server) { s.timeout = d }
}

func NewServer(addr string, opts ...Option) *Server {
    s := &Server{addr: addr, timeout: 30 * time.Second} // defaults
    for _, opt := range opts {
        opt(s)
    }
    return s
}
```

**Embedding**: embed types to compose behavior. A `LoggedUserService` that embeds `UserService` adds logging behavior to every method without rewriting them. Embedding promotes the embedded type's methods to the outer type — be intentional about which methods you want promoted.

**Mutex placement**: put a `sync.Mutex` or `sync.RWMutex` immediately above the fields it protects, with a comment:

```go
type Cache struct {
    mu    sync.RWMutex // protects entries
    entries map[string]Entry
}
```

## Generics (Go 1.18+)

Use generics sparingly and only when the alternative is clear duplication or `any` (empty interface) gymnastics that lose type safety.

Good uses of generics: a `Map` function that transforms a slice, a `Filter` function, a `Result[T]` type that wraps a value or an error, a typed set implementation, generic pagination types.

Avoid generics when: the logic is genuinely different for each type (use methods or separate functions), when it makes the code harder to read, or when a simple `any` interface is clearer for the use case.

Type constraints express what operations a generic type must support. Use `comparable` for types that can be used as map keys. Use `~int | ~string` for types that have an underlying int or string. Define constraint interfaces in the same file or package where they are used — do not create a separate `constraints` package for application-level code.

## Common Patterns

**Functional options** (described above in struct patterns): the standard Go pattern for configurable types.

**Context threading**: `context.Context` is always the first parameter of any function that does I/O, starts goroutines, or needs cancellation. Never store a context in a struct — pass it through the call chain. A function that accepts a context should honor its cancellation by checking `ctx.Err()` or by passing the context to sub-calls.

**Defer for cleanup**: use `defer` to ensure resources are released even when errors occur. `defer rows.Close()` after opening database rows. `defer resp.Body.Close()` after an HTTP call. Defer runs when the enclosing function returns, not when the block ends — be aware of this in loops.

**Init functions**: avoid `func init()` in application code. It runs automatically, in an undetermined order, and makes the initialization flow hard to follow. Use explicit initialization in `main` or constructor functions instead. `init` is appropriate in packages that need to register themselves (database drivers use it legitimately with `_ "github.com/lib/pq"`).
