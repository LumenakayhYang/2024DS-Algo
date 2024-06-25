# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

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

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：如果直接模拟的话可能也可以做，但是听同学说会超时。于是自己想了两个样例来找这里的规律，最后是用栈实现了计算深度。有意思的事情是，所有操作可以被分为"d","ud","uu","u"共四种情况。遇到第一种，无论是原来的树还是转化后的树，当前深度都加1；第二种，原来的树当前深度不变，转换后的树深度加1；第三种和第四种需要用到栈，这个栈是用来记录路径上节点在转换后的树中的深度的，遇到"uu"，把之前所有的由"ud"产生的节点弹出至栈顶为"d"产生的节点，再把这个节点也弹出，这样相当于把原来的树中对应子树的这一层全部弹出。遇到"u",就弹出栈顶。（我应该没有说得太明白，真是抱歉，本人表达能力欠佳）



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr 19 18:49:10 2024

@author: liumi
"""

path=list(input())
layer=0
ord_depth,pre_depth=0,0
ord_layer,pre_layer=0,0
last_state=None
stack_depth=[]
stack_state=[]
has_poped=0
for step in path:
    if step=="d":
        ord_layer+=1
        if stack_depth!=[]:
            pre_layer=stack_depth[-1]+1
        else:
            pre_layer=1
        ord_depth=max(ord_depth,ord_layer)
        pre_depth=max(pre_depth,pre_layer)
        stack_depth.append(pre_layer)
        if last_state=="u":
            stack_state.append("ud")
        else:
            stack_state.append("d")
        last_state="d"
    else:
        ord_layer-=1
        if last_state=="u":
            if has_poped:
                stack_depth.pop()
                stack_state.pop()
            else:
                while stack_state[-1]=="ud":
                    stack_depth.pop()
                    stack_state.pop()
                stack_depth.pop()
                stack_state.pop()
                has_poped=1
        else:
            has_poped=0
            last_state="u"
print(f"{ord_depth} => {pre_depth}")

```

耗时约1h30min





### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：有点发懒，不想用别的办法了，直接模拟建棵树吧。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr 19 20:28:05 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None

def build_a_tree(lst):
    stack=[]
    poped=0
    for i in lst:
        node=Node(i)
        try:
            parent=stack.pop()
            while parent.right:
                parent=stack.pop()
            if not parent.left:
                parent.left=node
            else:
                parent.right=node
            if i==".":
                stack.append(parent)
            else:
                stack.append(parent)
                stack.append(node)
        except IndexError:
            stack.append(node)
    return stack[0]

def mid_print_a_tree(node):
    
    if node.name==".":
        return ""
    else:
        return mid_print_a_tree(node.left)+node.name+mid_print_a_tree(node.right)

def last_print_a_tree(node):
    
    if node.name==".":
        return ""
    else:
        return last_print_a_tree(node.left)+last_print_a_tree(node.right)+node.name


lst=list(input())
tree=build_a_tree(lst)
print(mid_print_a_tree(tree))
print(last_print_a_tree(tree))

```

耗时约30min





### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：使用两个栈，一个堆猪，一个记录当前最小值。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Apr 19 09:00:26 2024

@author: liumi
"""

class Pig_stack:
    
    def __init__(self):
        self.pigs=[]
        self.min_pig=[]
    
    def _push(self,n):
        self.pigs.append(n)
        try:
            self.min_pig.append(min(n,self.min_pig[-1]))
        except IndexError:
            self.min_pig.append(n)
    
    def _pop(self):
        try:
            self.pigs.pop()
            self.min_pig.pop()
        except IndexError:
            pass
    
    def _min(self):
        try:
            print(self.min_pig[-1])
        except IndexError:
            pass

def main():
    
    Pigs=Pig_stack()
    
    while True:
        try:
            cmd=input().split()
            if cmd[0]=="pop":
                Pigs._pop()
            if cmd[0]=="min":
                Pigs._min()
            if cmd[0]=="push":
                Pigs._push(int(cmd[1]))
        except EOFError:
            break

main()

```

耗时15min





### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：是一道深度优先搜索的题目，并无非常特别的地方。但是我在写的时候没理解应该在什么时候标记已访问节点，所以稍微看了一眼题解。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 21 11:02:40 2024

@author: liumi
"""

def dfs(x,y):
    
    global visited
    global n
    global m
    global D
    global cnt

    if len(visited)==n*m:
        return True
    else:
        for d in D:
            x_=x+d[0]
            y_=y+d[1]
            if not (x_,y_) in visited and  x_>=0 and x_<=n-1 and y_>=0 and y_<=m-1:
                visited.append((x_,y_))
                if dfs(x_, y_):
                    cnt+=1
                visited.pop()
                
D=[(1,2),(1,-2),(-1,2),(-1,-2),(2,1),(2,-1),(-2,1),(-2,-1)]
t=int(input())
for i in range(t):
    n,m,x,y=map(int,input().split())
    visited=[(x,y)]
    cnt=0
    dfs(x,y)
    print(cnt)

```

耗时约1h





### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：难度主要是在建图，广度优先搜索倒没有很难写。原先建图的时候，添加边是逐个添加的，这一步骤的时间复杂度直接变成O(n^2^)，于是超时，在同学指导下采用了正则表达式来建图，快了很多。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 21 13:09:53 2024

@author: liumi
"""

class Vertex:
    
    def __init__(self,name):
        self.name=name
        self.nbr=[]
        self.visited=-1
        self.upper=None

class Graph:
    
    def __init__(self):
        self.vertex_list={}
        self.queue=[]
        self.mate={}
    
    def add_edge(self,vertex):
        for i in range(4):
            pattern=vertex[:i]+"_"+vertex[i+1:]
            if pattern in self.mate:
                for other in self.mate[pattern]:
                    other.nbr.append(self.vertex_list[vertex])
                    self.vertex_list[vertex].nbr.append(other)
                self.mate[pattern].append(self.vertex_list[vertex])
            else:
                self.mate[pattern]=[self.vertex_list[vertex]]
    
    def add_vertex(self,vertex_name):
        vertex=Vertex(vertex_name)
        self.vertex_list[vertex_name]=vertex
        self.add_edge(vertex_name)
    
    def bfs(self,start,goal):
        
        self.queue.append(self.vertex_list[start])
        while self.queue!=[]:
            current=self.queue.pop(0)
            current.visited=1
            if current.name==goal:
                path=[current.name]
                while current.upper:
                    path.append(current.upper.name)
                    current=current.upper
                path.reverse()
                return " ".join(x for x in path)
            for neibour in current.nbr:
                if neibour.visited==-1:
                    self.queue.append(neibour)
                    neibour.upper=current
                    neibour.visited=0
        return "NO"

n=int(input())
graph=Graph()
vertex=[]
for i in range(n):
    word=input()
    vertex.append(word)
    graph.add_vertex(word)
start,goal=input().split()
if not start in vertex or not goal in vertex:
    print("NO")
else:
    print(graph.bfs(start, goal))

```

耗时约1h30min





### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：马走日的Pro Max版本，看了课件里面的Warnsdroff算法，感觉很有意思也感觉比较迷惑。为什么这样一个很像贪心的算法能很快找出是否有解？如果遇到无解的情况是不是也要全部搜一遍才能判断？



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 21 14:47:26 2024

@author: liumi
"""

class Vertex:
    
    def __init__(self,name):
        self.name=name
        self.nbr=[]
        self.state=0

class Graph:
    
    def __init__(self):
        self.vertex_list={}
    
    def add_vertex(self,vertex_name):
        vertex=Vertex(vertex_name)
        self.vertex_list[vertex_name]=vertex
    
    def add_edge(self,v1,v2):
        if not v1 in self.vertex_list:
            self.add_vertex(v1)
        if not v2 in self.vertex_list:
            self.add_vertex(v2)
        self.vertex_list[v1].nbr.append(self.vertex_list[v2])
        self.vertex_list[v2].nbr.append(self.vertex_list[v1])

def Create_A_Graph(n):
    G=Graph()
    D=[(1,2),(1,-2),(-1,2),(-1,-2),(2,1),(2,-1),(-2,1),(-2,-1)]
    for x in range(n):
        for y in range(n):
            if not y*n+x in G.vertex_list:
                G.add_vertex(y*n+x)
            for d in D:
                x_=x+d[0]
                y_=y+d[1]
                if x_>=0 and x_<n and y_>=0 and y_<n:
                    G.add_edge(y*n+x, y_*n+x_)
    return G

def dfs(depth,x,y,n,G):
    G.vertex_list[y*n+x].state=1
    if depth<n**2-1:
        neighbor=get_visitable(y*n+x,G)
        for neighbor_ in neighbor:
            x_=neighbor_.name%n
            y_=neighbor_.name//n
            if dfs(depth+1,x_,y_,n,G):
                return True
        else:
            G.vertex_list[y*n+x].state=0
            return False
    else:
        return True

def get_visitable(_id,G):
    visitable_neighbor=[]
    for neighbor in G.vertex_list[_id].nbr:
        if neighbor.state==0:
            visitable_neighbor.append(neighbor)
    visitable_neighbor.sort(key=visitable_neighbor_number)
    return visitable_neighbor

def visitable_neighbor_number(vertex):
    cnt=0
    for neighbor in vertex.nbr:
        if neighbor.state==0:
            cnt+=1
    return cnt

n=int(input())
G=Create_A_Graph(n)
x,y=map(int,input().split())
if dfs(0,x,y,n,G):
    print("success")
else:
    print("fail")

```

查看了课件，耗时约1h30min



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	学到图以后强度一直很高，写数算作业的时间大幅增加。比如本周就花了6个多小时，还有两题查看了题解。距离五一还有一周，希望九天的假期能够弥补一些因为过长的期中季落下的内容。

