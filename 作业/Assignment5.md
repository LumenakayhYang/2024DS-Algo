# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by ==杨英浩 生命科学学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows11

Python编程环境：Spyder IDE 5.4.3

C/C++编程环境：无

## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：非常规的思路，已知值为[-1,-1]的节点为叶子节点，只要统计这样的结点的个数就能知道叶子节点个数；找深度是对于每一个节点都计算高度，为了避免重复计算，用一个字典seeked来储存已经计算过的结点的高度，之后统计一个最大值，相应的可以得到二叉树的高度。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Mar 22 13:20:45 2024

@author: liumi
"""

def seek(l,n):
    
    global seeked
    
    if n in seeked:
        return seeked[n]
    else:

        if l[n]==[-1,-1] or n==-1:
            seeked[n]=0
        else:
            seeked[n]=1+max(seek(l,l[n][0]),seek(l,l[n][1]))
        return seeked[n]

n=int(input())
l,seeked,leaf=[],{},0
for i in range(n):
    in_=list(map(int,input().split()))
    if in_==[-1,-1]:
        leaf+=1
    l.append(in_)

for j in range(n):
    _=seek(l,j)
m=-1
for k in seeked:
    m=max(m,seeked[k])
print(" ".join(str(x) for x in (m,leaf)))

```



用时约40min

### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：还是非常规的思路，有点邪门，像是逃课一样的写法。前序遍历序列只要忽略所有符号就可以得到，后序遍历用栈可以实现直接输出。因为实在太短后来重新用建树的办法写了一遍，以此巩固对树的写法的理解。



代码

```python
# doesn't Build A Tree
# # -*- coding: utf-8 -*-
"""
Created on Fri Mar 22 14:11:56 2024

@author: liumi
"""

tree=list(input())
s1=""
for i in tree:
    if not i in "(),":
        s1+=i
s2,stack="",[]
for j in tree:
    if j==",":
        s2+=stack.pop()
    elif j==")":
        while stack[-1]!="(":
            s2+=stack.pop()
        stack.pop()
    else:
        stack.append(j)
print(s1)
print(s2)

#Build A Tree
# -*- coding: utf-8 -*-
"""
Created on Sun Mar 24 10:59:35 2024

@author: liumi
"""

class Node:
    
    def __init__(self,name):
        self.children=[]
        self.name=name
    
def build_a_tree(lst):
    stack=[]
    for i in lst:
        if i=="(":
            stack.append(i)
        elif i==")":
            child_list=[]
            while stack[-1]!="(":
                child_list.append(stack.pop())
            stack.pop()
            child_list.reverse()
            node=stack.pop()
            node.children=child_list
            stack.append(node)
        elif i==",":
            pass
        else:
            node=Node(i)
            stack.append(node)
    return stack[-1]
def print_out_last(node):
    
    if node.children==[]:
        return node.name
    
    s=""
    for i in node.children:
        s+=print_out_last(i)
    s+=node.name
    return s

def print_out_front(node):
    
    if node.children==[]:
        return node.name
    
    s=""
    s+=node.name
    for i in node.children:
        s+=print_out_front(i)
    return s

lst=list(input())
tree=build_a_tree(lst)
print(print_out_front(tree))
print(print_out_last(tree))
```





无建树版本，用时约20min

建树版本，用时约40min

### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：使用一种递归的思路建树和输出，这道题用递归的办法做至少看起来简单易行。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Fri Mar 22 18:46:14 2024

@author: liumi
"""

class FileTree:
    
    def __init__(self,name):
        self.d=[]
        self.f=[]
        self.name=name
    
    def get_file(self):
        return self.f

def Build_filetree(root_):
    
    global switch
    
    while True:
        s=input()
        if s[0]=="f":
            root_.f.append(s)
        elif s[0]=="d":
            _=FileTree(s)
            Build_filetree(_)
            root_.d.append(_)
        elif s[0]=="]":
            return root_
        elif s[0]=="#":
            switch=0
            break
        else:
            break
    return root_

def print_out(root_,n):
    global space
    for d_ in root_.d:
        print(space*n+d_.name)
        print_out(d_,n+1)
    a=root_.get_file()
    a.sort()
    root_.f=a
    for f_ in root_.f:
        print(space*(n-1)+f_)

i=0
while True:
    switch=1
    root=FileTree("ROOT")
    a=Build_filetree(root)
    i+=1
    if switch==1:
        space="|     "
        print(f"DATA SET {i}:")
        print("ROOT")
        print_out(root, 1)
        print("")
    else:
        break


```



总用时约60min，思考这两个递归的办法花了我比较长的时间

### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：根据后序表达式建树，然后按层次遍历得出结果（基本上就是提示的思路），但是我对于层次遍历的写法不是很清楚，上网搜了以后才知道二叉树的层次遍历可以用队列来做。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 23 21:01:09 2024

@author: liumi
"""

class Node:
    
    def __init__(self,first,second,name):
        self.first=first
        self.second=second
        self.name=name
    
    def get(self):
        queue=[]
        queue.append(self)
        result=""
        while queue!=[]:
            if type(queue[0])==Node:
                result+=queue[0].name
                queue.append(queue[0].first)
                queue.append(queue[0].second)
            else:
                result+=queue[0]
            queue.pop(0)
        return result[ : :-1]
    
def build_a_tree(lst):
    stack=[]
    for i in lst:
        if ord(i)>=97:
            stack.append(i)
        else:
            second=stack.pop()
            first=stack.pop()
            node=Node(first, second, i)
            stack.append(node)
    return stack[-1]

n=int(input())
for i in range(n):
    lst=list(input())
    tree=build_a_tree(lst)
    print(tree.get())

```





耗时约40min

### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：后序表达式的最后一个值必为根结点的值，在中序表达式里面找到这个值就可以区分出左子树和右子树，进而可以在后序表达式里面区分左子树和右子树。如此递归地做下去，最终可以得到一颗完整的树。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 23 23:40:06 2024

@author: liumi
"""

class Node:
    
    def __init__(self,left,right,name):
        self.left=left
        self.right=right
        self.name=name

def build_a_tree(mid,last):
    
    if mid==[] and last==[]:
        return False
    elif len(mid)==1 and mid==last:
        return mid[0]
    
    root=last.pop()
    root_index=mid.index(root)
    left_mid=mid[:root_index]
    left_last=last[:root_index]
    left_tree=build_a_tree(left_mid, left_last)
    right_mid=mid[root_index+1:]
    right_last=last[root_index:]
    right_tree=build_a_tree(right_mid, right_last)
    
    return Node(left_tree, right_tree, root)

def print_out(node):
    if type(node)!=Node:
        if not node:
            return ""
        else:
            return node
    s=""
    s+=node.name
    s+=print_out(node.left)
    s+=print_out(node.right)
    return s
        
mid=list(input())
last=list(input())
tree=build_a_tree(mid, last)
s=print_out(tree)
print(s)



```



耗时约70min

### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：和前面的题目一模一样，只是把后序换成了前序



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Mar 24 00:56:49 2024

@author: liumi
"""

class Node:
    
    def __init__(self,left,right,name):
        self.name=name
        self.left=left
        self.right=right
    
def build_a_tree(head,mid):
    
    if head==mid:
        if head==[]:
            return False
        elif len(head)==1:
            return head[0]
        
    root=head.pop(0)
    root_index=mid.index(root)
    left_head=head[:root_index]
    left_mid=mid[:root_index]
    left_tree=build_a_tree(left_head, left_mid)
    right_head=head[root_index:]
    right_mid=mid[root_index+1:]
    right_tree=build_a_tree(right_head, right_mid)
    
    return Node(left_tree, right_tree, root)

def print_out(node):
    
    if type(node)!=Node:
        if not node:
            return ""
        else:
            return node
    
    s=""
    s+=print_out(node.left)
    s+=print_out(node.right)
    s+=node.name
    
    return s

while True:
    try:
        head=list(input())
        mid=list(input())
        tree=build_a_tree(head, mid)
        s=print_out(tree)
        print(s)
    except EOFError:
        break

```



耗时约40min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	这周的作业都在建树，在对树的写法逐渐熟悉的同时也对树的各种概念有了理解，对遍历的实现方法有了一定的掌握。文件结构“图”这道题目让我摸到了递归的门道，开始慢慢熟悉上学期一直困扰我的递归的写法。因为作业实在太多没有补上每日选做，等到这学期课少一些的时候还要多下力气钻研。



