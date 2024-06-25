# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

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

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：比较常规，使用了一个实时更新的列表来存放该层最后一个被访问的节点值。



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Thu May 23 10:10:42 2024

@author: liumi
"""

from collections import deque

class Node:
    
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def build_a_tree(n,node_list):
    for i in range(n):
        left,right=map(int,input().split())
        if left!=-1:
            left-=1
            node_list[i].left=node_list[left]
        if right!=-1:
            right-=1
            node_list[i].right=node_list[right]
    return node_list[0]

def main():
    n=int(input())
    node_list=[Node(i+1) for i in range(n)]
    tree_root=build_a_tree(n, node_list)
    queue=deque([(tree_root,0)])
    result=[]
    while queue:
        node,height=queue.popleft()
        try:
            result[height]=node.value
        except IndexError:
            result.append(node.value)
        for child in (node.left,node.right):
            if child:
                queue.append((child,height+1))
    return " ".join(str(x) for x in result)

print(main())

```

耗时17min



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Thu May 23 10:33:02 2024

@author: liumi
"""

def main():
    n=int(input())
    lst=list(map(int,input().split()))
    result=[0 for i in range(n)]
    stack=[]
    for i in range(n):
        if stack and stack[-1][0]<lst[i]:
            while stack and stack[-1][0]<lst[i]:
                result[stack[-1][1]]=i+1
                stack.pop()
        stack.append((lst[i],i))
    print(*result)

main()

```





### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：其实本来是一道简单dfs。但是我在写的时候没有特别注意return的性质导致WA了。最后只好求助于GPT。只能多练了。



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Fri May 24 23:39:39 2024

@author: liumi
"""

class Node:
    
    def __init__(self,value):
        self.value=value
        self.child=[]
    
class Graph:
    
    def __init__(self,n):
        self.node_list=[Node(i+1) for i in range(n)]
    
    def add_edge(self,v1,v2):
        v1-=1
        v2-=1
        self.node_list[v1].child.append(v2)
    
    def dfs(self,v,visited,general_visited):
        visited.add(v)
        general_visited.add(v)
        if self.node_list[v].child != []:
            for i in self.node_list[v].child:
                if i in visited:
                    return True
                elif i not in general_visited:  # Modify this condition
                    if self.dfs(i, visited, general_visited):
                        return True
        visited.remove(v)
        return False
    
    def find_loop(self):
        general_visited=set()
        for node in self.node_list:
            if node.value-1 not in general_visited:
                if self.dfs(node.value-1, set(), general_visited):
                    return True
        return False


def function():
    n,m=map(int,input().split())
    G=Graph(n)
    for i in range(m):
        v1,v2=map(int,input().split())
        G.add_edge(v1, v2)
    if G.find_loop():
        print("Yes")
    else:
        print("No")

def main():
    t=int(input())
    for _ in range(t):
        function()

main() 

```







### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：这题完全没想到能用对要求的值用二分法来确定。刚开始还以为是动态规划，但是仔细一想感觉凭自己写不出来。查看题解发现前人栽了很好的树，于是我就坐在树下乘凉了。这题也算是拓展了我解题的一些思路吧



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Sat May 25 15:49:00 2024

@author: liumi
"""

n,m=map(int,input().split())
lst=[]
for i in range(n):
    lst.append(int(input()))
lb,hb=max(lst),sum(lst)
def check(value,lst,m):
    temp,fajo_num=0,1
    for i in lst:
        temp+=i
        if temp>value:
            fajo_num+=1
            temp=i
    return fajo_num<=m
mid=(lb+hb)//2
while lb<hb:
    if check(mid, lst, m):
        hb=mid
        mid=(lb+hb)//2
    else:
        lb=mid+1
        mid=(lb+hb)//2
    ans=mid
print(hb)
```





### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：使用Dijkstra算法可以解决。由于可能有到达的多条路径，所以其实visited需要被改进成保存最小值才对（但是这里直接删去visited也能过，删去可以避免一条路走不通的时候不走其他路）



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Sat May 25 13:15:59 2024

@author: liumi
"""
from heapq import heappop,heappush
class Graph:
    
    def __init__(self,n):
        self.node_list={i:[] for i in range(n+1)}
        self.goal=n
    
    def add_edge(self):
        s,d,l,t=map(int,input().split())
        self.node_list[s].append((l,t,d))
    
    def find_a_road(self,money):
        queue=[]
        heappush(queue, (0,0,1))
        visited=set()
        while queue:
            length,fee,city=heappop(queue)
            if city==self.goal:
                return length
            for next_city in self.node_list[city]:
                if fee+next_city[1]<=money:
                    heappush(queue, (next_city[0]+length,next_city[1]+fee,next_city[2]))
        return -1

def main():
    money=int(input())
    n=int(input())
    _map=Graph(n)
    m=int(input())
    for i in range(m):
        _map.add_edge()
    print(_map.find_a_road(money))

main()

```





### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：完全被各种捕食关系绕晕了，只好查看题解的做法。这相当于一个有元素间相互关系的并查集。不得不说，答案用一个长度3n+1的列表来存每个动物对应的食物和天敌的做法是巧妙的。



代码

```python
# -*- coding: utf-8 -*-
"""
Created on Sat May 25 13:52:55 2024

@author: liumi
"""
def get_parent(x,lst):
    if lst[x]!=x:
        lst[x]=get_parent(lst[x], lst)
    return lst[x]

    
n,k=map(int,input().split())
lst=[i for i in range(3*n+1)] #1~n:自身种类; n+1~2n:食物; 2n+1~3n:天敌
ans=0
for i in range(k):
    cmd,x,y=map(int,input().split())
    if x>n or y>n:
        ans+=1
        continue
    if cmd==1:
        if get_parent(x, lst)==get_parent(y+n, lst) or get_parent(x+n, lst)==get_parent(y, lst):
            ans+=1
            continue
        lst[get_parent(x, lst)]=get_parent(y, lst)
        lst[get_parent(x+n, lst)]=get_parent(y+n, lst)
        lst[get_parent(x+2*n, lst)]=get_parent(y+2*n, lst)
    else:
        if get_parent(x, lst)==get_parent(y, lst) or get_parent(y+n, lst)==get_parent(x, lst):
            ans+=1
            continue
        lst[get_parent(x+n, lst)]=get_parent(y, lst) 
        lst[get_parent(y+2*n, lst)]=get_parent(x, lst)
        lst[get_parent(x+2*n, lst)]=get_parent(y+n, lst) #成环关键：x的天敌是y的食物
print(ans)

```





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	感觉到本周的题目兼具模板性和综合性，很适合拿来复习。第四题“月度开销”真是震撼，居然可以用二分法来找解。第六题“食物链”也挺复杂，平常没做过的很难想到思路。考试要是遇到这样的题可能要断舍离了。其他题目还好，因为基本上是模板，不是特别的困难。

