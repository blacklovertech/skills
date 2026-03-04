# Database Patterns in Go

## Choosing the Data Access Layer

**sqlc** is the recommended choice for most new projects. You write SQL queries in `.sql` files, run `sqlc generate`, and get type-safe Go functions. You control the SQL exactly — no ORM magic, no N+1 surprises, no unexpected queries. The generated code is readable and testable. Works with PostgreSQL, MySQL, and SQLite.

**pgx directly** (`github.com/jackc/pgx/v5`) is the highest-performance PostgreSQL driver. Use it with `pgxpool` for connection pooling. You write the SQL and the scanning code yourself. Choose this when you need PostgreSQL-specific features (advisory locks, LISTEN/NOTIFY, COPY) or when performance is critical and you want zero overhead.

**sqlx** (`github.com/jmoiron/sqlx`) extends the standard `database/sql` with struct scanning and named parameters. A middle ground — you write SQL, it handles the scanning boilerplate. Good for teams that know SQL but find raw `database/sql` tedious.

**GORM** (`gorm.io/gorm`) is a full ORM. It generates SQL from struct tags and provides hooks, associations, and migrations. Reduces boilerplate for simple CRUD. The generated SQL for complex queries is often inefficient. Choose GORM for prototypes or CRUD-heavy services where developer speed matters more than query control.

## Connection Pooling

Always use a connection pool. Creating a new database connection per request is expensive (~5ms overhead for PostgreSQL). A pool maintains a set of ready connections and reuses them.

For `pgxpool`, configure the pool at application startup:

```go
config, _ := pgxpool.ParseConfig(os.Getenv("DATABASE_URL"))
config.MaxConns = 25                          // max open connections
config.MinConns = 5                           // keep warm connections ready
config.MaxConnLifetime = 1 * time.Hour        // rotate connections periodically
config.MaxConnIdleTime = 30 * time.Minute     // close idle connections
config.HealthCheckPeriod = 1 * time.Minute   // verify connections periodically

pool, err := pgxpool.NewWithConfig(ctx, config)
```

Size the pool based on your database server's connection limit and your application's concurrency. A PostgreSQL server defaults to 100 max connections. With multiple app instances behind a load balancer, size each instance's pool so the total connections (instances × MaxConns) stays within the database limit. `pgbouncer` or `PgCat` as a connection pooler in front of PostgreSQL is recommended for high-concurrency production deployments.

## Transactions

Use transactions for operations that must be atomic — all succeed or all fail. Wrap the transaction in a helper that handles the commit/rollback automatically:

```go
func withTransaction(ctx context.Context, db *pgxpool.Pool, fn func(pgx.Tx) error) error {
    tx, err := db.Begin(ctx)
    if err != nil {
        return fmt.Errorf("beginning transaction: %w", err)
    }
    defer tx.Rollback(ctx) // no-op if already committed

    if err := fn(tx); err != nil {
        return err // Rollback runs via defer
    }

    if err := tx.Commit(ctx); err != nil {
        return fmt.Errorf("committing transaction: %w", err)
    }
    return nil
}
```

Call it: `err := withTransaction(ctx, pool, func(tx pgx.Tx) error { ... })`. The `defer tx.Rollback` ensures rollback on any error path, and `Rollback` on an already-committed transaction is a no-op.

## Migrations

Never apply schema changes manually to a production database. Use a migration tool that tracks applied migrations in a table and runs only the ones that have not been applied yet.

**golang-migrate** is the most widely used. It reads migration files from a directory (or embedded in the binary), keeps a `schema_migrations` table of applied versions, and applies or rolls back migrations in order. Migration files are named `001_create_users.up.sql` and `001_create_users.down.sql`.

**goose** is similar, with a slightly more ergonomic CLI. It supports both SQL and Go-based migrations (for data migrations that need application logic).

**Atlas** is a newer tool that can diff your desired schema against the current database state and generate the migration automatically. Useful for teams that prefer a declarative schema approach.

Run migrations at startup in development. In production, run migrations as a separate step in the deployment pipeline before the new application version starts receiving traffic. This enables rollback: if the migration fails, the old binary is still running against the old schema.

Always write both up and down migrations. The down migration must reverse the up migration exactly. Test the down migration in staging before relying on it for a production rollback.

## Repository Pattern

Wrap database access in a repository struct. The repository takes a database pool (or a transaction interface for testability), exposes typed methods for each operation, and translates database-specific errors into domain errors.

The repository interface is defined in the domain package or service package — not in the repository package. This allows the service to be tested with a fake repository without touching the database.

```go
// Defined in the service package (where it's used)
type UserRepository interface {
    GetByID(ctx context.Context, id string) (*User, error)
    GetByEmail(ctx context.Context, email string) (*User, error)
    Create(ctx context.Context, user *User) error
    Update(ctx context.Context, user *User) error
    Delete(ctx context.Context, id string) error
}
```

The concrete `PostgresUserRepository` implements this interface and is injected at startup. In tests, a `FakeUserRepository` backed by an in-memory map is used instead.

## Redis

Use Redis for caching, session storage, rate limiting counters, and pub/sub. The standard Go client is `github.com/redis/go-redis/v9`.

Always set TTLs on cached values — a cache without TTLs is a memory leak. Use Redis `SETNX` (set if not exists) for distributed locks with an expiry to prevent lock leaks.

For caching: use a cache-aside pattern. On read, check Redis first; if the key is missing, fetch from the database and write to Redis with a TTL. On write, write to the database, then delete the Redis key (not write — deleting avoids the race between a stale write and a fresh read). This is simpler and more correct than write-through caching.

Structure Redis keys hierarchically: `users:{id}:profile`, `products:{id}:stock`, `ratelimit:{ip}:{minute}`. This makes it easy to scan, debug, and set key expiry by prefix.
