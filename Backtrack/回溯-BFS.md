# Leetcode 题解 - 回溯-BFS
<!-- GFM-TOC -->
* [Leetcode 题解 - 回溯-BFS](#leetcode-题解---回溯-BFS)
  * [回溯-BFS](#回溯-BFS)
    * [1. 01矩阵](#1-01矩阵)
    * [2. 最短的桥](#2-最短的桥)
<!-- GFM-TOC -->

## 回溯-BFS

### 1. 01矩阵

542\. 01 Matrix (Medium)

[Leetcode](https://leetcode.com/problems/01-matrix/) / [力扣](https://leetcode-cn.com/problems/01-matrix/)

```python
def updateMatrix(self, matrix: List[List[int]]) -> List[List[int]]:
    m, n = len(matrix), len(matrix[0])
    visited = [[0]*n for _ in range(m)]
    res = [[0]*n for _ in range(m)]
    queue = collections.deque()
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == 0:
                queue.append((i, j))
                visited[i][j] = 1
    step = 0
    while queue:
        size = len(queue)
        for i in range(size):
            x, y = queue.popleft()
            if matrix[x][y] == 1:
                res[x][y] = step
            for dx, dy in [(0, 1), (0, -1), (-1, 0), (1, 0)]:
                newx, newy = x + dx, y + dy
                if newx < 0 or newx >= m or newy < 0 or newy >= n or visited[newx][newy] == 1:
                    continue
                queue.append((newx, newy))
                visited[newx][newy] = 1
        step += 1
    
    return res
```

### 2. 最短的桥

934\. Shortest Bridge (Medium)

* 1. 染色：遍历矩阵，找到的一个1，调用dfs把和1联通的所有1改成2；
* 2. 寻找最短路径：调用bfs把第一个岛向周围扩散（即把它把周围的0改为2），直到在某次扩散时遇到1，说明已经遇到了另一个岛，此时返回扩散的次数即可。

[Leetcode](https://leetcode.com/problems/shortest-bridge/) / [力扣](https://leetcode-cn.com/problems/shortest-bridge/)

```python
def shortestBridge(self, A: List[List[int]]) -> int:
    # 将岛屿染色为2
    def dfs(x, y):
        if x < 0 or x >= m or y < 0 or y >= n or A[x][y] == 0 or A[x][y] == 2:
            return
        A[x][y] = 2
        queue.append((x, y))
        for dx, dy in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
            newx, newy = x + dx, y + dy
            dfs(newx, newy)
    # 寻找最小长度
    # 通过队列向外扩展到的就一定是最短路径：因为已经把第一个岛的位置全放进去了，那BFS就会一层一层的往外扩，一旦找到一个1，那么往外扩的次数就是最短的了      
    def bfs(x, y):
        step = 0
        while queue:
            size = len(queue)
            for i in range(size):
                x, y = queue.popleft()
                for dx, dy in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                    newx, newy = x + dx, y + dy
                    if newx < 0 or newx >= m or newy < 0 or newy >= n or A[newx][newy] == 2:
                        continue
                    if A[newx][newy] == 1:
                        return step
                    if A[newx][newy] == 0:
                        queue.append((newx, newy))
                        A[newx][newy] = 2
            step += 1
    
    m, n = len(A), len(A[0])
    queue = collections.deque()
    for i in range(m):
        for j in range(n):
            if A[i][j] == 1:
                dfs(i,j)
                return bfs(i,j)
```