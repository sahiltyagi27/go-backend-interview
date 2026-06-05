# Design an Analytics Pipeline

## Requirements

- collect events from APIs/apps
- process high volume
- deduplicate events
- support near-real-time dashboards
- support historical queries

## High-Level Design

```text
Client/API
  |
Event Collector
  |
Kafka
  |
Consumers / Stream Processors
  |
ClickHouse / Data Warehouse
  |
Dashboards
```

## Event Shape

```json
{
  "event_id": "evt_123",
  "user_id": "u1",
  "type": "page_view",
  "timestamp": "2026-06-05T10:00:00Z",
  "properties": {
    "page": "/pricing"
  }
}
```

## Kafka

Use partitions for scale.

Partition key examples:

- `user_id` for per-user ordering
- `event_type` for type-specific processing

## Batch vs Streaming

Streaming:

- low latency
- more operational complexity

Batch:

- simpler
- higher latency
- good for daily reports

Many systems use both.

## Deduplication

Use `event_id`.

Options:

- consumer keeps recent IDs in Redis
- database has unique event ID
- stream processor deduplicates within time window

## ClickHouse

Good for:

- analytical queries
- aggregations
- high ingest volume
- columnar compression

Example query:

```sql
SELECT type, count(*)
FROM events
WHERE timestamp >= now() - INTERVAL 1 DAY
GROUP BY type;
```

## Query Performance

Improve with:

- partition by date
- order by common filters
- materialized views
- pre-aggregated tables
- retention policies

