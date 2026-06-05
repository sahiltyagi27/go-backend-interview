# Extra Algorithm Patterns To Add Later

These are not required by the pasted backend checklist, but they are useful interview extras.

## Trapping Rain Water

Total water trapped across all bars.

Formula at each index:

```text
min(maxLeft, maxRight) - height[i]
```

Best solution:

- two pointers
- O(n) time
- O(1) space

## Container With Most Water

Choose two bars that maximize:

```text
min(height[left], height[right]) * (right - left)
```

Move the shorter pointer inward.

## Topological Sort

Used for dependency ordering.

Examples:

- course schedule
- build systems
- task dependencies

## Union Find

Used for connectivity problems.

Examples:

- connected components
- cycle detection in undirected graph
- accounts merge

## LRU Cache

Usually implemented with:

- hash map for O(1) lookup
- doubly linked list for O(1) recency update

