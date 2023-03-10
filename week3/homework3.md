# Homework3
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 16

#### 题目描述
#### 算法思路
* 将给定的数字用栈的方式，实现反转，并将反转的结果存到一个字符串num_str中。
* 去掉字符串中开头多余的‘0’。(如：数字12300翻转以后，字符串会存储为00321)
* 判断反转后的字符串所代表的数字是否是32位的，有三种情况：
    * 如果字符串中数字的长度大于10位，则该数字肯定超过了32位的限制，直接返回0即可。
    * 如果字符串中数字的长度小于10位，则改数字肯定没有超过32位的限制，直接用 int(num_str)返回该字符串代表的数字即可。(注意正负值)
    * 如果字符串中数字的长度刚好等于10位，这时候就判断该字符串代表的数字，是否位于-2^31和2^31之间(逐位比较)，如果在它们两个中间，则代表该值在32位以内，直接返回该值，否则返回。
#### 代码实现

```python
class Solution(object):

    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        l = len(nums)
        res = 100000

        def updt_res(num):
            return num if abs(num-target) < abs(res-target) else res
    
        for i in range(l):
            
            left,right= i+1,l-1
            #Sum = nums[i]+nums[left]+nums[right]
            #updt_res(Sum)
            while left >= 0 and right <= l-1 and left < right:
                Sum = nums[i]+nums[left]+nums[right]
                res = updt_res(Sum)
                if Sum < target:
                    left += 1
                elif Sum > target:
                    right -= 1
                else :
                    return target
        return res                
```
#### 运行结果
![](image/leetcode_16.png)

## Leetcode 17
#### 题目描述
#### 算法思路
新建一个哈希表存储每个罗马字母对应的数值，key为罗马字母，value为单个字母代表的数字。初始一个num变量，初始值为0.然后遍历给出的包含罗马字符的字符串，共有以下三种情况:
1. 当前字母代表的数值大于右侧，则将其代表的值加到num变量上；
2. 当前字母代表的数值大于右侧，则用num变量减去其代表的数值；
3. 当前字母为最后一位数字，则不用比较直接将其代表的值加到num变量上。
#### 代码实现
```python
class Solution(object):
    res = [] ##类内变量，用于存储最终返回的结果

    #回溯函数，用于回溯更新 变量res的值
    def BackTrack(self,digits,single_choice,choice_map):
        if len(single_choice) == len(digits) :
            Solution.res.append(single_choice)
            return

        choice_str = choice_map[digits[len(single_choice)]]
        for i in range(len(choice_str)):
            single_choice = single_choice + choice_str[i]

            self.BackTrack(digits,single_choice,choice_map)
                
            single_choice = single_choice[:-1]
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if digits == "":
            return []
        
        letter_map = {'':'',
                      '2':'abc',
                      '3':'def',
                      '4':'ghi',
                      '5':'jkl',
                      '6':'mno',
                      '7':'pqrs',
                      '8':'tuv',
                      '9':'wxyz'}
        single_choice = ''
        Solution.res = list()
        Solution.BackTrack(self,digits,single_choice,letter_map)
        return Solution.res
```
#### 运行结果
![](image/leetcode_17.png)

## Leetcode 21

#### 题目描述

#### 算法思路

#### 代码实现

```python
class ListNode(object):
     def __init__(self, val=0, next=None):
         self.val = val
         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        dummy = ListNode(0,head)
        fast_node = ListNode(0,dummy)
        slow_node = ListNode(0,dummy)
        for i in range(n):
            fast_node = fast_node.next
        while fast_node.next != None :
            slow_node, fast_node = slow_node.next,fast_node.next
        slow_node.next =  slow_node.next.next
        
        return dummy.next
```

#### 运行结果
![](image/leetcode_19.png)
