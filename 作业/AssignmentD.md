# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

2024 spring, Complied by ==杨英浩 生命科学学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows11

Python编程环境：Spyder IDE 5.4.3

C/C++编程环境：无

## 1. 题目

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：用一个列表来模拟行道树队列



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Wed May 15 00:31:01 2024

@author: liumi
"""

l,m=map(int,input().split())
tree=[True for _ in range(l+1)]
for i in range(m):
    start,end=map(int,input().split())
    for j in range(start,end+1):
        if tree[j]:
            tree[j]=False
print(tree.count(True))

```



代码运行截图 ==（至少包含有"Accepted"）==



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：使用python内置的转换进制功能



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Wed May 15 10:07:34 2024

@author: liumi
"""

s=input()
out=""
for i in range(len(s)):
    if int(s[:i+1],2)%5==0:
        out+="1"
    else:
        out+="0"
print(out)

```



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：其实就是简单的prim算法，但是没注意“输入数据有多组”被卡了好久，看了群里才发现原来WA是因为这个。强烈建议考试的时候出中文题面TT



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Wed May 15 10:16:19 2024

@author: liumi
"""

def prim():
    n=int(input())
    l=[]
    for _ in range(n):
        l.append(list(map(int,input().split())))
    visited=set()
    dist=[float("inf") for _ in range(n)]
    current,cnt=0,0
    while len(visited)!=n-1:
        visited.add(current)
        dist[current]=float("inf")
        for i in range(n):
            if i and l[current][i] and not i in visited:
                dist[i]=min(dist[i],l[current][i])
        minimum=min(dist)
        cnt+=minimum
        current=dist.index(minimum)
    print(cnt)
while True:
    try:
        prim()
    except EOFError:
        break

```

耗时20min，但是因为没有注意审题花了很多时间





### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：总觉得之前在哪里写过，可能是AssignmentC？但是那时候没有看懂。现在自己搓一遍，感觉还是要背模板，不然花很久的时间。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Wed May 15 10:35:36 2024

@author: liumi
"""
def isconnected(graph):
    visited=set()
    def dfs(n,graph):
        nonlocal visited
        visited.add(n)
        for i in graph[n]:
            if not i in visited:
                dfs(i, graph)
    dfs(0,graph)
    return len(graph)==len(visited)

def isloop(graph,edge):
    visited=set()
    lst=set()
    def dfs(m,n,graph,lst,edge):
        nonlocal visited
        if (m,n) in visited or (n,m) in visited:
            return False
        visited.add((m,n))
        visited.add((n,m))
        lst.add(m)
        for i in graph[n]:
            if i!=m and i in lst:
                return True
            else:
                if not (n,i) in visited:
                    if dfs(n, i, graph, lst, edge):
                        return True
                    else:
                        return False
        if len(visited)==edge*2:
            return False
        lst.remove(m)
    for i in range(len(graph)):
        if graph[i]!=[]:
            for j in graph[i]:
                if dfs(i, j, graph, lst, edge):
                    return True
    return False

v,e=map(int,input().split())
graph=[[] for i in range(v)]
for _ in range(e):
    n,m=map(int,input().split())
    graph[n].append(m)
    graph[m].append(n)
if isconnected(graph):
    print("connected:yes")
else:
    print("connected:no")
if isloop(graph, e):
    print("loop:yes")
else:
    print("loop:no")

```

耗时约1h





### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：本来想写一个自调整的二叉搜索树，但是奈何WA，而且就算真的改对了也会超时，于是改用两个堆，比中位数小的用一个最大堆，比中位数大的用一个最小堆。



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Thu May 16 15:52:49 2024

@author: liumi
"""
from heapq import heappop,heappush

class Line:
    
    def __init__(self,origin):
        self.value=origin
        self.left=[]
        self.right=[]
    
    def insert(self,item1,item2):
        move=0
        for i in (item1,item2):
            if i<=self.value:
                heappush(self.left, -i)
                move-=1/2
            else:
                heappush(self.right, i)
                move+=1/2
        if move==-1:
            heappush(self.right, self.value)
            self.value=-heappop(self.left)
        if move==1:
            heappush(self.left,-self.value)
            self.value=heappop(self.right)
        return self.value
            
def main():
    t=int(input())
    for i in range(t):
        lst=list(map(int,input().split()))
        lst.reverse()
        r=[lst.pop()]
        line=Line(r[0])
        while lst:
            i1=lst.pop()
            try:
                i2=lst.pop()
            except IndexError:
                break
            r.append(line.insert(i1, i2))
        print(len(r))
        print(" ".join(str(x) for x in r))

main()

```







### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：感觉题面都有点不太好懂，比如“最左边的奶牛是最矮的”究竟是在挑选出的奶牛中最矮还是在所有奶牛中最矮，所以去看了题解。甚至并没有太理解题解这种做法的机制。但是至少知道了单调栈是怎样一回事



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Sat May 18 15:49:48 2024

@author: liumi
"""

n=int(input())
lst=[]
for i in range(n):
    h=int(input())
    lst.append(h)
stack=[]
l=[-1]*n
r=[n]*n
for i in range(n):
    while stack and lst[stack[-1]]<lst[i]:
        stack.pop()
    if stack:
        l[i]=stack[-1]
    stack.append(i)
stack=[]
for i in range(n-1,-1,-1):
    while stack and lst[stack[-1]]>lst[i]:
        stack.pop()
    if stack:
        r[i]=stack[-1]
    stack.append(i)
result=0
for i in range(n):
    for j in range(l[i]+1,i):
        if r[j]>i:
            result=max(result,i-j+1)
            break
print(result)

```







## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	本次在规定时间内还是只能AC4。后面两题的话T1还好，只要能想到两个堆就很容易，T2感觉难于理解。另外就是发现自己可能需要多记住模板了，总不能因为模板记不住每次都现场搓耗费大量时间。



