# Graceful Shutdown

Graceful shutdown means the service stops safely.

Steps:

1. Stop accepting new requests.
2. Let in-flight requests finish.
3. Cancel background workers.
4. Flush queues/logs if needed.
5. Close DB/Kafka/Redis clients.

## HTTP Server Example

```go
srv := &http.Server{
    Addr:    ":8080",
    Handler: router,
}

go func() {
    if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
        log.Fatal(err)
    }
}()

quit := make(chan os.Signal, 1)
signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
<-quit

ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
defer cancel()

if err := srv.Shutdown(ctx); err != nil {
    log.Println("shutdown error:", err)
}
```

## Background Workers

Use context cancellation.

```go
func worker(ctx context.Context, jobs <-chan Job) {
    for {
        select {
        case job, ok := <-jobs:
            if !ok {
                return
            }
            process(job)
        case <-ctx.Done():
            return
        }
    }
}
```

## Common Mistakes

- closing channels from the receiver side
- not using timeout for shutdown
- forgetting background goroutines
- accepting new traffic while shutting down
- not closing DB/Redis/Kafka clients

