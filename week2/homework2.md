# Homework2
###### Name: 李杭禹
###### StudentID: 2201212834

## Leetcode 7

#### 算法思路
#### 代码实现

```python
class Stack(object):
    def __init__(self):
        self.items = []
    def isEmpty(self):
        return self.items==[]
    def push(self,item):
        self.items.append(item)
    def pop(self):
        return self.items.pop()
    def peek(self):
        return self.items[len(self.items)-1]
    def size(self):
        return len(self.items)

#去掉字符串开头的所有0
def remv_0(num_str):
    t = 0 ## t表示在字符串的开头0出现的次数，
    for i in range(len(num_str)):
        if num_str[i] == '0':
            t += 1
        else : break
    return num_str[t:]

#判断一个用字符串表示的数，是否是32位的,是的话返回true
def is_int(flag,num_str):
    num_max_list = [2,1,4,7,4,8,3,6,4,7]
    num_min_list = [2,1,4,7,4,8,3,6,4,8]
    num_list = []
    for i in range(len(num_str)):
        num_list.append(int(num_str[i]))
    if flag==1:
        j = 0
        while j < 10:
            if num_list[j] < num_max_list[j]:
                return True
            elif num_list[j] == num_max_list[j]:
                if j == 9 :
                    return True
                j += 1
            else :
                return False          
    if flag==-1:
        j = 0
        while j < 10:
            if num_list[j] < num_min_list[j]:
                return True
            elif num_list[j] == num_min_list[j]:
                if j == 9 :
                    return True
                j += 1
            else :
                return False       
            
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        stack = Stack()
        flag = 1 ##用于标记x是正数还是负数 
        if x == 0 :
            return 0
        
        if x<0 :
            flag = -1
            #stack.push('-')
            x = -x

        while x>0 :
            mod = x % 10
            stack.push(mod)
            x = x//10
        
        num_str = ""
        while not stack.isEmpty() :
            tmp = stack.pop()
            tmp = str(tmp)
            num_str = tmp + num_str

        result = 0
        num_str = remv_0(num_str)
        
        length = len(num_str)
        if length > 10 :
            return 0
        elif length == 10:
            if is_int(flag,num_str) :
                return flag*int(num_str)
            else: return 0             
        else :
            return flag*int(num_str)
```
#### 运行结果

## Leetcode 13
#### 算法思路
#### 代码实现
```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        Roma_dict = {'I':1,
                     'V':5,
                     'X':10,
                     'L':50,
                     'C':100,
                     'D':500,
                     'M':1000}
        num = 0
        for i in range(len(s)):
            if  i == len(s)-1:
                num += Roma_dict[s[i]]
            elif Roma_dict[s[i]] >= Roma_dict[s[i+1]]:
                num += Roma_dict[s[i]]
            else :
                num -= Roma_dict[s[i]]
        return num
```
#### 运行结果

## Leetcode 66

#### 算法思路
#### 代码实现

```python
class Solution(object):
    def plusOne(self, digits):
        """
        :type digits: List[int]
        :rtype: List[int]
        """
        i = len(digits)-1
        digits[i] = digits[i] + 1
        while digits[i]==10 and i >=1 :
            digits[i] = 0
            digits[i-1] = digits[i-1]+1
            i -= 1
        if digits[0]==10:
            digits[0] = 0
            digits.insert(0,1)
        return digits
```

#### 运行结果
