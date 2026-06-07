# Capacity Planning and Architecture Decisions

## Capacity Planning

Capacity planning estimates whether a system can handle expected traffic, storage, and cost.

Estimate:

- daily active users
- requests per second
- read/write ratio
- storage per day
- bandwidth
- peak multiplier
- background job volume

Example:

```text
10M DAU
Each user opens feed 10 times/day
100M feed reads/day
Average RPS = 100M / 86400 ~= 1157 RPS
Peak RPS = 5x average ~= 5800 RPS
```

## Storage Estimate

```text
1M posts/day
Each post metadata ~= 1KB
1GB/day metadata
365GB/year metadata
```

Media should go to object storage, not the relational DB.

## Cost Estimation

Think about:

- compute
- database
- cache
- queue
- object storage
- network egress
- observability/log volume

Interview line:

> Capacity estimates do not need to be perfect; they guide whether the architecture is reasonable.

## Architecture Decision Records

An ADR records important technical decisions.

Template:

```text
Title
Context
Decision
Alternatives considered
Consequences
```

Example:

```text
Decision: Use Redis sorted sets for first-page feed serving.
Reason: Low latency feed reads and simple ordering by score.
Tradeoff: Redis is not the source of truth, so DB fallback is needed.
```

## Well-Architected Thinking

Evaluate designs using these pillars:

- operational excellence
- security
- reliability
- performance efficiency
- cost optimization

## Tradeoff Language

Useful senior phrases:

- "This improves read latency but increases write amplification."
- "This makes the system more available but eventually consistent."
- "This reduces operational complexity but may limit scale."
- "This adds cost, so I would only introduce it after measuring bottlenecks."

## Interview Line

> Senior engineers do not just pick technologies. They explain why a design fits the requirements, what tradeoffs it creates, and how they would validate it in production.

