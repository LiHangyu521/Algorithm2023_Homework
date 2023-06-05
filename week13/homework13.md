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
bool isPrime(int n){
    if(n<=3){
        return n>1;
    }
 
    if(n%6!=1 && n%6!=5){
        return false;   
    }
    
    int s = (int)sqrt(n);
    for(int i=5; i<=s; i+=6){
        if(n%i==0 || n%(i+2)==0){
            return false;
        }
    }
 
    return true;
}
```
#### 复杂度分析

1. 从 2 到 $\sqrt{n}$ 的遍历试除，算法的复杂度为$O(\sqrt{n})$
2. 再增加了判断除6的余数之后，算法的最坏时间复杂度仍为$O(\sqrt{n})$,但对于4/6的情况都可以在O(1)的时间复杂度内计算完成。所以算法的平均复杂度小于$O(\sqrt{n})$
