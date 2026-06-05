# SQL, Indexes, Transactions, and Locking

## Joins

Joins combine rows from multiple tables.

```sql
SELECT p.id, p.body, u.name
FROM posts p
JOIN users u ON u.id = p.author_id
WHERE p.author_id = $1;
```

## Constraints

Constraints protect data correctness.

```sql
email TEXT UNIQUE NOT NULL
author_id BIGINT REFERENCES users(id)
amount NUMERIC CHECK (amount > 0)
```

## Indexes

Indexes speed up reads by maintaining an extra searchable structure.

Most common DB index:

> B-tree index

Good for:

- equality lookup
- range queries
- sorting by indexed columns

Composite index:

```sql
CREATE INDEX idx_posts_author_created
ON posts(author_id, created_at DESC);
```

Index order matters. `(author_id, created_at)` helps:

```sql
WHERE author_id = ?
ORDER BY created_at DESC
```

But may not help:

```sql
WHERE created_at > ?
```

## When Indexes Hurt

Indexes slow writes because every insert/update/delete must update the index.

Too many indexes cause:

- slower writes
- more disk usage
- more memory pressure

## Transactions

ACID:

- Atomicity: all or nothing
- Consistency: valid state
- Isolation: concurrent transactions do not corrupt each other
- Durability: committed data survives failure

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

On error:

```sql
ROLLBACK;
```

## Isolation Levels

| Issue | Meaning |
|---|---|
| Dirty read | read uncommitted data |
| Non-repeatable read | same row changes during transaction |
| Phantom read | new matching rows appear during transaction |

Common isolation levels:

- Read committed
- Repeatable read
- Serializable

## Locking

Pessimistic locking:

```sql
SELECT * FROM orders WHERE id = $1 FOR UPDATE;
```

Optimistic locking:

```sql
UPDATE orders
SET status = 'PAID', version = version + 1
WHERE id = $1 AND version = $2;
```

If zero rows updated, someone else changed it first.

## Deadlocks

Deadlock happens when transactions wait on each other forever.

Reduce risk:

- access rows in consistent order
- keep transactions short
- retry deadlock errors

