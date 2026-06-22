# Vanilla Go Interview Code

Use this for live coding rounds.

Goal:

```text
No custom Stack type.
No custom Queue type.
No custom Set type.
Use slices and maps.
```

## Default Rules

```text
Stack      -> []T, push append, pop from end
Queue      -> []T, push append, pop from front
Set        -> map[T]bool
0..n nodes -> [][]int + []bool
String IDs -> map[string][]string + map[string]bool
Random IDs -> map[int][]int + map[int]bool
```

## Stack With Slice

```go
stack := []int{}

stack = append(stack, 10)      // push
top := stack[len(stack)-1]     // top
stack = stack[:len(stack)-1]   // pop
_ = top
```

Use for:

```text
Valid Parentheses
DFS iterative
Monotonic stack
Min Stack
```

## Queue With Slice

```go
queue := []int{start}

for len(queue) > 0 {
	node := queue[0]
	queue = queue[1:]

	for _, next := range graph[node] {
		queue = append(queue, next)
	}
}
```

Use for:

```text
BFS
Kahn's algorithm
Level-order traversal
Shortest path in unweighted graph
```

## Set With Map

```go
visited := map[int]bool{}

visited[node] = true

if visited[next] {
	continue
}
```

For `0..n-1` nodes, prefer:

```go
visited := make([]bool, n)
```

## Graph Representation

Numbered nodes:

```go
graph := make([][]int, n)
visited := make([]bool, n)
```

Directed edge:

```go
graph[u] = append(graph[u], v)
```

Undirected edge:

```go
graph[u] = append(graph[u], v)
graph[v] = append(graph[v], u)
```

Random integer IDs:

```go
graph := map[int][]int{}
visited := map[int]bool{}
```

String nodes:

```go
graph := map[string][]string{}
visited := map[string]bool{}
```

## BFS: Numbered Nodes

```go
func BFS(start int, graph [][]int) []int {
	visited := make([]bool, len(graph))
	queue := []int{start}
	visited[start] = true

	order := []int{}

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		order = append(order, node)

		for _, next := range graph[node] {
			if visited[next] {
				continue
			}
			visited[next] = true
			queue = append(queue, next)
		}
	}

	return order
}
```

## DFS Recursive: Numbered Nodes

```go
func DFS(node int, graph [][]int, visited []bool, order *[]int) {
	if visited[node] {
		return
	}

	visited[node] = true
	*order = append(*order, node)

	for _, next := range graph[node] {
		DFS(next, graph, visited, order)
	}
}
```

Call:

```go
visited := make([]bool, len(graph))
order := []int{}
DFS(0, graph, visited, &order)
```

## DFS Iterative: Numbered Nodes

```go
func DFSIterative(start int, graph [][]int) []int {
	visited := make([]bool, len(graph))
	stack := []int{start}
	order := []int{}

	for len(stack) > 0 {
		node := stack[len(stack)-1]
		stack = stack[:len(stack)-1]

		if visited[node] {
			continue
		}

		visited[node] = true
		order = append(order, node)

		for i := len(graph[node]) - 1; i >= 0; i-- {
			stack = append(stack, graph[node][i])
		}
	}

	return order
}
```

## BFS: String Nodes

```go
func BFSString(start string, graph map[string][]string) []string {
	visited := map[string]bool{start: true}
	queue := []string{start}
	order := []string{}

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		order = append(order, node)

		for _, next := range graph[node] {
			if visited[next] {
				continue
			}
			visited[next] = true
			queue = append(queue, next)
		}
	}

	return order
}
```

## DFS Recursive: String Nodes

```go
func DFSString(start string, graph map[string][]string) []string {
	visited := map[string]bool{}
	order := []string{}

	var dfs func(string)
	dfs = func(node string) {
		if visited[node] {
			return
		}

		visited[node] = true
		order = append(order, node)

		for _, next := range graph[node] {
			dfs(next)
		}
	}

	dfs(start)
	return order
}
```

## Course Schedule: Kahn's Algorithm

```go
func CanFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	indegree := make([]int, numCourses)

	for _, p := range prerequisites {
		course := p[0]
		prereq := p[1]

		graph[prereq] = append(graph[prereq], course)
		indegree[course]++
	}

	queue := []int{}
	for course := 0; course < numCourses; course++ {
		if indegree[course] == 0 {
			queue = append(queue, course)
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

Interview line:

```text
Course Schedule is topological sort. I process nodes with indegree 0. If all
courses are processed, no cycle exists. Otherwise, a cycle blocks completion.
```

## Min Stack

```go
type MinStack struct {
	values []int
	mins   []int
}

func Constructor() MinStack {
	return MinStack{}
}

func (s *MinStack) Push(val int) {
	s.values = append(s.values, val)

	if len(s.mins) == 0 || val < s.mins[len(s.mins)-1] {
		s.mins = append(s.mins, val)
		return
	}

	s.mins = append(s.mins, s.mins[len(s.mins)-1])
}

func (s *MinStack) Pop() {
	s.values = s.values[:len(s.values)-1]
	s.mins = s.mins[:len(s.mins)-1]
}

func (s *MinStack) Top() int {
	return s.values[len(s.values)-1]
}

func (s *MinStack) GetMin() int {
	return s.mins[len(s.mins)-1]
}
```

## Valid Parentheses

```go
func IsValid(s string) bool {
	pairs := map[rune]rune{
		')': '(',
		']': '[',
		'}': '{',
	}

	stack := []rune{}

	for _, ch := range s {
		if ch == '(' || ch == '[' || ch == '{' {
			stack = append(stack, ch)
			continue
		}

		if len(stack) == 0 || stack[len(stack)-1] != pairs[ch] {
			return false
		}

		stack = stack[:len(stack)-1]
	}

	return len(stack) == 0
}
```

## Two Sum

```go
func TwoSum(nums []int, target int) []int {
	seen := map[int]int{}

	for i, n := range nums {
		need := target - n
		if j, ok := seen[need]; ok {
			return []int{j, i}
		}
		seen[n] = i
	}

	return nil
}
```

## Longest Substring Without Repeating Characters

```go
func LengthOfLongestSubstring(s string) int {
	lastSeen := map[byte]int{}
	left := 0
	best := 0

	for right := 0; right < len(s); right++ {
		ch := s[right]

		if last, ok := lastSeen[ch]; ok && last >= left {
			left = last + 1
		}

		lastSeen[ch] = right

		if right-left+1 > best {
			best = right - left + 1
		}
	}

	return best
}
```

## Binary Array Sort

```go
func SortBinaryArray(nums []int) {
	left := 0

	for right := 0; right < len(nums); right++ {
		if nums[right] == 0 {
			nums[left], nums[right] = nums[right], nums[left]
			left++
		}
	}
}
```

## Final Interview Default

```text
Stack problem       -> []T
Queue/BFS problem   -> []T
Set/visited problem -> map[T]bool or []bool
Course Schedule     -> [][]int + []int indegree
Min Stack           -> two []int stacks
Two Sum             -> map[int]int
Sliding window      -> map[byte]int + left pointer
```

