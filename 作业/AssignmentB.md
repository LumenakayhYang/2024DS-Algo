# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

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

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：遍历一遍棋盘，如果遇到"."就用dfs把整个连通块内的"."全部标记成"-"防止重复计数。值得注意的是如果只是计算形如下图的图形数量

 . 

...

 .

在样例点里面也可以通过，本人就因为读错题目，理解成计算形如上图的图形数量WA很久……

代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat May  4 11:17:29 2024

@author: liumi
"""

def main():
    
    def dfs(x,y):
        nonlocal D
        nonlocal graph
        graph[x][y]="-"
        for d in D:
            x_=x+d[0]
            y_=y+d[1]
            if 0<=x_<10 and 0<=y_<10 and graph[x_][y_]==".":
                dfs(x_, y_)
            else:
                pass

    graph=[]
    D=[(1,0),(-1,0),(0,1),(0,-1)]
    for _ in range(10):
        l=list(input())
        graph.append(l)
    cnt=0
    for x in range(10):
        for y in range(10):
            if graph[x][y]==".":
                cnt+=1
                dfs(x,y)
    print(cnt)

main()

```

读懂题目后耗时8min





### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：按行依次放入皇后，每次放入一个就更新之后的行可以放入的位置。记录所有解然后按要求输出



代码

```python
# 
# -*- coding: utf-8 -*-
"""
Created on Sun May  5 17:43:00 2024

@author: liumi
"""

import copy

def dfs(n,str_,graph):
    
    if n==9:
        global lst
        lst.append(str_)
        return None
    elif graph[n]==[0,0,0,0,0,0,0,0]:
        return None
    else:
        for i in graph[n]:
            if i!=0:
                temp=copy.deepcopy(graph)
                s=str_*10+i
                for row in range(n+1,9):
                    temp[row][i-1]=0
                    if i+row-n-1<8:
                        temp[row][i+row-n-1]=0
                    if i-row+n-1>=0:
                        temp[row][i-row+n-1]=0
                dfs(n+1, s, temp)
                    
graph={i:[1,2,3,4,5,6,7,8] for i in range(1,9)}
lst=[]
dfs(1, 0, graph)
n=int(input())
for i in range(n):
    t=int(input())
    print(lst[t-1])

```

耗时约30min，因为OJ网站奇怪的CE报错耽误了时间，似乎是global声明不当导致的。



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：一共支持6种操作，在每一次遍历当前节点之后依次加入这六种操作。如果操作和操作状态已经被访问过，那么就不再入队。奇怪的是visited似乎不能用list而必须用set，否则会超时。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun May  5 22:53:11 2024

@author: liumi
"""
from collections import deque


class node:
    
    def __init__(self,cmd,av,bv,node):
        self.cmd=cmd
        self.av=av
        self.bv=bv
        self.parent=node
    
    def get_value(self):
        return (self.cmd,self.av,self.bv)
    
    def get_ab(self):
        return self.av,self.bv
    
def bfs():
    a,b,c=map(int,input().split())
    cmd_list=["FILL(1)","FILL(2)","DROP(1)","DROP(2)","POUR(1,2)","POUR(2,1)"]
    queue=deque()
    queue.append(node("FILL(1)", 0, 0, None))
    queue.append(node("FILL(2)", 0, 0, None))
    visited=set()
    while queue:
        crt=queue.popleft()
        a_,b_=crt.get_ab()
        visited.add(crt.get_value())
        to_do=1
        if crt.cmd=="FILL(1)":
            a_=a
        elif crt.cmd=="FILL(2)":
            b_=b
        elif crt.cmd=="DROP(1)":
            a_=0
        elif crt.cmd=="DROP(2)":
            b_=0
        elif crt.cmd=="POUR(1,2)":
            if b_>=b:
                to_do=0
            else:
                rest=b-b_
                if rest>a_:
                    b_+=a_
                    a_=0
                else:
                    a_-=rest
                    b_=b
        elif crt.cmd=="POUR(2,1)":
            if a_>=a:
                to_do=0
            else:
                rest=a-a_
                if rest>b_:
                    a_+=b_
                    b_=0
                else:
                    b_-=rest
                    a_=a
        if a_==c or b_==c:
            stack=[crt.cmd]
            while crt.parent:
                crt=crt.parent
                stack.append(crt.cmd)
            stack.reverse()
            print(len(stack))
            for i in stack:
                print(i)
            return
        if to_do:
            for _ in cmd_list:
                if (_,a_,b_) not in visited:
                    queue.append(node(_, a_, b_, crt))
    print("impossible")
    return

bfs()

```

耗时约40min



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：似乎就是一道比较普通的有关二叉树的题目，与一般二叉树不同的是，这里需要记录父节点



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon May  6 15:01:37 2024

@author: liumi
"""

class node:
    
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None
        self.parent=None
    
    def get_child(self,child):
        if child==self.left:
            return "left"
        else:
            return "right"
    def get_parent(self):
        return self.parent.name

def build_a_tree(n):
    node_list=[node(i) for i in range(n)]
    for _ in range(n):
        index,left,right=map(int,input().split())
        if left!=-1:
            node_list[index].left=node_list[left]
            node_list[left].parent=node_list[index]
        if right!=-1:
            node_list[index].right=node_list[right]
            node_list[right].parent=node_list[index]
    return node_list

def cmd(node_list,m):
    for _ in range(m):
        _cmd=list(map(int,input().split()))
        if _cmd[0]==1:
            p1,p2=node_list[_cmd[1]].get_parent(),node_list[_cmd[2]].get_parent()
            p1_lr,p2_lr=node_list[p1].get_child(node_list[_cmd[1]]),node_list[p2].get_child(node_list[_cmd[2]])
            if p1_lr=="left":
                node_list[p1].left=node_list[_cmd[2]]
                node_list[_cmd[2]].parent=node_list[p1]
            else:
                node_list[p1].right=node_list[_cmd[2]]
                node_list[_cmd[2]].parent=node_list[p1]
            if p2_lr=="left":
                node_list[p2].left=node_list[_cmd[1]]
                node_list[_cmd[1]].parent=node_list[p2]
            else:
                node_list[p2].right=node_list[_cmd[1]]
                node_list[_cmd[1]].parent=node_list[p2]
        else:
            crt=node_list[_cmd[1]]
            while crt.left:
                crt=crt.left
            print(crt.name)
def main():
    t=int(input())
    for _ in range(t):
        n,m=map(int,input().split())
        node_list=build_a_tree(n)
        cmd(node_list, m)

main()

```

耗时约35min



### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：并查集，但是自己写了一遍，没有看模板。运行用时大大增加了。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon May  6 15:42:50 2024

@author: liumi
"""

class cup:
    
    def __init__(self,i):
        self.id=i
        self.parent=i
    
class Disjoin_set:
    
    def __init__(self,n):
        self.node_list=[cup(i) for i in range(n)]
    
    def join(self,x,y):
        x_=self.node_list[x-1].parent
        while x_!=self.node_list[x_].parent:
            x_=self.node_list[x_].parent
        y_=self.node_list[y-1].parent
        while y_!=self.node_list[y_].parent:
            y_=self.node_list[y_].parent
        if x_==y_:
            print("Yes")
        else:
            print("No")
            self.node_list[y_].parent=x_
    
    def get_cups(self):
        l=[]
        for i in self.node_list:
            if i.id==i.parent:
                l.append(i.id)
        l.sort()
        print(len(l))
        print(" ".join(str(x+1) for x in l))

def main():
    while True:
        try:
            n,m=map(int,input().split())
            Cokes=Disjoin_set(n)
            for i in range(m):
                x,y=map(int,input().split())
                Cokes.join(x,y)
            Cokes.get_cups()
        except EOFError:
            break

main()

```

耗时约40min





### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：感觉和上周走山路一样，都是Dijkstra算法的运用，不一样的地方在于我使用了类来记录路径，这样需要在类里面定义一个判断字典序的函数来防止等长环路的出现。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon May  6 17:51:10 2024

@author: liumi
"""

from heapq import heappop,heappush

class node:
    
    def __init__(self,name,dist,parent):
        self.name=name
        self.dist=dist
        self.parent=parent
    
    def get_way(self):
        if self.dist!=0:
            return f'->({self.dist})->'+self.name
        else:
            return ""
    def __lt__(self,other):
        return self.name<other.name
    
def bfs(start,goal):
    
    global points
    queue=[(0,start,node(start, 0, None))]
    visited=set()
    while queue:
        distance,point,parent_=heappop(queue)
        visited.add(point)
        if point==goal:
            str_=""
            while parent_:
                str_=parent_.get_way()+str_
                parent_=parent_.parent
            str_=start+str_
            print(str_)
            return
        for p_ in points[point]:
            if p_[0] not in visited:
                crt_node=node(p_[0], p_[1], parent_)
                new_distance=distance+p_[1]
                heappush(queue, (new_distance,p_[0],crt_node))
                
            
p=int(input())
points={}
for i in range(p):
    name=input()
    points[name]=[]
q=int(input())
for i in range(q):
    l=list(input().split())
    p1,p2,dist=l[0],l[1],l[2]
    points[p1].append((p2,int(dist)))
    points[p2].append((p1,int(dist)))
r=int(input())
for i in range(r):
    start,goal=input().split()
    bfs(start,goal)

```

耗时约2h



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	感觉复习之前学过的东西占主要，似乎没有上一周那么难了。但是没有合理安排好五一假期学习和放松的关系，这是很大的失误。



