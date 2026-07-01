# Backend Interview Spaced Revision System

Main line:

```text
We do not chase topics anymore. We rotate them until they become interview memory.
```

## Why This Exists

The old pattern was:

```text
Learn once -> forget -> panic before interview
```

The new pattern is:

```text
Learn -> revise after 1 day -> revise after 3 days -> revise weekly -> mock -> interview-ready
```

Interview feedback is still useful, but it should feed the system instead of becoming the whole system.

## Revision Cycle

Every important topic follows this cycle:

| Stage | Timing | Goal |
|---|---|---|
| Day 0 | Learn / practice | Understand and write notes/code |
| Day 1 | 10-minute recall | Confirm it did not disappear |
| Day 3 | Re-code or re-explain | Move from familiar to repeatable |
| Day 7 | Mock interview answer | Practice pressure + speaking |
| Day 14 | Mixed revision | Recall it among other topics |
| Day 30 | Retention check | Confirm long-term interview memory |

## Daily Plan Structure

Every normal prep day must include:

1. New learning
2. Old topic revision
3. Spoken interview answer

Example:

| Block | Example |
|---|---|
| New topic | SQL joins |
| Old revision | Course Schedule + Min Stack |
| Speaking | Kafka offset / DLQ / idempotency |

Next day:

| Block | Example |
|---|---|
| New topic | MongoDB indexes |
| Old revision | SQL join + Course Schedule |
| Speaking | Go goroutine / channel / context |

## Rolling Revision Tracker

Use this table as the source of truth.

| Topic | Current XP | Last Revised | Next Revision | Revision Type | Status | Next Action |
|---|---:|---|---|---|---|---|
| Course Schedule / Kahn's Algorithm | 6.5 | 2026-06-24 | 2026-06-29 | code + explain | okay | Write once without looking |
| Min Stack | 4.75 | 2026-06-24 | 2026-06-29 | code | okay | Recode two-slice version |
| Valid Anagram | 4.5 | 2026-06-26 | 2026-06-29 | code | weak | Write lowercase array version |
| SQL JOIN + CREATE VIEW | 2.0 | 2026-06-26 | 2026-06-29 | code/query | weak | Write join + view query |
| Normalization vs denormalization | 4.0 | 2026-06-26 | 2026-06-29 | explain | weak | Speak comparison |
| Replication advantages | 4.0 | 2026-06-26 | 2026-06-29 | explain | weak | Speak availability/failover/read scaling |
| Go goroutines / channels / context | 6.0 | 2026-06-26 | 2026-06-30 | explain | okay | 5-minute spoken answer |
| Worker pool | 6.0 | 2026-06-22 | 2026-06-30 | code + explain | needs recall | Re-read and explain |
| Node event loop / libuv | 3.0 | 2026-06-26 | 2026-06-30 | explain | weak | Speak safe Node answer |
| Kafka offset / retry / DLQ / idempotency | 6.25 | 2026-06-24 | 2026-06-30 | explain | okay | 8-question revision |
| MongoDB indexes | 4.0 | 2026-06-26 | 2026-07-01 | explain | weak | Explain index + compound index |
| Redis caching | 5.0 | 2026-06-21 | 2026-07-01 | explain | needs recall | Cache-aside + invalidation |
| REST API design | 5.0 | 2026-06-26 | 2026-07-01 | explain | okay | Status codes + idempotency |
| Incident management | 4.5 | 2026-06-22 | 2026-07-02 | mock | needs recall | Problem -> impact -> mitigation -> RCA |
| Observability | 4.0 | 2026-06-22 | 2026-07-02 | explain | needs recall | Logs vs metrics vs traces |
| Project explanation | 5.0 | 2026-06-24 | 2026-07-02 | mock | needs recall | Problem -> design -> role -> result |

Status meanings:

| Status | Meaning |
|---|---|
| strong | Can explain/code under pressure |
| okay | Can recall with light effort |
| needs recall | Not recently touched |
| rusty | Struggled or delayed recall |
| weak | Known gap |

Revision types:

| Type | Meaning |
|---|---|
| code | Write implementation/query |
| explain | Speak answer aloud |
| mock | Interview-style timed answer |
| notes | Read short note and summarize |

## Frequency Buckets

### Daily For Now

These are fragile and important. Touch them for only 10-15 minutes total:

- Course Schedule / Kahn's Algorithm
- Min Stack
- Valid Anagram
- SQL JOIN + CREATE VIEW
- Go goroutine / channel / context
- Node event loop

### Every 2-3 Days

- Kafka basics
- MongoDB indexes
- Redis cache
- REST API design
- Incident handling
- Normalization / replication

### Weekly

- Docker / deployment
- ClickHouse
- System design
- Observability
- Cloud migration stories
- Resume project stories

## XP Rules

If a topic is not touched:

| Untouched time | XP action |
|---|---|
| 3 days | No drop, mark `needs recall` |
| 5-7 days | -0.25 XP |
| 10+ days | -0.5 XP |
| 14+ days | -1.0 XP |

If revised successfully:

| Revision result | XP action |
|---|---|
| Quick recall | +0.25 |
| Write/explain without looking | +0.5 |
| Mock-style answer | +0.5 to +1.0 |
| Failed recall but corrected | +0.25 recovery XP |

## Busy Day Minimum

Even on family/travel days, do 20 minutes if possible:

| Time | Topic |
|---:|---|
| 5 min | Course Schedule memory hook |
| 5 min | SQL JOIN / VIEW |
| 5 min | Go concurrency |
| 5 min | One backend concept aloud |

If life genuinely blocks it, no shame. Resume the cycle the next day.

## Do Not Allow

```text
No major resume/backend topic should go untouched for more than 7 days during active interview preparation.
```

## Daily Board Template Add-On

Add this to daily plans:

```text
New learning:
Old revision:
Spoken interview answer:
Next spaced revision updates:
```

## Lifestyle XP Add-On

From July onward, daily EOD updates should include:

```text
Career XP
Interview Prep XP
Project Building XP
Fitness XP
Hydration / Diet XP
Emotional Recovery XP
Marriage / Family XP
Stock Market XP
PC Gaming XP
Overall Day XP
```

Stock market and gaming are not automatically bad. They are judged by discipline and timing.

Main rule:

```text
Stock market is wealth-building.
Gaming is recovery.
Neither should become escape mode.
```

Stock Market Slot:

```text
Time: 3:30 PM - 4:00 PM
Limit: 20-30 minutes
Allowed: portfolio check, dividend tracking, watchlist update, one company/news review
Not allowed: random tips, panic buy/sell, intraday/revenge trading
```

PC Gaming Reward Slot:

```text
Time: 8:30 PM - 9:30 PM
Limit: 45-75 minutes
Allowed only after minimum win:
- 10 jobs applied
- SQL / DSA / Go minimum touched
- project or prep block done
- hydration decent

Reward options:
- FC 26: 1-2 matches
- ETS2: 1 delivery / route

Hard stop:
- Normal day: 75 min max
- Strong day: 90 min max
- Weak day: 20-30 min recovery only
```
