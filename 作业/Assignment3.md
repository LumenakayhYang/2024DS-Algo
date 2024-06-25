# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：利用一个dp数组来不断更新在发射高度降至某高度时可以击落导弹的最大枚数，然后输出第一个值得到发射高度降到0时可以击落的最大枚数



##### 代码

```python
# 
# -*- coding: utf-8 -*-
"""
Created on Fri Mar  8 13:00:39 2024

@author: liumi
"""

k=int(input())
h_list=list(map(int,input().split()))
m=max(h_list)
dp1=[0 for i in range(m)]
dp2=[0 for i in range(m)]
for i in range(k):
    for j in range(m):
        if j<=h_list[i]:
            try:
                dp1[j]=max(dp2[j],dp2[h_list[i]]+1)
            except IndexError:
                dp1[j]=max(dp2[j],dp2[h_list[i]-1]+1)
        else:
            pass
    dp2=dp1.copy()
print(dp2[0])
            
```



代码编写耗时约30min，思路耗时2d

**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：本题的核心在于，要把n个盘子从A移动到B，也就是要把第n个盘子从A移动到B，必须先把上面n-1个盘子移动到C，完成第n个盘子的移动之后，再把C上的n-1个盘子移回B



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Mar  7 18:24:19 2024

@author: liumi
"""

def move(from_,to_,by_,p):
    if p==1:
        print(f"{p}:{from_}->{to_}")
    else:
        move(from_, by_, to_, p-1)
        print(f"{p}:{from_}->{to_}")
        move(by_,to_,from_,p-1)

num,from_,by_,to_=input().split()
num=int(num)
move(from_, to_, by_, num)

```



耗时约15min

**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：本题可以用模拟的方法来做，报数环节可以用模简化，需要注意的是循环变量要控制得当。



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Mar  7 18:50:43 2024

@author: liumi
"""

def queue(n,m,p):
    l=[i for i in range(1,n+1)]
    j=p-1+m-1
    if j>=n:
        j=j%n
    r=[]
    while l!=[]:
        r.append(l.pop(j))
        if l==[]:
            break
        else:
            j+=m-1
            if j>=len(l):
                j%=len(l)
    return r

n,p,m=map(int,input().split())
while (n,p,m)!=(0,0,0):
    print(",".join(str(x) for x in queue(n, m, p)))
    n,p,m=map(int,input().split())

```



耗时约15min

**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：先把序号和用时关联起来，存在一个列表中，把这个列表按照用时来排序，按用时从小到大的顺序来排队，可以使得平均等待时间最短。



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Mar  7 20:08:20 2024

@author: liumi
"""

def get_second(x):
    return x[1]

n=int(input())
l=list(map(int,input().split()))
l_=[]
for i in range(n):
    l_.append([i+1,l[i]])
l_.sort(key=get_second)
r=[i[0] for i in l_]
s=0
for j in range(0,n-1):
    s+=l_[j][1]*(n-(j+1))
print(" ".join(str(x) for x in r))
print(f"{s/n:.2f}")

```



耗时约20min

**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：比较简单，计算中位数之后逐个比较大小即可。



##### 代码

```python
 # -*- coding: utf-8 -*-
"""
Created on Thu Mar  7 21:05:35 2024

@author: liumi
"""

n=int(input())
place=input().split()
price=list(map(int,input().split()))
l=[]
for i in range(n):  
    s=place[i][1:len(place[i])-1]
    s=list(map(int,s.split(",")))
    h=s[0]+s[1]
    l.append((h/price[i],price[i]))
l.sort(key=lambda x:x[0])
lb=[]
if n%2==0:
    mid=(l[int(n/2-1)][0]+l[int(n/2)][0])/2
    for j in l:
        if j[0]>mid:
            lb.append(j)
else:
    mid=l[int(n/2)][0]
    for j in l:
        if j[0]>mid:
            lb.append(j)
l.sort(key=lambda x:x[1])
lp=[]
if n%2==0:
    mid=(l[int(n/2-1)][1]+l[int(n/2)][1])/2
    for j in l:
        if j[1]<mid:
            lp.append(j)
else:
    mid=l[int(n/2)][1]
    for j in l:
        if j[1]<mid:
            lp.append(j)
if lb==[] or lp==[]:
    print(0)
else:
    sum_=0
    for m in lb:
        if m in lp:
            sum_+=1
    print(sum_)
```



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：基本上就是按照模型名称为键，参数大小的列表为值来创建字典并输出，但是这里将“M”和“B”分开比较再合并。



##### 代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Mar  7 20:40:42 2024

@author: liumi
"""

n=int(input())
dic={}
for i in range(n):
    s=input().split("-")
    if not s[0] in dic:
        dic[s[0]]=[s[1]]
    else:
        dic[s[0]].append(s[1])

def transfer(n):
    try:
        n=int(n)
    except ValueError:
        n=float(n)
    return n

def process(x):
    lm,lb=[],[]
    for i in x:
        if i[-1]=="M":
            lm.append(transfer(i[0:len(i)-1:1]))
        else:
            lb.append(transfer(i[0:len(i)-1:1]))
    lm.sort()
    lb.sort()
    l=[str(j)+"M" for j in lm]+[str(k)+"B" for k in lb]
    return l

dic={m[0]:m[1] for m in sorted(dic.items(),key=lambda x:x[0])}

for j in dic:
    dic[j]=process(dic[j])

for k in dic:
    print(f"{k}: {', '.join(x for x in dic[k])}")

```



耗时40min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	本周的作业（月考）感觉上强度了，在规定时间内只能AC3题，我的编程习惯时常导致我需要debug很长时间。以及出现了完全没有思路的“拦截导弹”，这题是看到群里大佬在谈论dp数组之后才想起来可以用这样的办法。每日选做难度参差不齐，有的时候随便挑的题目用到计概时期完全没有学过的知识。这说明我的水平有待提高。革命尚未成功，同志仍需努力。



