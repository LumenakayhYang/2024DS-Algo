# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by 杨英浩 生命科学学院 2300012284



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows11

Python编程环境：Spyder IDE 5.4.3

C/C++编程环境：无

## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：得到管骏杰同学的思路提示，根据中序表达式和先序表达式建树。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 30 15:13:46 2024

@author: liumi

with Guan's solution
"""

class Node:
    
    def __init__(self,key,left,right):
        self.key=key
        self.left=left
        self.right=right

def build_a_tree(front,mid):
    
    if front==mid:
        if front==[]:
            return None
        elif len(front)==1:
            return front[0]
    
    root=front.pop(0)
    root_index=mid.index(root)
    left_front=front[:root_index]
    left_mid=mid[:root_index]
    left_tree=build_a_tree(left_front,left_mid)
    right_front=front[root_index:]
    right_mid=mid[root_index+1:]
    right_tree=build_a_tree(right_front,right_mid)
    
    return Node(root, left_tree, right_tree)

def print_out(tree):
    if tree==None:
        return []
    elif type(tree)==int:
        return [str(tree)]
    
    s=[]
    s+=print_out(tree.left)
    s+=print_out(tree.right)
    s+=[str(tree.key)]
    return s

n=int(input())
front=list(map(int,input().split()))
mid=[i for i in range(1,n+1)]
s=print_out(build_a_tree(front, mid))
print(" ".join(str(i) for i in s))

```

耗时约30min



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：先按照二叉搜索树的定义建树，然后用队列实现其层次遍历



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 30 16:01:14 2024

@author: liumi
"""

class Node:
    
    def __init__(self,key):
        self.key=key
        self.left=False
        self.right=False
    

def find_and_insert(node,num):
    
    if int(node.key)>num:
        if not node.left:
            node.left=Node(str(num))
        else:
            find_and_insert(node.left, num)
    elif int(node.key)<num:
        if not node.right:
            node.right=Node(str(num))
        else:
            find_and_insert(node.right, num)
    else:
        node.key=str(num)

def build_a_tree(lst):
    root=Node(str(lst[0]))
    for i in range(1,len(lst)):
        find_and_insert(root, lst[i])
    return root

def print_layers_out(tree):
    s=[]
    queue=[tree]
    while queue!=[]:
        node=queue.pop(0)
        s.append(node.key)
        if not node.left:
            pass
        else:
            queue.append(node.left)
        if not node.right:
            pass
        else:
            queue.append(node.right)
    return s
        
lst=list(map(int,input().split()))
tree=build_a_tree(lst)
outlist=print_layers_out(tree)
print(" ".join(i for i in outlist))


```





耗时约40min



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：基本上是按照教参里面的办法搓了一个堆结构，主要实现了插入和删除最小值操作



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Mar 31 13:11:24 2024

@author: liumi
"""

class Binheap:
    
    def __init__(self):
        self.heap=[]
    
    def insert(self,i):
        self.heap.append(i)
        index=len(self.heap)-1
        while (index-1)//2>=0:
            parent_index=(index-1)//2
            if self.heap[parent_index]>self.heap[index]:
                self.heap[parent_index],self.heap[index]=(self.heap[index],self.heap[parent_index])
            index=parent_index
    
    def get_min_child(self,i):
        if i*2+2>len(self.heap)-1:
            return i*2+1
        if self.heap[i*2+1]<self.heap[i*2+2]:
            return i*2+1
        return i*2+2
    
    def delete(self):
        self.heap[-1],self.heap[0]=(self.heap[0],self.heap[-1])
        min_=self.heap.pop()
        i=0
        while 2*i+1<len(self.heap):
            min_child=self.get_min_child(i)
            if self.heap[i]>self.heap[min_child]:
                self.heap[min_child],self.heap[i]=(self.heap[i],self.heap[min_child])
            else:
                break
            i=min_child

        print(min_)

n=int(input())
binheap=Binheap()
for i in range(n):
    s=list(map(int,input().split()))
    if s[0]==2:
        binheap.delete()
    else:
        binheap.insert(s[1])

```

耗时约60min





### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：上网搜了哈夫曼编码的概念，直接照着概念来做了。因为不考虑效率大量使用了列表的sort方法，后来经过同学提示才想到也可以利用堆结构来实现，在合并两个元素的时候进行堆的操作。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Mar 31 14:13:54 2024

@author: liumi
"""

class Haffman_Tree_Node:
    
    def __init__(self,key,left,right,weight):
        self.key=key
        self.left=left
        self.right=right
        self.weight=weight


def Build_A_Tree():
    n=int(input())
    forest=[]
    for i in range(n):
        s=input().split()
        s[1]=int(s[1])
        node=Haffman_Tree_Node(set(s[0]), False, False, s[1])
        forest.append(node)
    forest.sort(key=lambda x:(x.weight,x.key))
    forest.reverse()
    while len(forest)>1:
        left=forest.pop()
        right=forest.pop()
        weight=left.weight+right.weight
        key=left.key|right.key
        node=Haffman_Tree_Node(key, left, right, weight)
        forest.append(node)
        forest.sort(key=lambda x:(x.weight,x.key))
        forest.reverse()
    return forest.pop()

def unlock_num(tree,num):
    s=""
    command=num.pop()
    if command=="0":
        if len(tree.left.key)==1:
            letter=iter(tree.left.key)
            return s+next(letter)
        else:
            return s+unlock_num(tree.left, num)
    elif command=="1":
        if len(tree.right.key)==1:
            letter=iter(tree.right.key)
            return s+next(letter)
        else:
            return s+unlock_num(tree.right, num)

def printable_num(tree,num):
    s=""
    while num!=[]:
        s+=unlock_num(tree, num)
    print(s)

def unlock_str(tree,letter):
    if letter in tree.left.key:
        if len(tree.left.key)==1:
            return "0"
        else:
            return "0"+unlock_str(tree.left, letter)
    elif letter in tree.right.key:
        if len(tree.right.key)==1:
            return "1"
        else:
            return "1"+unlock_str(tree.right, letter)

def printable_str(tree,lst):
    s=""
    while lst!=[]:
        letter=lst.pop()
        s+=unlock_str(tree, letter)
    print(s)

tree=Build_A_Tree()
while True:
    try:
        input_=input()
        try:
            a=int(input_)
            num=list(input_)
            num.reverse()
            printable_num(tree, num)
        except ValueError:
            lst=list(input_)
            lst.reverse()
            printable_str(tree, lst)
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：各种各样的旋转看得我的头很晕，所以我跑去看了题解。题解在每一次返回递归的时候“顺便”旋转来维护二叉树的平衡，非常美妙。当然题解里面也有很多不起眼但很重要的细节，比如旋转之后高度的计算顺序，曾经因为顺序写反得出过令人匪夷所思的答案



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon Apr  1 19:47:01 2024

@author: liumi
"""

class Node:
    
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None
        self.height=1

class AVL:
    
    def __init__(self):
        self.root=None
    
    def get_height(self,node):
        if not node:
            return 0
        else:
            return node.height
    
    def get_balance(self,node):
        if not node:
            return 0
        else:
            return self.get_height(node.left)-self.get_height(node.right)
    
    def rotate_left(self,z):
        y=z.right
        temp=y.left
        y.left=z
        z.right=temp
        z.height=1+max(self.get_height(z.left),self.get_height(z.right))
        y.height=1+max(self.get_height(y.left),self.get_height(y.right))
        return y
    
    def rotate_right(self,y):
        x=y.left
        temp=x.right
        x.right=y
        y.left=temp
        y.height=1+max(self.get_height(y.left),self.get_height(y.right))
        x.height=1+max(self.get_height(x.left),self.get_height(x.right))
        return x
        
    def insert(self,value):
        if not self.root:
            self.root=Node(value)
        else:
            self.root=self.insert_(value,self.root)
    
    def insert_(self,value,node):
        if not node:
            return Node(value)
        elif value<node.value:
            node.left=self.insert_(value, node.left)
        else:
            node.right=self.insert_(value, node.right)
        
        node.height=1+max(self.get_height(node.left),self.get_height(node.right))
        
        balance=self.get_balance(node)
        
        if balance>1:
            if value<node.left.value:
                return self.rotate_right(node)
            else:
                node.left=self.rotate_left(node.left)
                return self.rotate_right(node)
        
        if balance<-1:
            if value>node.right.value:
                return self.rotate_left(node)
            else:
                node.right=self.rotate_right(node.right)
                return self.rotate_left(node)
        
        return node
    
    def _preorder(self,node):
        if not node:
            return []
        else:
            return [node.value]+self._preorder(node.left)+self._preorder(node.right)
        
    def preorder(self):
        return self._preorder(self.root)
    
n=int(input())
lst=list(map(int,input().split()))
avl=AVL()
for i in range(n):
    avl.insert(lst[i])
print(" ".join(str(x) for x in avl.preorder()))


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：刚开始用一种很奇怪的搜索算法来写，但是一直报RE；之后听同学提示用并查集，查了并查集的定义，最后因为没有对子节点向父节点的索引进行优化报了MLE.不得已查看了题解,至少学到了并查集的模板化写法



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Mon Apr  1 12:20:23 2024

@author: liumi
"""

def get_father(x,lst):
    if lst[x]!=x:
        lst[x]=get_father(lst[x], lst)
    return lst[x]

def join_together(x,y,lst):
    fx=get_father(x, lst)
    fy=get_father(y, lst)
    if fx==fy:
        return
    lst[fx]=fy

case=0
while True:
    n,m=map(int,input().split())
    case+=1
    if (n,m)==(0,0):
        break
    else:
        lst=[i for i in range(n+1)]
        for i in range(m):
            x,y=map(int,input().split())
            join_together(x, y, lst)
        cnt=0
        for j in range(1,n+1):
            if lst[j]==j:
                cnt+=1
        print(f'Case {case}: {cnt}')

```





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	难度好大,想利用清明节假期再好好养一养小树.

