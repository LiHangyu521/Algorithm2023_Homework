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
