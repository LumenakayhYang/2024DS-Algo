# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

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

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：直接用列表的reverse方法可以得到答案



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 13:29:41 2024

@author: liumi
"""

s=input().split()
s.reverse()
print(" ".join(x for x in s))

```



机考用时和再度复现用时都只用了两分钟（可能通过一些语法糖可以更快？）

### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：这个“内存”其实就是一个队列，满了就把前面的弹出。



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 13:35:20 2024

@author: liumi
"""

m,n=map(int,input().split())
dic=[]
lst=list(map(int,input().split()))
cnt=0
for i in lst:
    if not i in dic:
        if len(dic)<m:
            dic.append(i)
        else:
            dic.pop(0)
            dic.append(i)
        cnt+=1
    else:
        pass
print(cnt)

```



机考用了可能10分钟左右，复现用了6分钟。

### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：直接使用列表的sort方法，但是边界情况值得注意：当k==1时，应该视情况输出1或者-1。机考的时候就是因为这个导致消耗太多时间，后面两题没来得及做。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 13:44:27 2024

@author: liumi
"""

n,k=map(int,input().split())
l=list(map(int,input().split()))
l.sort()
if k==n:
    print(l[-1])
elif k==0:
    if l[0]>1:
        print(1)
    else:
        print(-1)
else:
    if l[k-1]<l[k]:
        print(l[k-1])
    else:
        print(-1)

```



机考的时候直接思考的时间有可能差不多20min（为了debug），清楚边界情况之后复现只用了3分钟。

### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：比较常规的建树题目，通过不断二分字符串来建树。在机考的时候还算顺利，但是因为习惯性地把**打成^而debug消耗了一些时间。



代码

```python
# 
# -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 13:49:09 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None

def Build_A_Tree(s,n):
    if n==0:
        if s=="0":
            return Node("B")
        else:
            return Node("I")
    
    if "1" in s and "0" in s:
        s_=Node("F")
    elif "1" in s and not "0" in s:
        s_=Node("I")
    elif "0" in s and not "1" in s:
        s_=Node("B")

    l=s[:2**(n-1)]
    r=s[2**(n-1):]
    s_.left=Build_A_Tree(l, n-1)
    s_.right=Build_A_Tree(r, n-1)
    return s_

def printable(node):
    if not node:
        return ""
    return printable(node.left)+printable(node.right)+node.name

n=int(input())
s=input()
tree=Build_A_Tree(s, n)
print(printable(tree))
```





### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：这题的数据量一度让我担心直接模拟会不会爆内存或者爆时间，但是好在没有。用表套表的方式来模拟整个队列，每个小组用一个内层的表来表示，各组之间再形成一个大的队列。另外有一个储存小组在大的队列当中的位置的表，在插入新成员时可以直接索引到小组的位置进行插入。出队的话主要考虑当排在第一的小组无人可出的情况进行处理



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 14:00:28 2024

@author: liumi
"""

t=int(input())
l,p=[],[-1 for i in range(t)]
for i in range(t):
    g=tuple(input().split())
    l.append(g)
queue=[]
while True:
    cmd=input().split()
    if cmd[0]=="STOP":
        break
    elif cmd[0]=="ENQUEUE":
        for j in range(t):
            if cmd[1] in l[j]:
                if p[j]==-1:
                    queue.append([cmd[1]])
                    p[j]=len(queue)-1
                else:
                    queue[p[j]].append(cmd[1])
                break
    elif cmd[0]=="DEQUEUE":
        try:
            print(queue[0].pop(0))
        except IndexError:
            queue.pop(0)
            for k in range(t):
                if p[k]!=-1:
                    p[k]-=1
            print(queue[0].pop(0))

```





花了15分钟来实现，可惜机考的时候时间不够了

### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：用邻接表来表示树，通过对所有节点集合和子节点集合的比较来确定根节点。确定根节点以后再通过一个转换函数来进行遍历。这个函数可以将当前节点及其子节点的大小进行排序，然后再按大小顺序依次遍历每个节点。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr  5 15:17:00 2024

@author: liumi
"""

n=int(input())
nodes,children,dic=[],[],{}
for i in range(n):
    s=list(map(int,input().split()))
    nodes.append(s[0])
    children+=s[1:]
    dic[s[0]]=s[1:]
nodes=set(nodes)
children=set(children)
r=nodes-children
it=iter(r)
root=next(it)

def printable(tree,root):
    if tree[root]==[]:
        return [root]
    
    lst,to_go=[],[root]
    to_go+=tree[root]
    to_go.sort()
    for i in to_go:
        if i!=root:
            lst+=printable(tree, i)
        else:
            lst+=[root]
    return lst

lst=printable(dic, root)
for i in range(n):
    print(lst[i])

```



实现花了23分钟，但是经历了一个小时的反复思考才得到邻接表这个做法。

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	本次的月考，在该时间限制下的题目难度个人觉得适中（但是比近两周作业简单了好多），是我再熟练一点就有可能AC6的题目。但是这其中还有很长的路要走。在什么时候用什么方式来表达数据结构，这是我要不断从练习中才能得到的。



