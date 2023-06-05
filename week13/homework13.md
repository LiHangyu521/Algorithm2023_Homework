# Homework 13
###### Name: 李杭禹
###### StudentID: 2201212834

## 判断素数

#### 题目描述
提供一个算法复杂度比 $O(\sqrt{n})$要低的判断素数的方法，并给出算法复杂度分析过程。

#### 算法思路
1. 遍历试除+筛选
#### 代码实现

```C
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
