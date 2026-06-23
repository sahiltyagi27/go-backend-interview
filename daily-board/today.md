# Daily Board - 2026-06-24

## Theme

```text
24 June - DSA Consolidation + Backend Interview Communication
```

## Main Goal

```text
Course Schedule should become repeatable.
Kafka should become speakable.
Min Stack should become easy.
Technical communication should improve.
```

## XP Dashboard

```text
Overall Career XP:        5.25 / 10 -> 5.5 / 10
Overall Fitness XP:       recovery + mobility day, 7.0 / 10 effort target
Overall Emotional XP:     target 7.0 / 10 day score
Attachment Growth XP:     maintain
Personal Growth XP:       target 7.5 / 10 day score
Overall Day Target:       7.5 / 10

Today's XP Goal:          lock Course Schedule + add Min Stack + speak Kafka/project clearly
Energy:                   TBD
Confidence:               5.75 / 10 -> 6.0 / 10
```

## Career XP Dashboard

| Skill | Current | Target |
|---|---:|---:|
| Overall Career XP / Interview Readiness | 5.25 / 10 | 5.5 / 10 |
| DSA Pattern Confidence | 5.7 / 10 | 6.0 / 10 |
| Technical Communication | 4.6 / 10 | 5.0 / 10 |
| Mood / Confidence | 5.75 / 10 | 6.0 / 10 |

## Topic XP Split

| Topic | Current XP | Target XP | Today's Target |
|---|---:|---:|---|
| Course Schedule Practice | 6.0 / 10 | 6.5 / 10 | Write without looking |
| Course Schedule Interview | 4.75 / 10 | 5.25 / 10 | Explain cleanly |
| Min Stack Practice | 3.0 / 10 | 4.5 / 10 | Two-slice implementation |
| Min Stack Interview | 3.0 / 10 | 3.75 / 10 | Explain O(1) minimum |
| Kafka Practice | 6.25 / 10 | 6.5 / 10 | Speak revision |
| Kafka Interview | 5.5 / 10 | 5.75 / 10 | Smooth answers |

## Board Rule

```text
Do not chase novelty.
Do not start hard graph problems.
Do not change to custom queue.
Do not look at Course Schedule solution first.
No MAI replay.
Convert today's progress into interview memory.
```

## Daily Scorecard

| Area | Target | Done |
|---|---:|---:|
| Course Schedule repeat | 1 | 0 |
| Min Stack implementation | 1 | 0 |
| Kafka spoken revision | 1 | 0 |
| Project explanation practice | 1 | 0 |
| Night light revision | 1 | 0 |
| Fitness block | 1 | 0 |

## Morning Block - 9:30 AM to 11:00 AM

### Quest 1: Course Schedule Repeat

Steps:

1. Open blank Go file.
2. Write `canFinish` without looking.
3. Use vanilla Go only.
4. Run with 2 examples.

Example 1, possible:

```go
numCourses := 4
prerequisites := [][]int{{1,0}, {2,0}, {3,1}, {3,2}}
```

Expected:

```text
true
```

Example 2, cycle:

```go
numCourses := 2
prerequisites := [][]int{{1,0}, {0,1}}
```

Expected:

```text
false
```

Say aloud:

```text
I build graph from prerequisite to course.
Indegree means pending prerequisites.
Zero indegree courses enter the queue.
Processing a course unlocks dependent courses.
If processed count equals total courses, no cycle exists.
```

XP target:

```text
Course Schedule Practice XP:    6.0  -> 6.5
Course Schedule Interview XP:   4.75 -> 5.25
DSA Confidence:                 5.7  -> 5.9
```

Avoid:

```text
Do not look at solution first.
Do not start hard graph problem.
Do not change to custom queue.
```

## Late Morning Block - 11:15 AM to 12:15 PM

### Quest 2: Min Stack

This is the easy DSA confidence builder.

Steps:

1. Write Min Stack using two slices.
2. Main stack stores values.
3. Min stack stores minimum so far.
4. Implement `Push`, `Pop`, `Top`, and `GetMin`.

Memory rule:

```text
Push value to main stack.
Push current minimum to min stack.
Pop from both.
GetMin is top of min stack.
```

XP target:

```text
Min Stack Practice XP:       3.0 -> 4.5
Min Stack Interview XP:      3.0 -> 3.75
DSA Confidence:              5.9 -> 6.0
```

## Lunch / Rest - 12:15 PM to 2:00 PM

```text
Eat properly.
No doom scrolling.
No MAI replay.
No "one month wasted" thought loop.
Light walk if possible.
```

## Afternoon Block - 2:00 PM to 3:15 PM

### Quest 3: Kafka Speaking Revision

No long notes. Speak answers.

Questions:

1. What is Kafka?
2. Topic vs partition?
3. Consumer group?
4. Offset?
5. When to commit offset?
6. What if processing fails?
7. Retry vs DLQ?
8. Why idempotency?
9. Where is ordering guaranteed?

Strong answer focus:

```text
Offset commit after successful processing.
Retry with backoff.
After max retries, send to DLQ.
Use idempotency because duplicate processing is possible.
Ordering is guaranteed only within a partition.
```

XP target:

```text
Kafka Practice XP:        6.25 -> 6.5
Kafka Interview XP:       5.5  -> 5.75
Technical Communication:  4.6  -> 4.8
```

## Break - 3:15 PM to 4:00 PM

```text
Tea / water / family / rest.
No new topic.
```

## Evening Career Block - 4:00 PM to 5:15 PM

### Quest 4: Project Explanation Practice

This is important for actual interviews.

Speak one project from your resume in this structure:

1. Problem
2. System/design
3. Your contribution
4. Scale/performance/production issue
5. Tradeoff
6. Result

Template:

```text
In one of my backend projects, the problem was ______.
We designed it using ______.
My responsibility was ______.
One challenge was ______.
I handled it by ______.
The result was ______.
```

XP target:

```text
Technical Communication:      4.8  -> 5.0
Overall Interview Readiness:  5.25 -> 5.5
Mood / Confidence:            5.75 -> 6.0
```

## Fitness Block - Recovery + Mobility + Light Pull

### Why This Plan

```text
Sunday:  Pull-ups + core + glutes
Monday:  Pull-ups + core + glutes + sumo squats + lunges
Tuesday: Bar dips + push-ups + core + glutes + 20 kg sumo squats

Tomorrow should not repeat Tuesday.
Recovery is part of the XP system.
Goal: consistency, waist reduction, strength progression, and keeping hips/back happy.
```

Effort target:

```text
7 / 10 effort day.
Do not chase another 8.5 / 10 workout.
```

Workout:

- [ ] Wide-grip pull-ups with band: 3-4 sets.
- [ ] Dead hangs: 2 rounds.
- [ ] Bulgarian split squats: 2-3 sets each leg.
- [ ] Hip mobility work: 10 minutes.
- [ ] Cat-cow.
- [ ] Cobra.
- [ ] Child's pose.
- [ ] Hip flexor stretch.
- [ ] Easy walk.

Avoid:

```text
No push-ups.
No dips.
No heavy sumo squat repeat.
No ego workout.
No repeating Tuesday just because energy feels good.
```

Fitness target:

```text
Workout consistency:       maintain 7+
Steps / movement:          easy walk, 8k+ if natural
Protein / diet control:    7+
Hydration:                 7+
Sleep discipline:          6.5+
Overall fitness day:       7.0-7.5 target
```

## Night Block - 9:15 PM to 9:40 PM

Light revision only.

Say these once:

```text
Kahn = graph + indegree + zero queue + unlock + processed count.
Min Stack = main stack + min stack.
Kafka = topic, partition, consumer group, offset, retry, DLQ, idempotency.
```

Then stop.

```text
No new coding after 10 PM.
```

## Win Conditions

Minimum win:

- [ ] Course Schedule written once without looking.
- [ ] Min Stack written once.
- [ ] Kafka spoken once.
- [ ] One project explanation spoken once.
- [ ] Recovery fitness block completed.

Strong win:

- [ ] Course Schedule written cleanly without looking.
- [ ] Min Stack correct.
- [ ] Kafka answers smooth.
- [ ] Project explanation clear.
- [ ] Recovery fitness block completed without ego lifting.
- [ ] Sleep around 11:30 PM.

## Final Target Score

```text
Career day target:          7.5 / 10
Fitness day target:         7.0-7.5 / 10
Emotional health target:    7.0 / 10
Personal growth target:     7.5 / 10
Overall day target:         7.5 / 10
```

## End Of Day Summary

```text
Fill this at EOD.
```

## XP Change Today

```text
Career XP:
Start: 5.25 / 10
End:   TBD
Delta: TBD

Fitness XP:
Start: 8.2 / 10
End:   TBD
Delta: TBD

Emotional XP:
Start: 5.25 / 10
End:   TBD
Delta: TBD

Attachment / Growth XP:
Start: 4.9 / 10
End:   TBD
Delta: TBD
```

## XP Drop Check

```text
Did any skill get rusty, avoided, or fail recall today?

No drop for planned rest.
No drop for one emotionally hard day if one useful action happened.

Drops:
-0.25 = minor rust / distracted prep
-0.50 = failed recall or skipped important planned task
-1.00 = blank on previously known topic / repeated neglect
```

## Main Line

```text
Don't chase novelty. Convert today's progress into interview memory.
```
