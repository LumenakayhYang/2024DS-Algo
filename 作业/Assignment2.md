# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows11

Python编程环境：Spyder IDE 5.4.3

C/C++编程环境：无



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/



思路：定义一个类对象Fraction，在该对象中用函数gcd实现分数约分，用双下划线add来实现加号，用双下划线str使其可以被print函数输出



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon Feb 26 21:10:12 2024

@author: liumi
"""

class Fraction:
    
    def __init__(self,num,den):
        self.num=num
        self.den=den
    
    @staticmethod
    def gcd(m,n):
        if m<n:
            t=m
            m=n
            n=t
        while m%n!=0:
            m,n=n,m%n
        return n
    
    def __add__(self,obj):
        num=self.num*obj.den+self.den*obj.num
        den=self.den*obj.den
        gcd_=Fraction.gcd(num, den)
        num=int(num/gcd_)
        den=int(den/gcd_)
        return Fraction(num, den)
    
    def __str__(self):
        return f'{self.num}/{self.den}'

s=list(map(int,input().split()))
print(Fraction(s[0], s[1])+Fraction(s[2], s[3]))

```



代码运行截图 ==（至少包含有"Accepted"）==

耗时约10min

### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：按照单位质量价值从高到低依次来装糖果，装满后算出总价值



##### 代码

```python
# 
# -*- coding: utf-8 -*-
"""
Created on Mon Feb 26 21:30:27 2024

@author: liumi
"""
def get_last(n):
    return n[-1]

number,max_weight=map(int,input().split())
list_=[]

for i in range(number):
    value,weight=map(int,input().split())
    average_value=value/weight
    list_.append([value,weight,average_value])
list_.sort(key=get_last)

total_value=0

while max_weight>0:
    if list_!=[]:
        gift=list_.pop()
    else:
        break
    if max_weight>=gift[1]:
        total_value+=gift[0]
        max_weight-=gift[1]
    else:
        total_value+=gift[0]*max_weight/gift[1]
        break

print(f'{total_value:.1f}')
```





### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：用字典来储存每个时刻对应能够使用的技能伤害，先将每一个时刻的技能伤害从大到小排序，再将时刻从前到后排序，依次遍历时刻，当怪物血量减小至不大于零时输出当前时刻



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon Feb 26 22:16:10 2024

@author: liumi
"""

def judge(n,m,b):
    attack={}
    for i in range(n):
        t,x=map(int,input().split())
        if not t in attack:
            attack[t]=[x]
        else:
            attack[t].append(x)
    for j in attack:
        attack[j].sort(reverse=True)
    attack={i:attack[i] for i in sorted(attack.keys())}
    for k in attack:
        b-=sum(attack[k][0:min(m,len(attack[k]))])
        if b<=0:
            print(k)
            return None
    print("alive")
    return None

number=int(input())
for i in range(number):
    n,m,b=map(int,input().split())
    judge(n, m, b)

```



耗时约18min

### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：先将0至1000000的质数表用欧拉筛打出，然后用math库中的sqrt函数给被输入的整数开平方，利用索引直接查找其平方根是否是质数



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Tue Feb 27 22:54:01 2024

@author: liumi
"""
from math import sqrt

l=[1 for i in range(1000001)]
l[0],l[1]=0,0
for j in range(1000001):
    if l[j]!=0:
        for k in range(j*j,1000001,j):
            l[k]=0

n=int(input())
lst=list(map(int,input().split()))
for x in lst:
    if sqrt(x)%1==0 and l[int(sqrt(x))]:
        print("YES")
    else:
        print("NO")

```





### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：将从左端开始的子序列的和和右段开始的子序列的和用两个列表储存，依次遍历检查其是否为给定整数的倍数，并且不断更新符合条件的最大长度。



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Feb 29 11:46:45 2024

@author: liumi
"""

N=int(input())
for i in range(N):
    n,x=map(int,input().split())
    a=list(map(int,input().split()))
    l1,l2,s1,s2,s=[],[],0,0,-1
    for k in range(n):
        s1+=a[k]
        l1.append(s1)
        s2+=a[-k-1]
        l2.append(s2)
    for l in range(1,n+1):
        if l1[-l]%x!=0 or l2[-l]%x!=0:
            s=n-l+1
            break
    print(s)

```



耗时约30min

### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：这题同T-prime一样，都是用欧拉筛打质数表+索引的办法来判断一个整数是否满足条件。



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Tue Feb 27 22:00:43 2024

@author: liumi
"""
from math import sqrt

l=[1 for i in range(10001)]
l[0],l[1]=0,0
for i in range(10001):
    if l[i]==0:
        pass
    else:
        for j in range(i*i,10001,i):
            l[j]=0

m,n=map(int,input().split())
for j in range(m):
    lst=list(map(int,input().split()))
    s=0
    for x in lst:
        if sqrt(x)%1==0 and l[int(sqrt(x))]:
            s+=x
    if s==0:
        print(s)
    else:
        print(f'{s/len(lst):.2f}')


```



耗时约13min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	本周的题目变得比较有意思，不过还没有出现难得完全做不出来的情况。XXXXX一开始是审题出了问题，不得已查看了题解才发现自己审题有误，以后面对英文题目还是要多加小心。每日选做因为课程的原因跟不上了，正在想办法弥补。



