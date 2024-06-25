# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ==同学的姓名、院系==



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

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：用类定义一个双端队列，然后按照输入的指令进行操作



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 16 13:22:40 2024

@author: liumi
"""

class queue:
    
    def __init__(self):
        self._item=[]
    
    def add(self,n):
        self._item.append(n)
    
    def out(self,p):
        if p==1:
            self._item.pop()
        else:
            self._item.pop(0)
    
    def get(self):
        return self._item
    
t=int(input())
for i in range(t):
    n=int(input())
    a=queue()
    for j in range(n):
        command,value=map(int,input().split())
        if command==1:
            a.add(value)
        else:
            a.out(value)
    result=a.get()
    if result==[]:
        print("NULL")
    else:
        print(" ".join(str(x) for x in result))
    

```



用时约13min



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：先转换成逆波兰表达式，然后用非常经典的栈结构来实现运算



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 16 13:37:53 2024

@author: liumi
"""
def calculator(a,b,c):
    if c=="+":
        return a+b
    elif c=="-":
        return a-b
    elif c=="*":
        return a*b
    elif c=="/":
        return a/b
    
l=list(input().split())
stack=[]
l.reverse()
for i in l:
    if i in ["+","-","*","/"]:
        b=stack.pop()
        a=stack.pop()
        stack.append(calculator(b, a, i))
    else:
        stack.append(float(i))

print(f'{stack[0]:.6f}')

```



约20min，但是因为结果格式化输出的原因debug很久



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：几乎完全没有思路，只能把题解的思路复述一遍：遍历一遍输入，把遇到的数字直接加入结果中；遇到+-*/，则需要把栈中能够弹出的同级或者更高级运算符弹出，再将其压入栈中；遇到（，则需要将其压入栈中，；遇到），将栈中元素弹出至栈顶为（；按以上步骤遍历一遍输入的表达式，最后处理剩余的数字和顺序弹出运算符。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Mar 17 23:54:48 2024

@author: liumi
"""

def trans(ex):
    cal={"+":1,"-":1,"*":2,"/":2}
    result=[]
    stack=[]
    number=""
    
    for i in ex:
        if i.isnumeric() or i==".":
            number+=i
        else:
            if number:
                num=float(number)
                result.append(int(num) if num.is_integer() else num)
                number=""
            if i in "+-*/":
                while stack and stack[-1] in "+-*/" and cal[i]<=cal[stack[-1]]:
                    result.append(stack.pop())
                stack.append(i)
            elif i=="(":
                stack.append(i)
            elif i==")":
                while stack and stack[-1]!="(":
                    result.append(stack.pop())
                stack.pop()
    if number:
        num=float(number)
        result.append(int(num) if num.is_integer() else num)
    while stack!=[]:
        result.append(stack.pop())
            
    return " ".join(str(x) for x in result )

n=int(input())
for i in range(n):
    expression=list(input())
    print(trans(expression))

```



查看题解的情况下单独写耗时50min

### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：用模拟的办法来实现：遍历输出结果，如果被读取到的字母在原来字符串中的顺序比上一次压入的大，就把上一次压入的字符和本次被读取字符之间的部分一次性压入栈中；如果被读取到的字母恰好在栈顶，则执行弹出操作；如果不满足以上两个条件直接输出NO。第一次做是在每日选做里面熟悉栈的写法，这是第二次，相对地熟了一些。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 16 17:35:20 2024

@author: liumi
"""

s=list(input())
while True:
    try:
        n=list(input())
        a=0
        if len(s)!=len(n):
            print("NO")
        else:
            stack=[]
            last_push=-1
            for i in n:
                if not i in s:
                    a=1
                    print("NO")
                    break
                else:
                    push=s.index(i)
                    if push>last_push:
                        stack+=s[last_push+1:push+1]
                        last_push=push
                        stack.pop()
                    elif i==stack[-1]:
                        stack.pop()
                    else:
                        a=1
                        print("NO")
                        break
            if a==0:
                print("YES")
    except EOFError:
        break
    

```



耗时约30min

### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：用字典来实现树，每一次退出当前递归层时往结果上加一来统计层数



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sat Mar 16 23:41:34 2024

@author: liumi
"""

n=int(input())
tree={}
for i in range(1,n+1):
    s=list(map(int,input().split()))
    tree[i]=s

def get_in(i):
    
    global tree
    
    left=tree[i][0]
    right=tree[i][1]
    if left==-1 and right==-1:
        return 0
    elif left==-1 and right!=-1:
        return 1+get_in(right)
    elif left!=-1 and right==-1:
        return 1+get_in(left)
    else:
        return 1+max(get_in(left),get_in(right))

print(1+get_in(1))

```



耗时约22min

### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：一看就很难不觉得是冒泡排序，于是用了一下计概时期模拟冒泡的办法，发现根本冒泡时间复杂度太高根本行不通，只好查看题解，从题解中学到了归并排序的写法：需要有两个函数（分组和归并），分组的时候要递归调用直至分至只有单个元素，然后再调用归并函数把两个组合在一起，并且计算交换次数，将结果返回上一层。



代码

```python
# # -*- coding: utf-8 -*-
"""
Created on Sun Mar 17 23:13:10 2024

@author: liumi
"""

def group(lst):
    
    global merge
    if len(lst)<=1:
        return 0,lst
    
    mid=len(lst)//2
    l,left=group(lst[:mid])
    r,right=group(lst[mid:])
    
    receive_count,merged_lst=merge(left,right)
    
    return l+r+receive_count,merged_lst
    
def merge(left,right):
    
    i,j,count,merged=0,0,0,[]
    
    while i<len(left) and j<len(right):
        if left[i]<=right[j]:
            merged.append(left[i])
            i+=1
        else:
            merged.append(right[j])
            j+=1
            count+=len(left)-i
    merged+=left[i:]
    merged+=right[j:]
    return count,merged

while True:
    n=int(input())
    if n==0:
        break
    
    l=[]
    for i in range(n):
        l.append(int(input()))
    cnt,a=group(l)
    print(cnt)

```



在查看了题解的情况下单独写出花费了近50min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	作业题目可不简单了，这周的作业只有四道有思路会写，”超级快速排序“和”中序转后序“都需要学习题解才能知道做法，说明要学的还有很多。比较有意思的点是个人觉得字典很适合用来表示树（？），目前尚未清楚此种想法的合理性；练习了类的写法，还是比较卡手，写的时候需要注意很多地方。每日选做有在慢慢跟上，每次打开题库就有一种打游戏里升级副本的感觉（？



