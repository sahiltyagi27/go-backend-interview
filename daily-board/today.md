# Daily Board - 2026-06-23

## Theme

```text
23 June - Course Schedule Gets Less Scary
```

## Main Goal

```text
BFS/DFS should move from "I did it once" -> "I can explain and write it again."
Course Schedule should move from "I understand it" -> "I can code it with light help."
```

## XP Dashboard

```text
Overall Career XP:        4.9 / 10 -> 5.25 / 10
Overall Fitness XP:       7.8 / 10 -> 8.2 / 10
Overall Emotional XP:     5.0 / 10 -> 5.25 / 10
Attachment Growth XP:     4.75 / 10 -> 4.9 / 10
Overall Day Target:       7.4 / 10

Today's XP Goal:          BFS/DFS recall + Course Schedule coding + Kafka speak revision
Energy:                   TBD
Confidence:               5.25 / 10 -> 5.75 / 10
```

## Career XP Dashboard

| Skill | Current | Target |
|---|---:|---:|
| Overall Career XP / Interview Readiness | 4.9 / 10 | 5.25 / 10 |
| DSA Pattern Confidence | 5.0 / 10 | 5.7 / 10 |
| Technical Communication | 4.25 / 10 | 4.6 / 10 |
| Mood / Confidence | 5.25 / 10 | 5.75 / 10 |

## Topic XP Split

| Topic | Current XP | Target XP | Today's Target |
|---|---:|---:|---|
| BFS Practice | 5.5 / 10 | 6.0 / 10 | Revised / retained |
| DFS Practice | 5.75 / 10 | 6.25 / 10 | Revised / retained |
| Graph Practice | 5.5 / 10 | 5.8 / 10 | Stronger base |
| Graph Interview | 4.0 / 10 | 4.25 / 10 | Improving explanation |
| Course Schedule Practice | 4.5 / 10 | 6.0 / 10 | Major jump |
| Course Schedule Interview | 3.75 / 10 | 4.75 / 10 | Blind code + explanation |
| Kafka Practice | 6.0 / 10 | 6.25 / 10 | Revised |
| Kafka Interview | 5.0 / 10 | 5.5 / 10 | Spoken revision |

## Board Rule

```text
No random LeetCode.
No new graph video.
No custom queue/stack.
No hard graph problems today.
No MAI replay.
Make Course Schedule less scary.
```

## Daily Scorecard

| Area | Target | Done |
|---|---:|---:|
| BFS/DFS recall | 1 | 1 |
| Course Schedule manual trace | 1 | 1 |
| Course Schedule coding | 1 | 1 |
| Kafka spoken revision | 1 | 1 |
| Night light revision | 1 | 0 |
| Fitness block | 1 | 1 |

## Morning Block - 9:30 AM to 11:00 AM

### Quest 1: BFS + DFS Recall

Steps:

1. Open blank Go file.
2. Write graph creation using `[][]int`.
3. Write BFS without looking.
4. Write DFS without looking.
5. Run both on a small graph.

Example graph:

```go
graph := [][]int{
	{1, 2},
	{3},
	{3},
	{},
}
```

Explain aloud:

```text
BFS uses a queue and explores level by level.
DFS uses recursion or stack and explores deep before backtracking.
Visited prevents cycles and repeated processing.
```

XP target:

```text
BFS Practice XP:     5.5  -> 6.0
DFS Practice XP:     5.75 -> 6.25
Graph Interview XP:  4.0  -> 4.5
DSA Confidence:      5.0  -> 5.25
```

Avoid:

```text
Do not watch a new graph video.
Do not add custom queue/stack.
Do not move to hard graph problems.
```

## Late Morning Block - 11:15 AM to 12:30 PM

### Quest 2: Course Schedule Manual Trace

Use this exact example:

```text
numCourses = 4
prerequisites = [[1,0], [2,0], [3,1], [3,2]]
```

Write manually:

```text
0 -> 1
0 -> 2
1 -> 3
2 -> 3
```

Indegree:

```text
0 = 0
1 = 1
2 = 1
3 = 2
```

Queue:

```text
[0] -> [1,2] -> [2] -> [3] -> []
```

Processed:

```text
4 courses processed -> true
```

XP target:

```text
Course Schedule Practice XP:   4.5  -> 5.0
Course Schedule Interview XP:  3.75 -> 4.0
```

## Lunch / Rest - 12:30 PM to 2:00 PM

Rules:

```text
Eat properly.
No job panic.
No MAI replay.
No LinkedIn doom scrolling.
Light walk if possible.
```

## Afternoon Block - 2:00 PM to 3:30 PM

### Quest 3: Course Schedule Code While Looking

Write full solution once while looking at notes.

Core structure:

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	indegree := make([]int, numCourses)

	for _, pre := range prerequisites {
		course := pre[0]
		prereq := pre[1]

		graph[prereq] = append(graph[prereq], course)
		indegree[course]++
	}

	queue := []int{}

	for i := 0; i < numCourses; i++ {
		if indegree[i] == 0 {
			queue = append(queue, i)
		}
	}

	processed := 0

	for len(queue) > 0 {
		course := queue[0]
		queue = queue[1:]
		processed++

		for _, next := range graph[course] {
			indegree[next]--

			if indegree[next] == 0 {
				queue = append(queue, next)
			}
		}
	}

	return processed == numCourses
}
```

Then explain:

```text
I build graph from prerequisite to course.
I count indegree as number of pending prerequisites.
Courses with zero indegree can be taken first.
Then I process them using queue and reduce indegree of dependent courses.
If all courses are processed, no cycle exists.
Otherwise, cycle exists.
```

XP target:

```text
Course Schedule Practice XP:   5.0 -> 5.5
Course Schedule Interview XP:  4.0 -> 4.5
Overall Career XP:             4.9 -> 5.1
```

## Break - 3:30 PM to 4:15 PM

```text
Tea / water / family / rest.
No new topic.
```

## Evening Career Block - 4:15 PM to 5:15 PM

### Quest 4: Kafka Revision

Only speak, no deep notes.

Answer these:

1. What is Kafka?
2. What is topic vs partition?
3. What is consumer group?
4. What is offset?
5. When should offset be committed?
6. What happens on failure?
7. Why DLQ?
8. Why idempotency?

XP target:

```text
Kafka Practice XP:        6.0  -> 6.25
Kafka Interview XP:       5.0  -> 5.5
Technical Communication:  4.25 -> 4.5
```

## Fitness Block - 7:00 PM to 8:15 PM

Warm-up:

- [x] 5-8 minutes.

Push:

- [x] Push-ups: 4 sets.
- [x] Dips: 3-4 sets.

Legs:

- [x] Sumo squats: 4 sets.
- [x] Lunges: 2 sets each leg.

Glutes:

- [x] Hip bridge: 3 sets.

Grip / Shoulders:

- [x] Dead hang: 2 rounds.

Mobility:

- [x] Hip flexor stretch.
- [x] Cat-cow.
- [x] Child's pose.
- [x] Cobra.
- [x] 8-10 minutes total.

## Fitness XP Targets

```text
Workout consistency:      maintain strong day
Steps / movement:         14k steps completed
Protein / diet control:   strong protein, 2 whey
Hydration:                3L hydration
Sleep discipline:         6.0 / 10 because Portugal match
Overall fitness day:      8.2 / 10 final
```

## Night Block - 9:15 PM to 9:45 PM

Light revision only.

Say these once:

```text
BFS = queue, level by level.
DFS = recursion/stack, deep traversal.
Kahn = BFS with indegree.
Indegree = pending prerequisites.
Kafka offset should be committed after successful processing.
DLQ stores failed messages after max retries.
```

Then stop.

```text
No new problem after 10 PM.
```

## Tomorrow's Win Conditions

Minimum win:

- [ ] BFS/DFS recall once.
- [ ] Course Schedule trace once.
- [ ] Course Schedule code once while looking.
- [ ] Kafka speak revision.
- [ ] 8k+ steps.
- [ ] Decent protein.

Strong win:

- [ ] Course Schedule coded with only light help.
- [ ] Explained Kahn's algo aloud.
- [ ] 10k+ steps.
- [ ] Workout done.
- [ ] Sleep around 11:30 PM.

## Final Target Score

```text
Career day target:      7.0 / 10
Fitness day target:     7.5 / 10
Emotional target:       7.0 / 10
Overall day target:     7.4 / 10
```

## End Of Day Summary

```text
Fitness XP logged.
Career XP logged.
Emotional Health XP logged.
Attachment / Personal Growth XP logged.
Personal Growth XP logged.
Very strong fitness day: 2 whey, creatine, 14k steps, workout, good protein, and 3L hydration.
Strong consolidation day.
BFS/DFS were revised and retained.
Course Schedule moved from scary to repeatable.
Wrote Kahn's algo correctly without looking.
Kafka revised.
Technical explanation improved.
The real win: doubted recall, challenged it, wrote correct code, and rebuilt confidence.
```

## XP Change Today

```text
Career XP:
Start: 4.9 / 10
End:   5.25 / 10
Delta: +0.35

Career skill changes:
BFS Practice:                 5.5  -> 6.0
DFS Practice:                 5.75 -> 6.25
Graph Practice:               5.5  -> 5.8
Graph Interview XP:           4.0  -> 4.25
Course Schedule Practice:     4.5  -> 6.0
Course Schedule Interview XP: 3.75 -> 4.75
DSA Pattern Confidence:       5.0  -> 5.7
Technical Communication:      4.25 -> 4.6
Kafka Practice:               6.0  -> 6.25
Kafka Interview XP:           5.0  -> 5.5
Overall Interview Readiness:  4.9  -> 5.25

Career result:
Strong consolidation day. Course Schedule moved from "I understand it" to "I can write the solution from memory."

Fitness XP:
Start: 7.8 / 10
End:   8.2 / 10
Delta: +0.4

Fitness skill changes:
Workout consistency:     6.0 -> 7.5
Steps / movement:        6.0 -> 6.5
Protein / diet control:  7.8 -> 8.5
Hydration:               7.5 -> 7.8
Sleep discipline:        6.0 -> 6.0
Overall fitness day:     7.8 -> 8.2

Fitness notes:
2 whey, creatine, 14k steps, workout, good protein, 3L hydration.
Sleep stayed flat because of Portugal match.

Emotional XP:
Start: 5.0 / 10
End:   5.25 / 10
Delta: +0.25

Emotional skill changes:
Mood stability:           5.5  -> 5.75
Rejection handling:       4.75 -> 5.0
Overthinking control:     4.5  -> 4.75
Self-respect:             5.5  -> 5.75
Emotional recovery speed: 5.25 -> 5.5
Overall emotional health: 5.0  -> 5.25

Emotional result:
Doubt was redirected into action. That created self-trust XP.

Attachment / Growth XP:
Start: 4.75 / 10
End:   4.9 / 10
Delta: +0.15

Attachment / personal growth skill changes:
External validation control:  4.25 -> 4.4
Boundary discipline:          6.0  -> 6.0
Fantasy / rumination control: 4.25 -> 4.4
Trigger response control:     4.75 -> 4.9
Letting go ability:           4.75 -> 4.9
Overall attachment control:   4.75 -> 4.9

Personal Growth XP:
Discipline:               5.5 -> 6.2
Self-awareness:           7.0 -> 7.2
Confidence rebuilding:    4.5 -> 5.25
Action over overthinking: 5.0 -> 5.75
Handling discomfort:      4.5 -> 5.25
Overall personal growth:  5.5 -> 6.0

Non-fitness day scores:
Career day score:          7.5 / 10
Emotional health score:    7.0 / 10
Personal growth score:     7.5 / 10
Attachment control score:  6.5 / 10
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
Don't chase 10 topics. Make Course Schedule less scary.
```
