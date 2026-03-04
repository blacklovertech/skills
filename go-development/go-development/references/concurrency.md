# Concurrency in Go

## Start With: Does This Need to Be Concurrent?

Before reaching for goroutines, ask whether concurrency actually helps. Sequential code is easier to read, test, and debug. Concurrency is appropriate when:

There is genuine parallelism to exploit — CPU-bound work that can run simultaneously on multiple cores. Decompress ten files at once; parse ten JSON blobs in parallel.

There is I/O blocking to overlap — making five API calls that each take 200ms sequentially takes 1000ms; making them concurrently takes ~200ms.

There is a producer-consumer separation that simplifies the design — a goroutine reads from a message queue while another processes items, decoupled by a channel.

If none of these apply, sequential code is the right choice.

## Goroutine Ownership and Lifecycle

Every goroutine must have a clear owner — a function or struct responsible for ensuring the goroutine terminates. Goroutines that are started and forgotten are leaks. They accumulate over time, consume memory, and eventually exhaust resources.

The pattern for a goroutine with a defined lifetime:

Start the goroutine with a context that can be cancelled. The goroutine selects on the context's Done channel alongside its actual work. When the context is cancelled, the goroutine returns. The owner waits for the goroutine to finish with a WaitGroup before proceeding (or before the program exits).

```go
func (w *Worker) Start(ctx context.Context) {
    var wg sync.WaitGroup
    wg.Add(1)
    go func() {
        defer wg.Done()
        for {
            select {
            case <-ctx.Done():
                return
            case job := <-w.jobs:
                w.process(ctx, job)
            }
        }
    }()
    // To stop: cancel the context, then wg.Wait()
}
```

For a pool of goroutines doing the same work, use a worker pool pattern: a fixed number of goroutines, each reading from a shared input channel, writing results to an output channel. Cap the pool size at `runtime.GOMAXPROCS(0)` for CPU-bound work; size it empirically for I/O-bound work.

## Context for Cancellation

`context.Context` is Go's mechanism for propagating cancellation signals and deadlines across goroutine boundaries. Pass it as the first argument to every function that does I/O or starts goroutines.

`context.WithTimeout` creates a context that automatically cancels after a duration — use it for all outbound HTTP calls and database queries. A call to a slow external API without a timeout can block your goroutine indefinitely.

`context.WithCancel` creates a context you cancel manually — use it to cancel a group of goroutines when the parent decides the work is no longer needed (e.g. the user cancelled a request).

`context.WithValue` stores a value in the context — use it sparingly, only for request-scoped values that are genuinely cross-cutting (request ID, trace ID, authenticated user). Do not use it as a back-channel for passing regular function parameters.

## Channels — Ownership and Direction

Channels transfer ownership of data between goroutines. The goroutine that creates a channel owns it and is responsible for closing it when sending is complete. Only the sender closes a channel; closing from the receiver side causes a panic if the sender tries to send again.

Specify channel direction in function signatures. A function that produces values and sends them to a channel takes a `chan<- T`. A function that consumes values takes a `<-chan T`. This makes the data flow explicit and prevents accidental misuse.

Buffered channels decouple senders and receivers. A send to a buffered channel does not block until the buffer is full. Use buffering when you know the producer will briefly outpace the consumer and you want to avoid blocking. Do not use large buffers to paper over a systemic throughput mismatch — that hides the real problem.

Closing a channel signals that no more values will be sent. Receiving from a closed channel immediately returns the zero value with `ok == false`. This is how you signal completion to a consumer:

```go
// Producer
go func() {
    defer close(results)
    for _, item := range items {
        results <- process(item)
    }
}()

// Consumer
for result := range results { // loop ends when channel is closed
    handleResult(result)
}
```

## Shared State — Mutexes

When goroutines share a struct and need to read and modify its fields, protect the fields with a `sync.RWMutex`. Use `RLock`/`RUnlock` for read-only access (allows concurrent readers) and `Lock`/`Unlock` for write access (exclusive).

Keep the locked section minimal — do only the work that requires mutual exclusion, then release the lock. Never call external functions or do I/O while holding a lock. Holding a lock across an I/O call is a common source of deadlocks and latency spikes.

```go
func (c *Cache) Get(key string) (string, bool) {
    c.mu.RLock()
    defer c.mu.RUnlock()
    v, ok := c.entries[key]
    return v, ok
}

func (c *Cache) Set(key, value string) {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.entries[key] = value
}
```

## The Race Detector

The Go race detector finds data races at runtime — concurrent read and write of the same memory without synchronization. It is essential. Enable it with `-race`:

```
go test -race ./...
go run -race main.go
```

The race detector adds overhead (roughly 5–10x slowdown, significant memory increase) so it is not used in production binaries. It is run in CI on every commit. Any race detector finding is a critical bug — do not ship code with known races.

## Common Concurrency Mistakes

**Goroutine leaks**: goroutines started in request handlers that run past the request lifetime, goroutines that block on a channel that is never sent to, goroutines waiting on a context that is never cancelled. Use `goleak` in tests to detect leaked goroutines.

**Closing a channel from the receiver**: panics at runtime when the sender tries to write. Always close from the sender.

**Sending to a closed channel**: panics at runtime. Use a `sync.Once` or a done channel to ensure a channel is closed exactly once.

**Loop variable capture**: in Go 1.21 and earlier, closures in loops capture the loop variable by reference, not by value. By Go 1.22, range loop variables are per-iteration. For pre-1.22 code, copy the variable: `item := item` before the goroutine launch.

**Nil channel blocks forever**: sending to or receiving from a nil channel blocks forever without a panic. This can be used intentionally (to disable a case in a select) but is usually a bug when unintentional.
