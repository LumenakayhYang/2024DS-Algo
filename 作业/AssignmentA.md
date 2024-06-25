# Assignment #A: 图论：算法，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：直接用栈来实现，遇到“）”就弹出反转。本来还害怕这样做不到100ms内完成，没想到还是放了一马。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 28 17:49:08 2024

@author: liumi
"""

origin_str=list(input())
stack=[]
for letter in origin_str:
    if not letter==")":
        stack.append(letter)
    else:
        temp=[]
        while stack[-1]!="(":
            temp.append(stack.pop())
        stack.pop()
        stack+=temp
print("".join(stack))

```

耗时8min





### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：这应该是以前写过的题目：根据二叉树前中序列建树。思路是前序第一个字母提示树根，中序提示左树和右树，然后递归地建树就可以了。应该是第三次写这样的题目了，写的还是很熟练的，也达到了一些复习的效果。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 28 18:02:42 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None
        
def build_a_tree(first,mid):
    
    if first==mid:
        if len(first)==0:
            return None
        elif len(first)==1:
            return Node(first[0])
    
    root_name=first[0]
    root_index=mid.index(root_name)
    root=Node(root_name)
    left_first=first[1:root_index+1]
    left_mid=mid[:root_index]
    root.left=build_a_tree(left_first,left_mid)
    right_first=first[root_index+1:]
    right_mid=mid[root_index+1:]
    root.right=build_a_tree(right_first,right_mid)
    return root

def print_a_tree(root):
    
    if not root.left and not root.right:
        return root.name
    elif not root.left and root.right:
        return print_a_tree(root.right)+root.name
    elif root.left and not root.right:
        return print_a_tree(root.left)+root.name
    else:
        return print_a_tree(root.left)+print_a_tree(root.right)+root.name

def main():
    
    while True:
        try:
            first,mid=input().split()
            tree=build_a_tree(first, mid)
            print(print_a_tree(tree))
        except EOFError:
            break

main()
    

```

耗时18min





### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：太邪门了，用的是bfs，但是卡在99和198这两个点上，似乎数字位数超过十八位就开始变慢了，不得已把前十八位作为起点再找了一次，然后把这两个单独识别输出，非常取巧，但是我看不明白怎么用模数来优化



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 28 18:33:50 2024

@author: liumi
"""


def bfs(n):
    
    queue=["1"]
    if n==99:
        return 1101001010011010010111111111
    elif n==198:
        return 11010010100110100101111111110

    while queue!=[]:
        current=queue.pop(0)
        if int(current)%n==0:
            return current
        queue.append(current+"0")
        queue.append(current+"1")

while True:
    n=int(input())
    if n==0:
        break
    print(bfs(n))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：自己的代码不是超时就是超空间，想不明白为什么。最后跟了夏佬的写法。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Tue Apr 30 14:37:51 2024

@author: liumi
"""

from collections import deque

def seek_path(start,t,m,n):
    
    global map_
    
    start_point=(start[0], start[1], t, 0)
    queue=deque([start_point])
    visited=visited=[[-1]*n for i in range(m)]
    visited[start[0]][start[1]]=t
    D=[(0,1),(0,-1),(1,0),(-1,0)]
    while queue:
        current=queue.popleft()
        c_position=(current[0],current[1])

        for d in D:
            x_=c_position[0]+d[0]
            y_=c_position[1]+d[1]
            if x_>=0 and x_<m and y_>=0 and y_<n:
                if not (x_,y_) in visited:
                    if map_[x_][y_]=="+":
                        return current[3]+1
                    elif map_[x_][y_]=="#":
                        if current[2]>0:
                            visited[x_][y_]=current[2]-1
                            queue.append((x_, y_, current[2]-1, current[3]+1))
                    elif map_[x_][y_]=="*":
                        visited[x_][y_]=current[2]
                        queue.append((x_, y_, current[2], current[3]+1))
                else:
                    if map_[x_][y_]=="#":
                        if current[2]>0 and visited[x_][y_]<current[2]-1:
                            visited[x_][y_]=current[2]-1
                            queue.append((x_, y_, current[2]-1, current[3]+1))
                    elif map_[x_][y_]=="*" and visited[x_][y_]<current[2]:
                        visited[x_][y_]=current[2]
                        queue.append((x_, y_, current[2], current[3]+1))    
    return -1

m,n,t=map(int,input().split())
map_=[]
for x in range(m):
    l=list(input())
    map_.append(l)
for x in range(m):
    for y in range(n):
        if map_[x][y]=="@":
            start=(x,y)
print(seek_path(start, t,m,n))

#以上是我自己的解法
#以下是参考夏天明同学的解法

from collections import deque

M, N, T = map(int, input().split())
graph = [list(input()) for i in range(M)]
direc = [(0,1), (1,0), (-1,0), (0,-1)]
start, end = None, None
for i in range(M):
    for j in range(N):
        if graph[i][j] == '@':
            start = (i, j)
def bfs():
    q = deque([start + (T, 0)])
    visited = [[-1]*N for i in range(M)]
    visited[start[0]][start[1]] = T
    while q:
        x, y, t, time = q.popleft()
        time += 1
        for dx, dy in direc:
            if 0<=x+dx<M and 0<=y+dy<N:
                if (elem := graph[x+dx][y+dy]) == '*' and t > visited[x+dx][y+dy]:
                    visited[x+dx][y+dy] = t
                    q.append((x+dx, y+dy, t, time))
                elif elem == '#' and t > 0 and t-1 > visited[x+dx][y+dy]:
                    visited[x+dx][y+dy] = t-1
                    q.append((x+dx, y+dy, t-1, time))
                elif elem == '+':
                    return time
    return -1
print(bfs())

#经过老师提示以后进行修改，最后通过
# -*- coding: utf-8 -*-
"""
Created on Tue Apr 30 14:37:51 2024

@author: liumi
"""

from collections import deque

def seek_path(start,t,m,n):
    
    global map_
    
    start_point=(start[0], start[1], t, 0)
    queue=deque([start_point])
    visited=visited=[[-1]*n for i in range(m)]
    visited[start[0]][start[1]]=t
    D=[(0,1),(0,-1),(1,0),(-1,0)]
    while queue:
        current=queue.popleft()
        c_position=(current[0],current[1])
        for d in D:
            x_=c_position[0]+d[0]
            y_=c_position[1]+d[1]
            if x_>=0 and x_<m and y_>=0 and y_<n:
                if map_[x_][y_]=="+":
                    return current[3]+1
                elif map_[x_][y_]=="#":
                    if current[2]>0 and current[2]-1>visited[x_][y_]:
                        visited[x_][y_]=current[2]-1
                        queue.append((x_,y_,current[2]-1,current[3]+1))
                elif map_[x_][y_]=="*":
                    if current[2]>visited[x_][y_]:
                        visited[x_][y_]=current[2]
                        queue.append((x_,y_,current[2],current[3]+1))
    return -1

m,n,t=map(int,input().split())
map_=[]
for x in range(m):
    l=list(input())
    map_.append(l)
for x in range(m):
    for y in range(n):
        if map_[x][y]=="@":
            start=(x,y)
print(seek_path(start, t,m,n))
    
```







### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：刚开始上网查了Dijkstra算法的一般形式，尽管理解了但是还是不能很好地运用。最后参考了同学的做法，发现dijkstra的本质在于每一次都把“队列”里路径最短的那个点进行访问。在走山路这个题目里，相当于从疲劳度低到高进行bfs。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Wed May  1 19:20:52 2024

@author: liumi
"""

from heapq import heappop,heappush

def bfs(start,goal,map_,m,n):
    q=[(0,start[0],start[1])]
    visited=set()
    D=[(0,1),(0,-1),(1,0),(-1,0)]
    while q:
        t,x,y=heappop(q)
        if not (x,y) in visited:
            visited.add((x,y))
            if (x,y)==goal:
                return t
            else:
                for d in D:
                    x_=x+d[0]
                    y_=y+d[1]
                    if x_>=0 and x_<m and y_>=0 and y_<n:
                        if map_[x_][y_]!="#" and not (x_,y_) in visited:
                            t_=t+abs(int(map_[x][y])-int(map_[x_][y_]))
                            heappush(q, (t_,x_,y_))
        else:
            pass
    return "NO"

m,n,p=map(int,input().split())
map_=[]
for x in range(m):
    l=list(input().split())
    map_.append(l)
for i in range(p):
    l=list(map(int,input().split()))
    start=(l[0],l[1])
    goal=(l[2],l[3])
    if map_[l[0]][l[1]]=="#" or map_[l[2]][l[3]]=="#":
        print("NO")
    else:
        print(bfs(start, goal, map_, m, n))

```





### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：上网学习了Prim算法，发觉它同Dijkstra有异曲同工之妙：Dijkstra每次挑选离出发点最近的点进行广度优先搜索，Prim每次挑选距离生成树最近的点进行连接



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Wed May  1 20:05:08 2024

@author: liumi
"""

n=int(input())
graph={chr(65+i):[] for i in range(n)}
for _ in range(n-1):
    l=list(input().split())
    while len(l)>2:
        weight=int(l.pop())
        vertex=l.pop()
        graph[chr(65+_)].append((weight,vertex))
        graph[vertex].append((weight,chr(65+_)))

def prim(graph,n):
    dist=[float("inf") for i in range(n)]
    v=set()
    p=set(graph.keys())
    v.add("A")
    dist[0]=0
    for i in graph["A"]:
        dist[ord(i[1])-65]=i[0]
    while v!=p:
        rest=p-v
        rest_dist={i:dist[ord(i)-65] for i in rest}
        new=min(rest_dist,key=lambda x:rest_dist[x])
        v.add(new)
        for i in graph[new]:
            if not i[1] in v and dist[ord(i[1])-65]>i[0]:
                dist[ord(i[1])-65]=i[0]
    return sum(dist)

print(prim(graph, n))

```





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	本周作业还是很有难度，但是其实收获很大。发现自己和别人的差距部分在于不够了解标准库里面的数据结构和用法，还须多加努力。没有按时交六题则是因为自己没有安排好时间orz

