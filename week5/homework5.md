# Homework5
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 5

#### 题目描述
给你一个字符串 s，找到 s 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

示例 1：

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

链接：https://leetcode.cn/problems/longest-palindromic-substring

#### 算法思路
###### 动态规划
对于一个子串而言，如果它是回文串，并且长度大于 22，那么将它首尾的两个字母去除之后，它仍然是个回文串。例如对于字符串 “ababa”，如果我们已经知道 “bab” 是回文串，那么“ababa” 一定是回文串，这是因为它的首尾两个字母都是“a”。

根据这样的思路，我们就可以用动态规划的方法解决本题。我们用 P(i,j)表示字符串s的第i到j个字母组成的串（下文表示成s[i:j]）是否为回文串：

$$P(i, j)=\left\{\begin{array}{ll}
\text { true, } & \text { 如果子串 } S_{i} \ldots S_{j} \text { 是回文串 } \\
\text { false, } & \text { 其它情况 }
\end{array}\right.$$
 
这里的「其它情况」包含两种可能性：

* s[i,j] 本身不是一个回文串；
* i>j，此时s[i,j] 本身不合法。

那么我们就可以写出动态规划的状态转移方程：

$$P(i, j)=P(i+1, j-1) \wedge\left(S_{i}==S_{j}\right)$$

也就是说，只有s[i+1:j−1] 是回文串，并且 ss 的第 ii 和 jj 个字母相同时，s[i:j] 才会是回文串。

上文的所有讨论是建立在子串长度大于 22 的前提之上的，我们还需要考虑动态规划中的边界条件，即子串的长度为 11 或 22。对于长度为 11 的子串，它显然是个回文串；对于长度为 22 的子串，只要它的两个字母相同，它就是一个回文串。因此我们就可以写出动态规划的边界条件：

$$
\left\{\begin{array}{l}
P(i, i)=\text { true } \\
P(i, i+1)=\left(S_i==S_{i+1}\right)
\end{array}\right.
$$

根据这个思路，我们就可以完成动态规划了，最终的答案即为所有 P(i,j)=true中j−i+1（即子串长度）的最大值。注意：在状态转移方程中，我们是从长度较短的字符串向长度较长的字符串进行转移的，因此一定要注意动态规划的循环顺序。

#### 代码实现

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        if n < 2:
            return s
        
        max_len = 1
        begin = 0
        # dp[i][j] 表示 s[i..j] 是否是回文串
        dp = [[False] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = True
        
        # 递推开始
        # 先枚举子串长度
        for L in range(2, n + 1):
            # 枚举左边界，左边界的上限设置可以宽松一些
            for i in range(n):
                # 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                j = L + i - 1
                # 如果右边界越界，就可以退出当前循环
                if j >= n:
                    break
                    
                if s[i] != s[j]:
                    dp[i][j] = False 
                else:
                    if j - i < 3:
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i + 1][j - 1]
                
                # 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if dp[i][j] and j - i + 1 > max_len:
                    max_len = j - i + 1
                    begin = i
        return s[begin:begin + max_len]
```
#### 运行结果
![](image/leetcode_5.png)

## Leetcode 64
#### 题目描述
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

链接：https://leetcode.cn/problems/sort-list

#### 算法思路
「147. 对链表进行插入排序」要求使用插入排序的方法对链表进行排序，插入排序的时间复杂度是 O(n2)，其中 n 是链表的长度。这道题考虑时间复杂度更低的排序算法。题目的进阶问题要求达到 O(nlogn) 的时间复杂度和 O(1) 的空间复杂度，时间复杂度是 O(nlogn) 的排序算法包括归并排序、堆排序和快速排序（快速排序的最差时间复杂度是 O(n2)），其中最适合链表的排序算法是归并排序。
归并排序基于分治算法。最容易想到的实现方式是自顶向下的递归实现，考虑到递归调用的栈空间，自顶向下归并排序的空间复杂度是 O(logn)。如果要达到 O(1) 的空间复杂度，则需要使用自底向上的实现方式。
###### 方法一：自顶向下归并排序

对链表自顶向下归并排序的过程如下。
1. 找到链表的中点，以中点为分界，将链表拆分成两个子链表。寻找链表的中点可以使用快慢指针的做法，快指针每次移动 2 步，慢指针每次移动 1 步，当快指针到达链表末尾时，慢指针指向的链表节点即为链表的中点。
2. 对两个子链表分别排序。
3. 将两个排序后的子链表合并，得到完整的排序后的链表。可以使用「21. 合并两个有序链表」的做法，将两个有序的子链表进行合并。

上述过程可以通过递归实现。递归的终止条件是链表的节点个数小于或等于 1，即当链表为空或者链表只包含 1 个节点时，不需要对链表进行拆分和排序。

###### 方法二：自底向上归并排序

首先求得链表的长度length，然后将链表拆分成子链表进行合并。具体做法如下：
1. 用intv表示每次需要排序的子链表的长度，初始时intv=1。

2. 每次将链表拆分成若干个长度为intv的子链表（最后一个子链表的长度可以小于intv），按照每两个子链表一组进行合并，合并后即可得到若干个长度为intv×2 的有序子链表（最后一个子链表的长度可以小于intv×2）。然后合并两个子链表。

3. 将intv的值加倍，重复第 2 步，对更长的有序子链表进行合并操作，直到有序子链表的长度大于或等于length，整个链表排序完毕。

#### 代码实现
```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
class Solution(object):
    def sortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next: return head # termination.
        # cut the LinkedList at the mid index.
        slow, fast = head, head.next
        while fast and fast.next:
            fast, slow = fast.next.next, slow.next
        mid, slow.next = slow.next, None # save and cut.
        # recursive for cutting.
        left, right = self.sortList(head), self.sortList(mid)
        # merge `left` and `right` linked list and return it.
        h = res = ListNode(0)
        while left and right:
            if left.val < right.val: h.next, left = left, left.next
            else: h.next, right = right, right.next
            h = h.next
        h.next = left if left else right
        return res.next

        
```
#### 运行结果
![](image/leetcode_64.png)

## Leetcode 120

#### 题目描述
给你一个整数数组 citations ，其中 citations[i] 表示研究者的第 i 篇论文被引用的次数。计算并返回该研究者的 h 指数。

根据维基百科上 h 指数的定义：h 代表“高引用次数”，一名科研人员的 h指数是指他（她）的 （n 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。且其余的 n - h 篇论文每篇被引用次数 不超过 h 次。
如果 h 有多种可能的值，h 指数 是其中最大的那个

链接：https://leetcode.cn/problems/h-index

#### 算法思路
###### 排序
首先我们可以将初始的 H指数 h设为 0，然后将引用次数排序，并且对排序后的数组从大到小遍历。根据 H 指数的定义，如果当前 H 指数为 h 并且在遍历过程中找到当前值 
citations[i]>h，则说明我们找到了一篇被引用了至少 h+1 次的论文，所以将现有的 h 值加1。继续遍历直到 h 无法继续增大。最后返回 h 作为最终答案。

#### 代码实现

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        sorted_citation = sorted(citations, reverse = True)
        h = 0; i = 0; n = len(citations)
        while i < n and sorted_citation[i] > h:
            h += 1
            i += 1
        return h
```

#### 运行结果
![](image/leetcode_128.png)


