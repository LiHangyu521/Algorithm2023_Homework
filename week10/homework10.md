# Homework10
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 240

#### 题目描述
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

链接：https://leetcode.cn/problems/search-a-2d-matrix-ii

#### 算法思路
1. 二分查找
2. Z字查找
#### 代码实现

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        m = len(matrix)
        if m==1 :
            n=0    
        n = len(matrix[0])

        i,j = 0,n-1 
        while i<m and j>=0:
            tmp = matrix[i][j]
            if tmp == target :
                return True
            elif tmp > target :
                j-=1
            else :
                i+=1
        return False

```
#### 运行结果
![110](https://user-images.githubusercontent.com/63528028/234437734-3f461505-5353-463f-ae40-6e383e73eea5.png)

## Leetcode 347
#### 题目描述
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

#### 算法思路
1. 小顶堆


#### 代码实现
```python
class Solution {
public:
    static bool cmp(pair<int, int>& m, pair<int, int>& n) {
        return m.second > n.second;
    }

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> occurrences;
        for (auto& v : nums) {
            occurrences[v]++;
        }

        // pair 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
        for (auto& [num, count] : occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }
        vector<int> ret;
        while (!q.empty()) {
            ret.emplace_back(q.top().first);
            q.pop();
        }
        return ret;
    }
```

## Leetcode 374

#### 题目描述
猜数字游戏的规则如下：

每轮游戏，我都会从 1 到 n 随机选择一个数字。 请你猜选出的是哪个数字。
如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。
你可以通过调用一个预先定义好的接口 int guess(int num) 来获取猜测结果，返回值一共有 3 种可能的情况（-1，1 或 0）：

-1：我选出的数字比你猜的数字小 pick < num
1：我选出的数字比你猜的数字大 pick > num
0：我选出的数字和你猜的数字一样。恭喜！你猜对了！pick == num

链接：https://leetcode.cn/problems/guess-number-higher-or-lower

#### 算法思路
###### 方法一：二分查找

#### 代码实现

```python
class Solution:
    def guessNumber(self, n: int) -> int:
        left, right = 1, n
        while left < right:
            mid = (left + right) // 2
            if guess(mid) <= 0:
                right = mid   # 答案在区间 [left, mid] 中
            else:
                left = mid + 1   # 答案在区间 [mid+1, right] 中
        
        # 此时有 left == right，区间缩为一个点，即为答案
        return left
```


#### 运行结果
![257](https://user-images.githubusercontent.com/63528028/234439107-e50bf34a-0da2-4338-8432-5aee4736d5b4.png)

