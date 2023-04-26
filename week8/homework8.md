# Homework8
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 110

#### 题目描述
给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。

示例 1：

输入：root = [3,9,20,null,null,15,7]
输出：true

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/balanced-binary-tree

#### 算法思路
###### 方法一：自顶向下的递归
定义函数 height，用于计算二叉树中的任意一个节点 p 的高度：
```math
\operatorname{height}(p)=\left\{\begin{array}{ll}
0 & p \text { 是空节点 } \\
\max (\operatorname{height}(\text { p.left }), \text { height }(\text { p.right }))+1 & p \text { 是非空节点 }
\end{array}\right.
```
有了计算节点高度的函数，即可判断二叉树是否平衡。具体做法类似于二叉树的前序遍历，即对于当前遍历到的节点，首先计算左右子树的高度，如果左右子树的高度差是否不超过1，再分别递归地遍历左右子节点，并判断左子树和右子树是否平衡。这是一个自顶向下的递归的过程。
###### 方法二：自底向上的递归
方法一由于是自顶向下递归，因此对于同一个节点，函数 height 会被重复调用，导致时间复杂度较高。如果使用自底向上的做法，则对于每个节点，函数 
height只会被调用一次。
自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 −1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。

#### 代码实现

```python
class Solution:
    ##Solution1
    def isBalanced(self, root: TreeNode) -> bool:
        def height(root: TreeNode) -> int:
            if not root:
                return 0
            return max(height(root.left), height(root.right)) + 1

        if not root:
            return True
        return abs(height(root.left) - height(root.right)) <= 1 and self.isBalanced(root.left) and self.isBalanced(root.right)
```

```python
class Solution:
    ##Solution2
    def isBalanced(self, root: TreeNode) -> bool:
        def height(root: TreeNode) -> int:
            if not root:
                return 0
            leftHeight = height(root.left)
            rightHeight = height(root.right)
            if leftHeight == -1 or rightHeight == -1 or abs(leftHeight - rightHeight) > 1:
                return -1
            else:
                return max(leftHeight, rightHeight) + 1

        return height(root) >= 0
```
#### 运行结果
![110](https://user-images.githubusercontent.com/63528028/234437734-3f461505-5353-463f-ae40-6e383e73eea5.png)

## Leetcode 64
#### 题目描述
给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

```
示例 1：
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

链接：https://leetcode.cn/problems/minimum-path-sum

#### 算法思路
「147. 对链表进行插入排序」要求使用插入排序的方法对链表进行排序，插入排序的时间复杂度是 O(n2)，其中 n 是链表的长度。这道题考虑时间复杂度更低的排序算法。题目的进阶问题要求达到 O(nlogn) 的时间复杂度和 O(1) 的空间复杂度，时间复杂度是 O(nlogn) 的排序算法包括归并排序、堆排序和快速排序（快速排序的最差时间复杂度是 O(n2)），其中最适合链表的排序算法是归并排序。
归并排序基于分治算法。最容易想到的实现方式是自顶向下的递归实现，考虑到递归调用的栈空间，自顶向下归并排序的空间复杂度是 O(logn)。如果要达到 O(1) 的空间复杂度，则需要使用自底向上的实现方式。
###### 方法一：动态规划

由于路径的方向只能是向下或向右，因此网格的第一行的每个元素只能从左上角元素开始向右移动到达，网格的第一列的每个元素只能从左上角元素开始向下移动到达，此时的路径是唯一的，因此每个元素对应的最小路径和即为对应的路径上的数字总和。

对于不在第一行和第一列的元素，可以从其上方相邻元素向下移动一步到达，或者从其左方相邻元素向右移动一步到达，元素对应的最小路径和等于其上方相邻元素与其左方相邻元素两者对应的最小路径和中的最小值加上当前元素的值。由于每个元素对应的最小路径和与其相邻元素对应的最小路径和有关，因此可以使用动态规划求解。

创建二维数组 dp，与原始网格的大小相同，dp[i][j]表示从左上角出发到 (i,j) 位置的最小路径和。显然，dp[0][0]=grid[0][0]。对于 dp 中的其余元素，通过以下状态转移方程计算元素值。

* 当 i>0 且j=0 时，dp[i][0]=dp[i−1][0]+grid[i][0]。

* 当 i=0i=0 且 j>0j>0 时，dp[0][j]=dp[0][j−1]+grid[0][j]。

* 当 i>0i>0 且 j>0j>0 时，dp[i][j]=min(dp[i−1][j],dp[i][j−1])+grid[i][j]。

最后得到 dp[m−1][n−1] 的值即为从网格左上角到网格右下角的最小路径和。


#### 代码实现
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return 0
        
        rows, columns = len(grid), len(grid[0])
        dp = [[0] * columns for _ in range(rows)]
        dp[0][0] = grid[0][0]
        for i in range(1, rows):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        for j in range(1, columns):
            dp[0][j] = dp[0][j - 1] + grid[0][j]
        for i in range(1, rows):
            for j in range(1, columns):
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
        
        return dp[rows - 1][columns - 1]
```
#### 运行结果
![](image/leetcode_64.png)

## Leetcode 120

#### 题目描述
给定一个三角形 triangle ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。
```
示例 1：

输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```
链接：https://leetcode.cn/problems/triangle

#### 算法思路
###### 动态规划
我们用 f[i][j] 表示从三角形顶部走到位置 (i,j) 的最小路径和。这里的位置 (i,j) 指的是三角形中第 i 行第 j 列（均从 0 开始编号）的位置。

由于每一步只能移动到下一行「相邻的节点」上，因此要想走到位置(i,j)，上一步就只能在位置 (i−1,j−1) 或者位置 (i - 1, j)(i−1,j)。我们在这两个位置中选择一个路径和较小的来进行转移，状态转移方程为：

```math
f[i][j]=min(f[i−1][j−1],f[i−1][j])+c[i][j]
```

其中 c[i][j] 表示位置 (i,j) 对应的元素值。

注意第 i 行有 i+1 个元素，它们对应的 j 的范围为 [0,i]。当 j=0 或 j=i 时，上述状态转移方程中有一些项是没有意义的。例如当 j=0 时，f[i−1][j−1] 没有意义，因此状态转移方程为：

```math
f[i][0]=f[i−1][0]+c[i][0]
```

即当我们在第 i 行的最左侧时，我们只能从第 i−1 行的最左侧移动过来。当 j=i 时，f[i−1][j] 没有意义，因此状态转移方程为：

```math
f[i][i]=f[i−1][i−1]+c[i][i]
```

即当我们在第 ii 行的最右侧时，我们只能从第 i-1i−1 行的最右侧移动过来。

最终的答案即为 $f[n−1][0]$ 到 $f[n−1][n−1]$中的最小值，其中 nn 是三角形的行数。

#### 代码实现

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        f = [[0] * n for _ in range(n)]
        f[0][0] = triangle[0][0]

        for i in range(1, n):
            f[i][0] = f[i - 1][0] + triangle[i][0]
            for j in range(1, i):
                f[i][j] = min(f[i - 1][j - 1], f[i - 1][j]) + triangle[i][j]
            f[i][i] = f[i - 1][i - 1] + triangle[i][i]
        
        return min(f[n - 1])
```

#### 运行结果
![](image/leetcode_120.png)


