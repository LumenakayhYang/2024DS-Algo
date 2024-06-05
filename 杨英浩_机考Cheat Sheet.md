# 机考Cheat Sheet

## 关于常用的标准库

### 1.heapq

​	heapq是有关堆的模块，实际运用的时候通常只使用heappop和heappush。

```python
from heapq import heappop,heappush
a=[5,3,8,4,9,10]
heappush(a, 1)  #heappush(heap,item)->(要压入的堆，元素)
# >>>a=[1,3,4,5,8,9,10]
b=heappop(a)  #heappop(heap)->(要弹出元素的堆)
# >>>a=[3,4,5,8,9,10]
# >>>b=1
```

​	大根堆可以通过将各元素取负来实现，但是需注意在压入和弹出的时候进行正负号转换。

### 2.deque

​	deque是collection里面的一个类，是一种双端队列。实际使用的时候常使用append, appendleft, pop, popleft这几个方法。

```python
from collections import deque
a=[1,2,3,4,5]
dq_a=deque(a)
dq_a.append(6)
#>>>deque([1, 2, 3, 4, 5, 6])
dq_a.popleft()
#>>>deque([2, 3, 4, 5, 6])
dq_a.pop()
#>>>deque([2, 3, 4, 5])
dq_a.appendleft(1)
#>>>deque([1, 2, 3, 4, 5])
```

### 3.bisect

​	对于一个已经有序的列表，bisect可以用于查找出指定值在这个列表中的索引。常用的是bisect，bisect_left, bisect_right 三个方法。

​	**Case1 查找列表中不存在的元素** 三个方法的返回值都是适当插入点的索引。

```python
import bisect
a=[1,3,5,7,10]
index1=bisect.bisect(a,4)
index2=bisect.bisect_left(a, 4)
index3=bisect.bisect_right(a, 4)
#>>>index1=2, index2=2, index3=2
```

​	**Case2 要查找的元素在列表中仅有一个** bisect_left返回该值的索引，bisect和bisect_right一样，都返回该值的索引+1.

```python
import bisect
a=[1,3,5,7,10]
index1=bisect.bisect(a, 5)
index2=bisect.bisect_left(a, 5)
index3=bisect.bisect_right(a, 5)
#>>>index1=3, index2=2,index3=3
```

​	**Case3 要查找的元素在列表中有多个** bisect_left返回该值左边界的索引，bisect和bisect_right一样，都返回该值右边界的索引+1.

```python
import bisect
a=[1,5,5,5,10]
index1=bisect.bisect(a, 5)
index2=bisect.bisect_left(a, 5)
index3=bisect.bisect_right(a, 5)
#>>>index1=4, index2=1, index3=4
```

## 关于基本数据结构

### 1.栈（stack）

​	右进右出

#### 02694：波兰表达式

​	可以先把波兰表达式倒转进行处理

```python
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

#### 24591：中序表达式转后序表达式 Aka 调度场算法 (Shunting Yard Algorithm)

​	非常关键的点在于在允许符号入栈的时候，一定要保证栈里不存在优先级高于自己或者与自己相同的符号。若有，这些符号需依次弹出至结果当中。

```python
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

#### 单调栈(monotone stack)

​	单调栈总具有这样一种性质，其元素从栈顶到栈底是单调的，亦即：越靠近栈顶元素越大（越小）。

```python
#28203【模板】单调栈
def main():
    n=int(input())
    lst=list(map(int,input().split()))
    result=[0 for i in range(n)]
    stack=[]
    for i in range(n):
        if stack and stack[-1][0]<lst[i]:    #关键步骤
            while stack and stack[-1][0]<lst[i]:
                result[stack[-1][1]]=i+1
                stack.pop()
        stack.append((lst[i],i))
    print(*result)

main()
```

### 2.队列(queue)

​	右进左出。能用到的队列结构基本上能够被collections中的deque实现

## 关于树

### 1.表示方法

​	可以使用邻接表来表示，也可以使用链表来表示。

### 2.建树问题

#### ①已知任两个遍历序列的建树方法

​	本质上都是先确定根，然后确定左树和右树。然后递归地去建树。

##### 24750：根据二叉树中后序列建树

```python
class Node:
    def __init__(self,name):
        self.name=name
        self.left=None
        self.right=None
def build_tree(inorder, postorder):
    if inorder:
        root = Node(postorder.pop())
        root_index = inorder.index(root.data)
        root.right = build_tree(inorder[root_index+1:], postorder)
        root.left = build_tree(inorder[:root_index], postorder)
        return root
def printout(tree):
    if not tree:
        return ""
    return tree.name+printout(tree.left)+printout(tree.right)
def main():
    mid=list(input())
    last=list(input())
    tree=build_a_tree(mid, last)
    print(printout(tree))
main()
```

#### ②对于叶节点已知的扩展二叉树建树

##### 08581：扩展二叉树

```python
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
lst=list(input())
tree=build_a_tree(lst)
```

### 3.遍历问题

​	关于树的先序中序后序遍历已经叙述详尽，主要说说层次遍历

```python
def level_order_traversal(root):
    queue = [root]
    traversal = []
    while queue:
        node = queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal
```

### 4.Haffman编码树

​	要构建一个最优的哈夫曼编码树，首先需要对给定的字符及其权值进行排序。然后，通过重复合并权值最小的两个节点（或子树），直到所有节点都合并为一棵树为止。

```python
import heapq
class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    def __lt__(self, other):
        return self.freq < other.freq
def huffman_encoding(char_freq):
    heap = [Node(char, freq) for char, freq in char_freq.items()]
    heapq.heapify(heap)
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(None, left.freq + right.freq) # note: 合并之后 char 字典是空
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
    return heap[0]
def external_path_length(node, depth=0):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return depth * node.freq
    return (external_path_length(node.left, depth + 1) +
            external_path_length(node.right, depth + 1))
def main():
    char_freq = {'a': 3, 'b': 4, 'c': 5, 'd': 6, 'e': 8, 'f': 9, 'g': 11, 'h': 12}
    huffman_tree = huffman_encoding(char_freq)
    external_length = external_path_length(huffman_tree)
    print("The weighted external path length of the Huffman tree is:", external_length)
if __name__ == "__main__":
    main()
# Output:
# The weighted external path length of the Huffman tree is: 169 
```

### 5.二叉搜索树(BST)

​	二叉搜索树的性质：比根节点小的节点都在左边，比根节点大的节点都在右边。前序遍历就是所有节点从小到大的排序

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def insert(root, val):
    if root is None:
        return TreeNode(val)
    if val < root.val:
        root.left = insert(root.left, val)
    else:
        root.right = insert(root.right, val)
    return root
```

### 6.AVL树 aka 平衡二叉树

​	一种自平衡二叉搜索树。对于任意一个节点而言，左子树和右子树的高度差总不会超过一。这里没有涉及删除操作，因为我赌它不考

```python
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

### 7.并查集(Disjoint Set)

​	并查集实现主要依赖于两个如下两个函数。实际使用中可能碰到多元的并查集。

```python
def get_father(x,lst): #寻找父节点并压缩路径
    if lst[x]!=x:
        lst[x]=get_father(lst[x], lst) #路径压缩：每次退出函数都要更新父节点
    return lst[x]
def join_together(x,y,lst): #合并集合
    fx=get_father(x, lst)
    fy=get_father(y, lst)
    if fx==fy:
        return
    lst[fx]=fy
```

### 8.前缀树(Trie)

1. **插入（Insert）**：

   ```python
   class TrieNode:
       def __init__(self):
           self.children = {}
           self.is_end_of_word = False
   
   class Trie:
       def __init__(self):
           self.root = TrieNode()
   
       def insert(self, word):
           node = self.root
           for char in word:
               if char not in node.children:
                   node.children[char] = TrieNode()
               node = node.children[char]
           node.is_end_of_word = True
   ```

2. **查找（Search）**：

   ```python
   def search(self, word):
       node = self.root
       for char in word:
           if char not in node.children:
               return False
           node = node.children[char]
       return node.is_end_of_word
   ```

3. **前缀查询（StartsWith）**：

   ```python
   def starts_with(self, prefix):
       node = self.root
       for char in prefix:
           if char not in node.children:
               return False
           node = node.children[char]
       return True
   ```



## 关于图

### 1.表示方法

​	主要有两种：邻接表和邻接矩阵。第一种适用于稀疏图。但是也可以写类对象，只是那样会复杂很多。

### 2.遍历问题

#### ①DFS：图的深度优先搜索

​	dfs的实现具体说来有两种：a.使用栈；b.对函数进行递归调用。常见的是第二种。

```python
def DFS(self, v): #用栈实现
        visited = set()
        stack = [v]

        while stack:
            current = stack.pop()
            if current not in visited:
                print(current, end=' ')
                visited.add(current)
                stack.extend(reversed(self.graph[current]))
               
def knight_tour(n, path, u, limit): #骑士周游节选
    u.color = "gray"
    path.append(u)              #当前顶点涂色并加入路径
    if n < limit:
        neighbors = ordered_by_avail(u) #对所有的合法移动依次深入
        #neighbors = sorted(list(u.get_neighbors()))
        i = 0

        for nbr in neighbors:
            if nbr.color == "white" and \               
                knight_tour(n + 1, path, nbr, limit):   #选择“白色”未经深入的点，层次加一，递归深入
                return True
        else:                       #所有的“下一步”都试了走不通
            path.pop()              #回溯，从路径中删除当前顶点
            u.color = "white"       #当前顶点改回白色
            return False
    else:
        return True
def has_cycle(graph, n): #利用dfs判断是否成环
    def dfs(node, visited, parent):
        visited[node] = True
        for neighbor in graph[node]:
            if not visited[neighbor]:
                if dfs(neighbor, visited, node):
                    return True
            elif parent != neighbor:
                return True
        return False
    visited = [False] * n
    for node in range(n):
        if not visited[node]:
            if dfs(node, visited, -1):
                return True
    return False
```

#### ②BFS：图的广度优先搜索

​	bfs一般可以使用队列来实现。

```python
def bfs(self, startNode):
        queue = deque()
        visited = set()
        visited.add(startNode)
        queue.append(startNode)
        while queue:
            currentNode = queue.popleft()
            print(currentNode, end=" ")
            for neighbor in self.adjList[currentNode]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
```

### 3.经典算法

#### ①有向图拓扑排序

​	主要的算法有两种，分别用dfs和bfs实现，后者又称为Kahn算法

```python
from collections import deque, defaultdict
def topological_sort(graph):
    indegree = defaultdict(int)
    result = []
    queue = deque()
    # 计算每个顶点的入度
    for u in graph:
        for v in graph[u]:
            indegree[v] += 1
    # 将入度为 0 的顶点加入队列
    for u in graph:
        if indegree[u] == 0:
            queue.append(u)
    # 执行拓扑排序
    while queue:
        u = queue.popleft()
        result.append(u)

        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                queue.append(v)
    # 检查是否存在环
    if len(result) == len(graph):
        return result
    else:
        return None
```

#### ②检查无向图连通和成环

```python
def is_connected(graph, n):
    visited = [False] * n  # 记录节点是否被访问过
    stack = [0]  # 使用栈来进行DFS
    visited[0] = True
    while stack:
        node = stack.pop()
        for neighbor in graph[node]:
            if not visited[neighbor]:
                stack.append(neighbor)
                visited[neighbor] = True

    return all(visited)
def has_cycle(graph, n):
    def dfs(node, visited, parent):
        visited[node] = True
        for neighbor in graph[node]:
            if not visited[neighbor]:
                if dfs(neighbor, visited, node):
                    return True
            elif parent != neighbor:
                return True
        return False
    visited = [False] * n
    for node in range(n):
        if not visited[node]:
            if dfs(node, visited, -1):
                return True
    return False
```

#### ③有向图寻找强连通分量：kosaraju算法

```python
def dfs1(graph, node, visited, stack):
    visited[node] = True
    for neighbor in graph[node]:
        if not visited[neighbor]:
            dfs1(graph, neighbor, visited, stack)
    stack.append(node)
def dfs2(graph, node, visited, component):
    visited[node] = True
    component.append(node)
    for neighbor in graph[node]:
        if not visited[neighbor]:
            dfs2(graph, neighbor, visited, component)
def kosaraju(graph):
    # Step 1: Perform first DFS to get finishing times
    stack = []
    visited = [False] * len(graph)
    for node in range(len(graph)):
        if not visited[node]:
            dfs1(graph, node, visited, stack)
    # Step 2: Transpose the graph
    transposed_graph = [[] for _ in range(len(graph))]
    for node in range(len(graph)):
        for neighbor in graph[node]:
            transposed_graph[neighbor].append(node)
    # Step 3: Perform second DFS on the transposed graph to find SCCs
    visited = [False] * len(graph)
    sccs = []
    while stack:
        node = stack.pop()
        if not visited[node]:
            scc = []
            dfs2(transposed_graph, node, visited, scc)
            sccs.append(scc)
    return sccs
```

#### ④单源最短路径：Dijkstra算法

​	核心思想：每一次都从未被访问的节点中寻找到**起点**路程最短的节点进行BFS。

```python
def dijkstra(graph, start): #奇怪的类写法，注意变通
    pq = []
    start.distance = 0
    heapq.heappush(pq, (0, start))
    visited = set()
    while pq:
        currentDist, currentVert = heapq.heappop(pq)    
        if currentVert in visited:
            continue
        visited.add(currentVert)
        for nextVert in currentVert.getConnections():
            newDist = currentDist + currentVert.getWeight(nextVert)
            if newDist < nextVert.distance:
                nextVert.distance = newDist
                nextVert.pred = currentVert
                heapq.heappush(pq, (newDist, nextVert))
import heapq
def dijkstra(N, G, start): #使用邻接表
    INF = float('inf')
    dist = [INF] * (N + 1)  # 存储源点到各个节点的最短距离
    dist[start] = 0  # 源点到自身的距离为0
    pq = [(0, start)]  # 使用优先队列，存储节点的最短距离
    while pq:
        d, node = heapq.heappop(pq)  # 弹出当前最短距离的节点
        if d > dist[node]:  # 如果该节点已经被更新过了，则跳过
            continue
        for neighbor, weight in G[node]:  # 遍历当前节点的所有邻居节点
            new_dist = dist[node] + weight  # 计算经当前节点到达邻居节点的距离
            if new_dist < dist[neighbor]:  # 如果新距离小于已知最短距离，则更新最短距离
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))  # 将邻居节点加入优先队列
    return dist

```

#### ⑤最小生成树：Prim算法

​	核心思想：每一次都从未被访问的节点中寻找到**当前已知节点**路程最短的节点进行遍历。

```python3
def prim(graph, start): #奇怪的类写法，注意变通
    pq = []
    start.distance = 0
    heapq.heappush(pq, (0, start))
    visited = set()

    while pq:
        currentDist, currentVert = heapq.heappop(pq)
        if currentVert in visited:
            continue
        visited.add(currentVert)

        for nextVert in currentVert.getConnections():
            weight = currentVert.getWeight(nextVert)
            if nextVert not in visited and weight < nextVert.distance:
                nextVert.distance = weight
                nextVert.pred = currentVert
                heapq.heappush(pq, (weight, nextVert))
def prim(graph, start_node):
    mst = set()
    visited = set([start_node])
    edges = [
        (cost, start_node, to)
        for to, cost in graph[start_node].items()
    ]
    heapify(edges)
    while edges:
        cost, frm, to = heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.add((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in visited:
                    heappush(edges, (cost2, to, to_next))
#05442兔子与星空：
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
```

