# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

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

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：定义Vertex类和Graph类，在输入时先在Graph里设置节点，然后再单纯地建立起边。输出时，若横纵坐标相同就输出对应节点的度，若不同就检查边是否存在。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Thu Apr 11 16:09:44 2024

@author: liumi
"""

class Vertex:
    
    def __init__(self,key):
        self.id=key
        self.nbr=[]
        self.nbr_num=0
        
    def add_nbr(self,neighbor):
        self.nbr.append(neighbor)
        self.nbr_num+=1

    def get_id(self):
        return self.id
    
    def get_nbr(self):
        nbr_list=[]
        for i in self.nbr:
            nbr_list.append(i.get_id())
        return nbr_list

class Graph:
    
    def __init__(self):
        self.vertlist={}
    
    def addvertex(self,vertex):
        self.vertlist[vertex]=Vertex(vertex)
        
    def addedge(self,one,two):
        if not one in self.vertlist:
            self.addvertex(one)
        if not two in self.vertlist:
            self.addvertex(two)
        self.vertlist[one].add_nbr(self.vertlist[two])
        self.vertlist[two].add_nbr(self.vertlist[one])

G=Graph()
n,m=map(int,input().split())
if m!=0:
    for i in range(n):
        G.addvertex(i)
    for i in range(m):
        one,two=map(int,input().split())
        G.addedge(one, two)
    result=[]
    for i in range(n):
        r0=[]
        neighbor=G.vertlist[i].get_nbr()
        for j in range(n):
            if j==i:
                r0.append(G.vertlist[i].nbr_num)
            else:
                if not j in neighbor:
                    r0.append(0)
                else:
                    r0.append(-1)
        result.append(r0)
    for k in range(n):
        print(" ".join(str(x) for x in result[k]))
elif m==0:
    for i in range(n):
        r=[0 for j in range(n)]
        print(" ".join(str(x) for x in r))

```

耗时约1h（因为是第一次写图）



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：使用的是深度优先搜索（在我个人认知里），并且使用一个列表来存储已经访问过的点以避免重复搜索。

优化的力度还不够，不过也还没有试过广度优先搜索的写法



代码

```python

# -*- coding: utf-8 -*-
"""
Created on Thu Apr 11 23:22:26 2024

@author: liumi
"""

def DFS(G,i,j):
    
    global have_searched
    
    have_searched.append((i,j))
    
    if G[j][i]==".":
        return 0
    else:
        G[j][i]=="."
        S=1
        for k in range(3):
            for h in range(3):
                i+=dx[h]
                j+=dy[k]
                if i>=0 and i<m and j>=0 and j<n and not (i,j) in have_searched:
                    S+=DFS(G,i, j)
                i-=dx[h]
                j-=dy[k]
        return S

t=int(input())
dx=[-1,0,1]
dy=[-1,0,1]
for i in range(t):
    n,m=map(int,input().split())
    max_=-1
    Graph=[]
    have_searched=[]
    for j in range(n):
        l=list(input())
        Graph.append(l)
    for j in range(n):
        for k in range(m):
            if not (k,j) in have_searched:
                max_=max(max_,DFS(Graph, k, j))
    print(max_)

```

耗时约1h（因为深搜debug花了很长时间，以后熟练了应该不会那么久）



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：继续定义Vertex类和Graph类来实现，在Graph类中定义了一个静态方法来实现同一连通块权值的相加。按我个人理解好像还是用的深搜，虽然意识到用并查集应该会更方便。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Apr 13 19:50:26 2024

@author: liumi
"""

class Vertex:
    
    def __init__(self,key,value):
        self.key=key
        self.value=value
        self.neighbor=[]
        self.id=key
        
    def add_neighber(self,neighbor_):
        self.neighbor.append(neighbor_)
    
class Graph:
    
    @staticmethod
    def sum_up(has_found,vertex):
        if vertex in has_found:
            return 0
        else:
            has_found.append(vertex)
            sum_=vertex.value
            for child in vertex.neighbor:
                if not child in has_found:
                    sum_+=Graph.sum_up(has_found, child)
            return sum_

    def __init__(self):
        self.verlist={}
    
    def add_edge(self,key1,key2):
        self.verlist[key1].neighbor.append(self.verlist[key2])
        self.verlist[key2].neighbor.append(self.verlist[key1])
    
    def get_max_weight(self):
        have_found=[]
        max_=-1
        for vertex_key in self.verlist:
            max_=max(max_,Graph.sum_up(have_found, self.verlist[vertex_key]))
        print(max_)

n,m=map(int,input().split())
G=Graph()
value_list=list(map(int,input().split()))
for i in range(n):
    G.verlist[i]=Vertex(i, value_list[i])
for i in range(m):
    k1,k2=map(int,input().split())
    G.add_edge(k1, k2)
G.get_max_weight()

```

耗时约45min



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：因为之前用了三次循环导致超时，实在没想法去看了题解，原来这是一个可以用两遍循环就解决的问题



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 14 22:11:54 2024

@author: liumi
"""

n=int(input())
A,B,C,D,cnt=[],[],[],[],0
for i in range(n):
    l=list(map(int,input().split()))
    A.append(l[0])
    B.append(l[1])
    C.append(l[2])
    D.append(l[3])
sum_of_AB={}
for i in A:
    for j in B:
        if not i+j in sum_of_AB:
            sum_of_AB[i+j]=1
        else:
            sum_of_AB[i+j]+=1
for i in C:
    for j in D:
        if -(i+j) in sum_of_AB:
            cnt+=sum_of_AB[-(i+j)]
print(cnt)


```

耗时约30min，一半以上是在徒劳地优化



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：上网搜了一下Trie，在具体实现的时候发现确实用字典建立映射会很方便。判断插入失败的情况有很多，包括本次插入的字符未插完而已经走完当前路径、本次字符已经插入完毕但当前节点还有子节点等等，还要区分第一次插入和之后的插入。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Apr 14 22:51:05 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.name=name
        self.child={}

class Trie:
    
    def __init__(self):
        self.root=Node(-1)
        self.root.child[-1]={}
    
    def _insert(self,node,_str,i,m,first):
        if i==m-1:
            if _str[i] in node.child:
                return False
            else:
                if node.child!={}:
                    node.child[_str[i]]=Node(_str[i])
                    return True
                else:
                    if not first:
                        return False
                    else:
                        node.child[_str[i]]=Node(_str[i])
                        return True
        else:
            if _str[i] in node.child:
                return self._insert(node.child[_str[i]],_str,i+1,m,False)
            else:
                if node.child!={}:
                    node.child[_str[i]]=Node(_str[i])
                    return self._insert(node.child[_str[i]], _str, i+1, m,True)
                else:
                    if first:
                        node.child[_str[i]]=Node(_str[i])
                        return self._insert(node.child[_str[i]], _str, i+1, m,True)
                    else:
                        return False
n=int(input())
for i in range(n):
    t=int(input())
    T=Trie()
    success=True
    for j in range(t):
        s=input()
        m=len(s)
        if not T._insert(T.root, s, 0,m,True):
            success=False
    if success:
        print("YES")
    else:
        print("NO")

```

耗时约1h40min



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：这个题目确实不一般，做的时候能感觉到二叉树和树的区别。以输入的节点为依据，利用栈来建树。然后把伪满二叉树转换成原来的多叉树。由于在转换过程中使用了递归，得到的多叉树就是原多叉树的镜像，然后直接层次遍历输出即可。我的做法似乎并不需要节点数目n，只要给定伪满二叉树的节点列表就能建树和转换。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon Apr 15 10:48:29 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None
        self.parent=None

class TreeNode:
    
    def __init__(self,name):
        self.name=name
        self.child=[]
    
def build_a_fake_binary_tree(lst):
    stack=[]
    for i in lst:
        if i[1]=="0":
            node=Node(i[0])
            if stack!=[]:
                prt_node=stack.pop()
                if not prt_node.left:
                    node.parent=prt_node
                    prt_node.left=node
                    stack.append(prt_node)
                    stack.append(node)
                elif not prt_node.right:
                    node.parent=prt_node
                    prt_node.right=node
                    stack.append(prt_node)
                    stack.append(node)
                else:
                    while prt_node.right:
                        prt_node=stack.pop()
                    node.parent=prt_node
                    prt_node.right=node
                    stack.append(prt_node)
                    stack.append(node)
            else:
                stack.append(node)
        else:
            node=Node(i[0])
            prt_node=stack.pop()
            if not prt_node.left:
                node.parent=prt_node
                prt_node.left=node
                stack.append(prt_node)
            elif not prt_node.right:
                node.parent=prt_node
                prt_node.right=node
                stack.append(prt_node)
            else:
                while prt_node.right:
                    prt_node=stack.pop()
                node.parent=prt_node
                prt_node.right=node
                stack.append(prt_node)
    if len(stack)==1:
        return stack.pop()

def turn_a_fake_tree_to_a_tree(root,nodes):
    node=TreeNode(root.name)
    nodes[node.name]=node
    if not root.left and not root.right:
        return node
    elif root.left.name!="$" and root.right.name=="$":
        node.child.append(turn_a_fake_tree_to_a_tree(root.left, nodes))
        return node
    elif root.left.name=="$" and root.right.name!="$":
        root.right.parent=root.parent
        nodes[root.parent.name].child.append(turn_a_fake_tree_to_a_tree(root.right, nodes))
        return node
    elif root.left.name!="$" and root.right.name!="$":
        root.right.parent=root.parent
        node.child.append(turn_a_fake_tree_to_a_tree(root.left, nodes))
        nodes[root.parent.name].child.append(turn_a_fake_tree_to_a_tree(root.right, nodes))
        return node

def print_out(queue):
    s=""
    while queue!=[]:
        node=queue.pop(0)
        s+=node.name
        if node.child!=[]:
            for i in node.child:
                queue.append(i)
        else:
            pass
    return s
n=int(input())
lst=input().split()
tree=build_a_fake_binary_tree(lst)
nodes={}
Tree=turn_a_fake_tree_to_a_tree(tree, nodes)
result=list(print_out([Tree]))
print(" ".join(x for x in result))

```

耗时约2h



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	一刻也没有为期中季的到来感到悲伤，接下来到达战场上的是图结构！这周主要是练习了图的写法和图的搜索遍历（当然广搜后面还要再花时间），顺便复习了树的一些知识。本周的题目都没有特别简单，一下子就能做对的那种，有很多地方很磨人，花的时间也不少。不过独立写出最后一题还是很有成就感的。

​	因为是期中季，每日选做已经成为天坑一般的存在，希望五一假期还有一些空闲时间来做这个。



