# Daily Board - 2026-06-22

## Theme

```text
22 June - Comeback Day
```

## Excel Tracker

[interview-fitness-xp-tracker.xlsx](interview-fitness-xp-tracker.xlsx)

## XP Dashboard

```text
Overall Interview XP: 4.5 / 10 -> 5.2 / 10
Overall Fitness XP:   5.5 / 10 -> 7.0 / 10
Short-term Target:    7.0 / 10 interview readiness
Medium-term Target:   8.0 / 10 interview readiness

Today's XP Goal:      Career XP + Body XP
Streak:               0 days
Energy:               Medium
Confidence:           4 / 10 -> 6 / 10
```

## Career XP Dashboard

| Skill | Current | Target |
|---|---:|---:|
| Interview mood / confidence | 4.0 / 10 | 6.0 / 10 |
| Overall interview readiness | 4.5 / 10 | 5.2 / 10 |
| Technical communication | 4.0 / 10 | 5.0 / 10 |
| System design thinking | 4.0 / 10 | Maintain |

## Topic XP Split

| Topic | Practice XP | Interview XP | Today's Target |
|---|---:|---:|---|
| Go concurrency | 5.0 / 10 | 4.0 / 10 | Maintain |
| Worker pool | 6.0 / 10 | 4.5 / 10 | Maintain |
| Rate limiter | 6.0 / 10 | 5.0 / 10 | Maintain |
| DSA overall | 4.0 / 10 | 3.5 / 10 | +Practice, +Interview |
| Graph basics | 3.0 / 10 | 3.0 / 10 | +Practice, +Interview |
| Course Schedule / Kahn's algo | 4.0 / 10 | 3.5 / 10 | +Practice, +Interview |
| Min Stack | 3.0 / 10 | 3.0 / 10 | +Practice, +Interview |
| Kafka | 6.0 / 10 | 5.0 / 10 | +Interview |
| MongoDB | 3.0 / 10 | 2.5 / 10 | Maintain |
| Distributed tracing | 3.0 / 10 | 2.5 / 10 | Maintain |
| Incident communication | 3.5 / 10 | 3.0 / 10 | Maintain |
| Node.js | 2.5 / 10 | 2.0 / 10 | Maintain |

## Fitness XP Targets

| Fitness Area | Start | Target |
|---|---:|---:|
| Workout consistency | 6.0 / 10 | 7.0 / 10 |
| Steps / movement | 5.5 / 10 | Maintain |
| Protein / diet control | 6.0 / 10 | 7.0 / 10 |
| Hydration | 6.0 / 10 | 7.0 / 10 |
| Sleep discipline | 4.5 / 10 | 6.5 / 10 |
| Overall fitness day | 5.5 / 10 | 7.0 / 10 |

## Board Rule

```text
No random LeetCode.
No MAI overthinking.
Only pattern practice.
Farm XP.
Career XP + Body XP.
Honest XP, not cruel XP.
```

## Daily Scorecard

| Area | Target | Done |
|---|---:|---:|
| Coding drill | 3 | 0 |
| Concept revision | 4 | 0 |
| Spoken explanation | 5 | 0 |
| Project / production answer | 2 | 0 |
| Rest blocks respected | 5 | 0 |
| Fitness blocks respected | 4 | 0 |

## Fitness Rules Today

Hydration:

- [ ] 2.5-3L water total.
- [ ] Extra water if walking/workout.
- [ ] Electral only if heat/sweating is high.

Food:

- [ ] Keep protein strong.
- [ ] No random snacking after dinner.
- [ ] Milk/whey if protein is low.

Workout:

- [ ] 7:00-8:30 PM normal workout.
- [ ] No ego lifting.
- [ ] Focus on consistency and form.

Steps:

- [ ] Target 8k-10k if weather allows.
- [ ] Indoor walking is okay.
- [ ] Do not force outdoor walking in heat.

Sleep:

- [ ] 11 PM target.
- [ ] No heavy screen after 10:30.

## Schedule

### 12:00-12:15 - Reset + Setup

What:

- [x] Reply to MAI HR.
- [ ] Drink water.
- [ ] Open VS Code.
- [ ] Start fresh.

How:

```text
Send polite closing reply.
Do not reread feedback again.
Keep phone away after reply.
Open one Go file for graph practice.
```

### 12:15-1:00 - Graph Basics: BFS + DFS

What:

- [ ] Graph representation.
- [ ] BFS code once.
- [ ] DFS code once.

How:

```text
1. Create graph:
   graph := make([][]int, n)

2. Add edges:
   graph[u] = append(graph[u], v)

3. BFS:
   - Use queue
   - Mark visited when pushing
   - Pop from front
   - Visit neighbours

4. DFS:
   - Use recursion
   - Mark visited
   - Recursively visit unvisited neighbours
```

Say aloud:

> BFS uses queue and goes level by level. DFS uses recursion or stack and goes deep before backtracking.

### 1:00-2:00 - Lunch + Rest

What:

- [ ] Eat.
- [ ] Rest eyes.
- [ ] No job stress.

How:

```text
No LeetCode.
No LinkedIn scrolling.
No MAI replay.
Sit or lie down 20-30 min after lunch.
```

### 2:00-2:45 - Course Schedule: Kahn's Algo

What:

- [ ] Revise topological sort.
- [ ] Code Course Schedule once.
- [ ] Explain aloud once.

How:

```text
1. Build graph:
   prereq -> course

2. Build indegree:
   indegree[course]++

3. Add all indegree 0 courses to queue.

4. Process queue:
   - Pop course
   - processed++
   - For each next course:
     indegree[next]--
     If indegree[next] == 0, push to queue

5. Return:
   processed == numCourses
```

Say aloud:

> This is dependency resolution. If all nodes are processed, no cycle. If not, a cycle exists.

### 2:45-3:00 - Break

How:

```text
Leave desk.
Drink water.
Look far away / rest eyes.
No phone scrolling.
```

### 3:00-3:45 - Min Stack

What:

- [ ] Learn two-stack approach.
- [ ] Code once.
- [ ] Explain aloud once.

How:

```text
1. mainStack stores all values.
2. minStack stores current minimum at every level.

Push:
- Push value to mainStack.
- Push min(value, currentMin) to minStack.

Pop:
- Pop from both stacks.

Top:
- Return top of mainStack.

GetMin:
- Return top of minStack.
```

Say aloud:

> Min Stack uses an auxiliary stack so getMin is O(1).

### 3:45-4:15 - Break / Walk / Eye Rest

How:

```text
Walk inside home.
Stretch neck/shoulders.
No coding.
No job applications.
```

### 4:15-5:00 - Kafka Revision

What:

- [ ] Topic / partition / consumer group.
- [ ] Offset.
- [ ] Retry.
- [ ] DLQ.
- [ ] Idempotency.

How:

Write and speak these answers:

```text
1. Kafka basics:
Producer writes to topic. Topic has partitions. Consumers read from partitions. Consumer groups divide partitions among consumers.

2. Offset:
Offset is the position of a message inside a partition.

3. Commit:
I commit offset only after successful processing.

4. Retry:
If processing fails, retry with backoff.

5. DLQ:
After max retries, send to DLQ and then commit original offset.

6. Idempotency:
At-least-once delivery can create duplicates, so consumers should be idempotent using event IDs or unique constraints.
```

### 5:00-5:30 - Technical Communication

What:

- [ ] Explain one project aloud.
- [ ] Explain one incident answer.
- [ ] Practice senior structure.

How:

Use this structure every time:

```text
Problem -> Approach -> Tradeoff -> Production concern
```

Project explanation:

```text
1. What was the project?
2. What problem did it solve?
3. What was your role?
4. What tech stack?
5. What was difficult?
6. What was the impact?
```

Incident answer:

```text
1. Stop bleeding: rollback/disable feature.
2. Assess impact.
3. Communicate status.
4. Give ETA/workaround.
5. Share RCA later.
6. Add prevention.
```

### 5:30-6:30 - Rest

How:

```text
No guilt.
No MAI thoughts.
Relax properly.
```

### 7:00-8:30 - Workout

How:

```text
Normal workout.
No ego lifting.
Focus on consistency + form.
Do not overdo if mentally tired.
Focus on consistency, not PR.
```

### 8:30-9:15 - Dinner / Shower

How:

```text
Eat properly.
No interview stress during dinner.
```

### 9:15-9:45 - Light Revision Only

What:

- [ ] BFS vs DFS.
- [ ] Kahn's algo.
- [ ] Kafka offset/DLQ.
- [ ] Min Stack.

How:

```text
No coding.
Only read notes and speak answers aloud.
```

Say:

```text
BFS = queue, level by level.
DFS = recursion/stack, deep traversal.
Kahn = indegree + queue.
Kafka offset commit after success.
Min Stack = main stack + min stack.
```

### 10:30-11:00 - Wind Down

How:

```text
No LeetCode.
No job portals.
No MAI feedback reading.
Sleep mode.
```

### 11:00 - Sleep Target

- [ ] Sleep.

## Current Done

- [x] Added distributed tracing notes.
- [x] Added incident communication notes.
- [x] Added interview readiness summary.
- [x] Created XP-based daily board.

## Carry Forward

- [ ] Push current `go-backend-interview` notes when ready.
- [ ] Brevo project explanation refinement.
- [ ] MongoDB deeper prep later.

## End Of Day Summary

```text
Fill this at the end of the day.
```

## XP Change Today

```text
Career XP:
Start: 4.5 / 10
End:   TBD
Delta: TBD

Career skill changes:
Mood / confidence:           4.0 -> TBD
Overall interview readiness: 4.5 -> TBD
System design thinking:      4.0 -> TBD
Technical communication:     4.0 -> TBD

Topic XP split changes:
Go concurrency:              practice 5.0 -> TBD, interview 4.0 -> TBD
Worker pool:                 practice 6.0 -> TBD, interview 4.5 -> TBD
Rate limiter:                practice 6.0 -> TBD, interview 5.0 -> TBD
DSA overall:                 practice 4.0 -> TBD, interview 3.5 -> TBD
Graph basics:                practice 3.0 -> TBD, interview 3.0 -> TBD
Course Schedule / Kahn:      practice 4.0 -> TBD, interview 3.5 -> TBD
Min Stack:                   practice 3.0 -> TBD, interview 3.0 -> TBD
Kafka:                       practice 6.0 -> TBD, interview 5.0 -> TBD
MongoDB:                     practice 3.0 -> TBD, interview 2.5 -> TBD
Distributed tracing:         practice 3.0 -> TBD, interview 2.5 -> TBD
Incident communication:      practice 3.5 -> TBD, interview 3.0 -> TBD
Node.js:                     practice 2.5 -> TBD, interview 2.0 -> TBD

Fitness XP:
Start: 5.5 / 10
End:   TBD
Delta: TBD

Fitness skill changes:
Workout consistency:         6.0 -> TBD
Steps / movement:            5.5 -> TBD
Protein / diet control:      6.0 -> TBD
Hydration:                   6.0 -> TBD
Sleep discipline:            4.5 -> TBD
Overall fitness day:         5.5 -> TBD
```

## XP Drop Check

```text
No drop for planned rest.
No drop for one emotionally hard day if one useful action happened.

Drops only apply for:
- neglect
- failed recall
- random panic learning
- repeated skipped fitness basics
```

## What Felt Weak

```text
Fill this after practice.
```

## Tomorrow's First Task

```text
Pick one task before ending the day.
```
