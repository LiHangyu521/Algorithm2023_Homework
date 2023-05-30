# Homework11
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 743

#### 题目描述
有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。

链接：https://leetcode.cn/problems/network-delay-time

#### 算法思路
1. DJSTL
#### 代码实现

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        g = [[float('inf')] * n for _ in range(n)]
        for x, y, time in times:
            g[x - 1][y - 1] = time

        dist = [float('inf')] * n
        dist[k - 1] = 0
        used = [False] * n
        for _ in range(n):
            x = -1
            for y, u in enumerate(used):
                if not u and (x == -1 or dist[y] < dist[x]):
                    x = y
            used[x] = True
            for y, time in enumerate(g[x]):
                dist[y] = min(dist[y], dist[x] + time)

        ans = max(dist)
        return ans if ans < float('inf') else -1
```
#### 运行结果
![110](https://user-images.githubusercontent.com/63528028/234437734-3f461505-5353-463f-ae40-6e383e73eea5.png)

## Leetcode 934
#### 题目描述
给你一个大小为 n x n 的二元矩阵 grid ，其中 1 表示陆地，0 表示水域。

岛 是由四面相连的 1 形成的一个最大组，即不会与非组内的任何其他 1 相连。grid 中 恰好存在两座岛 。

你可以将任意数量的 0 变为 1 ，以使两座岛连接起来，变成 一座岛 。

返回必须翻转的 0 的最小数目

链接：https://leetcode.cn/problems/shortest-bridge

#### 算法思路
1. 广度优先搜索

  题目中求最少的翻转 0 的数目等价于求矩阵中两个岛的最短距离，因此我们可以广度优先搜索来找到矩阵中两个块的最短距离。首先找到其中一座岛，然后将其不断向外延伸一圈，直到到达了另一座岛，延伸的圈数即为最短距离。

2. 深度优先+广度优先:

  解法思路与方法一类似，我们可以利用深度优先搜索求出其中的一座岛，然后利用广度优先搜索来找到两座岛的最短距离。深度度优先搜索时，我们可以将已经遍历过的位置标记为 −1



#### 代码实现
```python
class Solution:
    def shortestBridge(self, grid: List[List[int]]) -> int:
        n = len(grid)
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if v != 1:
                    continue
                q = []
                def dfs(x: int, y: int) -> None:
                    grid[x][y] = -1
                    q.append((x, y))
                    for nx, ny in (x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1):
                        if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] == 1:
                            dfs(nx, ny)
                dfs(i, j)

                step = 0
                while True:
                    tmp = q
                    q = []
                    for x, y in tmp:
                        for nx, ny in (x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1):
                            if 0 <= nx < n and 0 <= ny < n:
                                if grid[nx][ny] == 1:
                                    return step
                                if grid[nx][ny] == 0:
                                    grid[nx][ny] = -1
                                    q.append((nx, ny))
                    step += 1
```

## Leetcode 682

#### 题目描述

你现在是一场采用特殊赛制棒球比赛的记录员。这场比赛由若干回合组成，过去几回合的得分可能会影响以后几回合的得分。

比赛开始时，记录是空白的。你会得到一个记录操作的字符串列表 ops，其中 ops[i] 是你需要记录的第 i 项操作，ops 遵循下述规则：

整数 x - 表示本回合新获得分数 x
"+" - 表示本回合新获得的得分是前两次得分的总和。题目数据保证记录此操作时前面总是存在两个有效的分数。
"D" - 表示本回合新获得的得分是前一次得分的两倍。题目数据保证记录此操作时前面总是存在一个有效的分数。
"C" - 表示前一次得分无效，将其从记录中移除。题目数据保证记录此操作时前面总是存在一个有效的分数。
请你返回记录中所有得分的总和。

链接：https://leetcode.cn/problems/baseball-game

#### 算法思路
###### 方法一：模拟
思路与算法

使用变长数组对栈进行模拟。

如果操作是 +，那么访问数组的后两个得分，将两个得分之和加到总得分，并且将两个得分之和入栈。

如果操作是 D，那么访问数组的最后一个得分，将得分乘以2加到总得分，并且将得分乘以2入栈。

如果操作是 C，那么访问数组的最后一个得分，将总得分减去该得分，并且将该得分出栈。

如果操作是整数，那么将该整数加到总得分，并且将该整数入栈。


#### 代码实现

```python
class Solution:
    def calPoints(self, ops: List[str]) -> int:
        ans = 0
        points = []
        for op in ops:
            if op == '+':
                pt = points[-1] + points[-2]
            elif op == 'D':
                pt = points[-1] * 2
            elif op == 'C':
                ans -= points.pop()
                continue
            else:
                pt = int(op)
            ans += pt
            points.append(pt)
        return ans
```


#### 运行结果
![257](https://user-images.githubusercontent.com/63528028/234439107-e50bf34a-0da2-4338-8432-5aee4736d5b4.png)


