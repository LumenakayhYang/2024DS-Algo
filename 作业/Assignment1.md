# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

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

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：使用递推表达式f(n)=f(n-1)+f(n-2)+f(n-3)来求出泰波纳契数列指定项的值



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 18:53:13 2024

@author: liumi
"""

def f(x):
    if x==0:
        return 0
    elif x==1 or x==2:
        return 0
    else:
        a,b,c,i=0,1,1,2
        while i!=x:
            t=a+b+c
            a=b
            b=c
            c=t
            i+=1
        return t

n=int(input())
print(f(n))

```



代码运行截图 ==（至少包含有"Accepted"）==





### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：用一个i作为循环变量去遍历输入的字符串，用一个列表a来保存已读取到的“hello”字符串的片段，在每一次对读入字符的时候把“该字符是否属于’hello‘“与”已读取到的’hello‘片段”同时进行检查，来保证每个字母的出现顺序正确。



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 19:11:09 2024

@author: liumi
"""

def check(s):
    c,a,i,n=0,[],0,len(s)
    while i<n:
        if s[i]=="h" and not s[i] in a:
            a.append("h")
            i+=1
        elif s[i]=="h" and s[i] in a:
            i+=1
        elif s[i]=="e" and a==["h"]:
            a.append("e")
            i+=1
        elif s[i]=="e" and s[i] in a:
            i+=1
        elif s[i]=="l":
            if a==["h","e"]:
                a.append("l")
                i+=1
            elif a==["h","e","l"]:
                a.append("l")
                i+=1
            elif a==["h","e","l","l"]:
                i+=1
            else:
                i+=1
        elif s[i]=="o" and a==["h","e","l","l"]:
            c=1
            return "YES"
        else:
            i+=1
    if c==0:
        return "NO"
    
n=input()
print(check(n))

```



代码运行截图 ==（至少包含有"Accepted"）==





### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：先将字符串全部转换为小写，依次判断字符串中的字符是否是元音字母，若是，跳过这个字符；若不是，则该字母为辅音字母，在其前加上点号，并顺序储存在字符串s0中。

##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 19:42:32 2024

@author: liumi
"""

def change(s):
    s=s.lower()
    s0=""
    l=["a","e","i","o","u","y"]
    for i in range(len(s)):
        if s[i] in l:
            pass
        else:
            s0+="."+s[i]
    return s0

s=input()
print(change(s))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：先定义判断质数的check函数，接着把从2到n的数遍历并判断是否是质数，若是，判断n与该质数之差是否为质数，若是，则可以输出这两个质数，循环结束



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 20:35:40 2024

@author: liumi
"""

def check(n):
    for i in range(2,int(n**(1/2)+1)):
        if n%i==0:
            return False
    return True

n=int(input())
for i in range(2,n):
    if check(i):
        if check(n-i):
            print(" ".join(str(x) for x in (i,n-i)))
            break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：将输入的表达式字符串按“+”分割得到每一项，定义检查每一项次数的check函数：check函数把每一项按照”^“分为含系数项和指数项，取出系数并检查是否为零，不为零的返回该项的指数，



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 20:57:58 2024

@author: liumi
"""

s=input().split("+")
def check(x):
    x=x.split("^")
    a=x[0]
    if a=="n":
        return int(x[1])
    else:
        na=len(a)
        a=a[ : :-1]
        a=a[1:len(a):1]
        a=a[ : :-1]
        if int(a)!=0:
            return int(x[1])
        else:
            return False
l=[]
for x in s:
    if check(x):
        l.append(check(x))
    else:
        pass
print("n^"+str(max(l)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：使用列表l来记录序号，使用另一个列表来记录对应的票数，然后获取最高票数，从原来的l中检索出最高票数的对应序号，存入列表r中，最后将r从小到大排序并输出。

##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Tue Feb 20 21:14:21 2024

@author: liumi
"""

n=input().split()
l,l_=[],[]
for i in n:
    if not i in l:
        l.append(i)
        l_.append(1)
    else:
        t=l.index(i)
        l_[t]+=1
m=max(l_)
r=[]
for j in range(len(l_)):
    if l_[j]==m:
        r.append(int(l[j]))
r.sort()
print(" ".join(str(x) for x in r))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“数算pre每日选做”、CF、LeetCode、洛谷等网站题目。==

​	第一周总的来说还算适应，又逐渐找到了之前计概写程序的感觉，有练习一些OJ上的题目，但是花的时间不够多，还远远赶不上提高班同学的水平，之后还要勤加练习。



