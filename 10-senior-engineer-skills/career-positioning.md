# Resume, Project Storytelling, and Communication

## Resume Positioning

A senior resume should show impact, ownership, and technical depth.

Weak:

```text
Worked on backend APIs.
```

Stronger:

```text
Designed and implemented order retry flow using idempotency keys, reducing duplicate payment incidents and improving recovery from provider timeouts.
```

## Project Storytelling

Use this structure:

```text
Context
Problem
Constraints
Decision
Tradeoffs
Impact
What I would improve next
```

Example:

```text
In my Instagram clone, feed reads were the most important path.
I started with direct DB reads, then designed a fan-out-on-write feed model.
For celebrity accounts, I would use fan-out-on-read to avoid massive write spikes.
The tradeoff is that reads become more complex, but writes stay manageable.
```

## Business Impact / ROI

Senior engineers connect technical work to business value.

Examples:

- reduced latency
- reduced infra cost
- improved conversion
- reduced incidents
- improved developer productivity
- enabled new product feature

## Communication

Good senior communication:

- explain tradeoffs clearly
- ask clarifying questions
- write design docs
- align with product/business constraints
- give status updates early
- call out risks before they become incidents

## Interview Project Explanation

For each project, prepare:

- what it does
- architecture
- database schema
- important APIs
- scaling bottleneck
- failure handling
- security/auth
- what you would improve

## Salary Negotiation

Basic principles:

- know market range
- avoid giving first number if possible
- negotiate based on value and competing options
- consider total compensation, not just base salary
- be polite and specific

## Interview Line

> I try to explain projects in terms of problem, constraints, tradeoffs, and measurable impact, not just technologies used.

