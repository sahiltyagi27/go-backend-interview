# Grid DFS: Surrounded Cities

Interview question:

> Find the number of cities surrounded by villages. `0` denotes village, `1` denotes city. A city must be surrounded on all 4 sides. Ignore diagonals.

The first thing to clarify:

> Are we counting individual city cells, or connected city components?

That clarification changes the solution.

## Version A: Individual City Cells

If every `1` is an individual city, DFS is not needed. Scan every cell and check only the four direct neighbors.

For `grid[r][c] == 1`, check:

```text
grid[r-1][c] == 0
grid[r+1][c] == 0
grid[r][c-1] == 0
grid[r][c+1] == 0
```

Boundary cells usually do not count because they cannot be surrounded on all four sides, unless the interviewer says outside the grid counts as village.

```go
func CountSurroundedCities(grid [][]int) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	rows := len(grid)
	cols := len(grid[0])
	count := 0
	dirs := [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			if grid[r][c] != 1 {
				continue
			}

			surrounded := true
			for _, d := range dirs {
				nr := r + d[0]
				nc := c + d[1]

				if nr < 0 || nr >= rows || nc < 0 || nc >= cols || grid[nr][nc] != 0 {
					surrounded = false
					break
				}
			}

			if surrounded {
				count++
			}
		}
	}

	return count
}
```

Complexity:

```text
Time: O(rows * cols)
Space: O(1)
```

## Version B: Connected City Components

If connected `1`s form one city region, use DFS or BFS.

Example:

```text
0 0 0 0
0 1 1 0
0 0 0 0
```

Here `1 1` is one city component. Count it once if the entire component is surrounded by villages and does not touch the boundary.

DFS idea:

- Traverse connected `1`s using only four directions.
- Track visited cells.
- If the component touches the boundary, it is not surrounded.
- Villages around the component are fine.
- Diagonals do not matter.

```go
func CountSurroundedCityComponents(grid [][]int) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	rows := len(grid)
	cols := len(grid[0])
	visited := make([][]bool, rows)
	for i := range visited {
		visited[i] = make([]bool, cols)
	}

	dirs := [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

	var dfs func(r, c int) bool
	dfs = func(r, c int) bool {
		visited[r][c] = true
		isSurrounded := true

		if r == 0 || r == rows-1 || c == 0 || c == cols-1 {
			isSurrounded = false
		}

		for _, d := range dirs {
			nr := r + d[0]
			nc := c + d[1]

			if nr < 0 || nr >= rows || nc < 0 || nc >= cols {
				isSurrounded = false
				continue
			}

			if grid[nr][nc] == 0 {
				continue
			}

			if grid[nr][nc] == 1 && !visited[nr][nc] {
				if !dfs(nr, nc) {
					isSurrounded = false
				}
			}
		}

		return isSurrounded
	}

	count := 0
	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			if grid[r][c] == 1 && !visited[r][c] && dfs(r, c) {
				count++
			}
		}
	}

	return count
}
```

Complexity:

```text
Time: O(rows * cols)
Space: O(rows * cols)
```

## Interview Answer

> If we count individual city cells, a simple scan with four-neighbor checks is enough. If we count connected city regions, I will use DFS or BFS over four directions, track visited cells, reject components that touch the boundary, and count the component only if all external neighbors are villages.
