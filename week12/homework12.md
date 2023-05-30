# Homework 12
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

## Leetcode 847
#### 题目描述
存在一个由 n 个节点组成的无向连通图，图中的节点按从 0 到 n - 1 编号。

给你一个数组 graph 表示这个图。其中，graph[i] 是一个列表，由所有与节点 i 直接相连的节点组成。

返回能够访问所有节点的最短路径的长度。你可以在任一节点开始和停止，也可以多次重访节点，并且可以重用边。


#### 算法思路
1. 广度优先搜索

由于题目需要我们求出「访问所有节点的最短路径的长度」，并且图中每一条边的长度均为 1，因此我们可以考虑使用广度优先搜索的方法求出最短路径。

在常规的广度优先搜索中，我们会在队列中存储节点的编号。对于本题而言，最短路径的前提是「访问了所有节点」，因此除了记录节点的编号以外，我们还需要记录每一个节点的经过情况。因此，我们使用三元组 (u,mask,dist) 表示队列中的每一个元素，其中 u 表示当前位于的节点编号；mask 是一个长度为 n 的二进制数，表示每一个节点是否经过。如果 mask 的第 i 位是 1，则表示节点 i 已经过，否则表示节点 i 未经过；dist 表示到当前节点为止经过的路径长度。

这样一来，我们使用该三元组进行广度优先搜索，即可解决本题。初始时，我们将所有的 (i,2^i,0) 放入队列，表示可以从任一节点开始。在搜索的过程中，如果当前三元组中的 mask 包含 n 个 1（即 mask=2n−1），那么我们就可以返回 dist 作为答案。


#### 代码实现
```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        q = deque((i, 1 << i, 0) for i in range(n))
        seen = {(i, 1 << i) for i in range(n)}
        ans = 0
        
        while q:
            u, mask, dist = q.popleft()
            if mask == (1 << n) - 1:
                ans = dist
                break
            # 搜索相邻的节点
            for v in graph[u]:
                # 将 mask 的第 v 位置为 1
                mask_v = mask | (1 << v)
                if (v, mask_v) not in seen:
                    q.append((v, mask_v, dist + 1))
                    seen.add((v, mask_v))
        
        return ans
```


