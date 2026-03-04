# Testing in Go

## Table-Driven Tests — The Go Standard

Table-driven tests are the idiomatic Go way to test a function against multiple inputs. Define a slice of test case structs, give each a name, and loop over them with `t.Run`. When a specific case fails, the test output names the exact case that failed.

```go
func TestCalculateDiscount(t *testing.T) {
    tests := []struct {
        name     string
        price    float64
        quantity int
        want     float64
    }{
        {"no discount under threshold", 100, 1, 0},
        {"10 percent at 5 items", 100, 5, 10},
        {"20 percent at 10 items", 100, 10, 20},
        {"capped at 30 percent for large orders", 100, 100, 30},
    }

    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            got := CalculateDiscount(tc.price, tc.quantity)
            assert.Equal(t, tc.want, got)
        })
    }
}
```

Naming test cases is important. "case 1" tells you nothing when it fails. "no discount under threshold" tells you exactly what invariant is broken.

## Test File Placement and Package Names

Place test files alongside the code they test. `user_service.go` and `user_service_test.go` live in the same directory.

Use `package service_test` (the `_test` suffix) for black-box tests — tests that only access the package's exported API. This is the stronger test: if the external interface works correctly, the implementation is correct. It also forces you to design a clean public API.

Use `package service` (no suffix) for white-box tests that need access to unexported identifiers. Use this sparingly — testing internals couples the test to the implementation and makes refactoring harder.

## testify

`github.com/stretchr/testify` is the near-universal testing helper in Go. Use two subpackages:

`assert` for non-fatal checks. The test continues even if an assertion fails, so you see all failures in one run.

`require` for fatal checks. The test stops immediately on failure. Use `require` when subsequent assertions would panic or be meaningless if the current check fails (e.g. `require.NoError(t, err)` before using the value that `err` would invalidate).

```go
user, err := service.GetUser(ctx, "user-123")
require.NoError(t, err)          // stop here if error — next line would panic
assert.Equal(t, "Alice", user.Name)
assert.Equal(t, "alice@example.com", user.Email)
assert.False(t, user.Deleted)
```

## Mocks and Fakes

Mocking in Go is straightforward because of interface-based dependency injection. A mock is simply a struct that implements an interface with controlled behavior. Write fakes by hand for simple interfaces; use `mockery` to generate mocks for larger interfaces.

A hand-written fake for a simple repository:

```go
type fakeUserRepo struct {
    users map[string]*User
    err   error // inject an error for error path tests
}

func (f *fakeUserRepo) GetByID(_ context.Context, id string) (*User, error) {
    if f.err != nil {
        return nil, f.err
    }
    u, ok := f.users[id]
    if !ok {
        return nil, ErrNotFound
    }
    return u, nil
}
```

In the test:

```go
repo := &fakeUserRepo{users: map[string]*User{"1": {ID: "1", Name: "Alice"}}}
service := NewUserService(repo, slog.Default())
user, err := service.GetUser(ctx, "1")
require.NoError(t, err)
assert.Equal(t, "Alice", user.Name)
```

To test the error path:

```go
repo := &fakeUserRepo{err: ErrNotFound}
_, err := service.GetUser(ctx, "999")
assert.ErrorIs(t, err, ErrNotFound)
```

Never mock types you do not own (standard library, third-party packages). Test against the real thing, or use a real running dependency in an integration test.

## Integration Tests

Integration tests run against real dependencies — a real PostgreSQL database, a real Redis instance. Use `testcontainers-go` to spin up Docker containers for the test run, or use a test database with a known state.

Tag integration tests with a build tag so they do not run in the fast unit test pass:

```go
//go:build integration

package repository_test
```

Run integration tests separately: `go test -tags integration ./...`. Run them in CI on every pull request, but in a separate job that can be parallelized and cached separately from unit tests.

Use `TestMain` to set up and tear down shared test infrastructure (start the database container, run migrations, create a connection pool) once for the entire test package, rather than in each test function.

## Benchmarks

Write benchmarks for performance-critical code. A benchmark function is named `BenchmarkXxx` and receives `*testing.B`.

```go
func BenchmarkParseToken(b *testing.B) {
    token := generateTestToken()
    b.ResetTimer() // exclude setup time from the measurement
    for b.N > 0 {
        b.N--
        _, err := ParseToken(token)
        if err != nil {
            b.Fatal(err)
        }
    }
}
```

Run with `go test -bench=. -benchmem ./...`. The `-benchmem` flag shows memory allocations per operation, which often reveals optimization opportunities. Compare benchmarks across commits with `benchstat` to detect regressions.

## Test Coverage

Run `go test -coverprofile=coverage.out ./...` and `go tool cover -html=coverage.out` to see which lines are not covered. Aim for high coverage on business logic (service layer). Lower coverage on handlers is acceptable if they are thin and tested through integration tests. Do not aim for 100% coverage — it leads to tests that test implementation details rather than behavior.

Coverage is a metric, not a goal. Untested code that is obviously correct is better than covered code tested badly.
