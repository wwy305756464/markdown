[TOC]



# 数组 vs. 链表

## 数组

### 储存机制

​		数组在内存中占用连续的一串内存空间，因此当新的元素加入到数组中时，如果导致内存位置不够，需要将整个数组都移动到其他的内存处。可以通过预留内存来解决这个问题.

​		数组支持顺序访问和随机访问

### 读取元素 

​		数组中元素都是按顺序排列，只要知道index就可以随时读取元素

### 插入元素

​		插入元素需要将后面的所有元素向后移，如果没有足够的空间可能还需要将整个数组复制到其他内存

### 删除

​		删除元素后，需要将之后的元素都向前移，但这里不会涉及到内存不够的情况



## 链表

### 储存机制

​		链表中的元素可以储存在内存中的任意位置，也就是说不用占据连续的一段内存。链表中的每个元素，除了储存元素本身外，也要将下一个元素的地址存下。所以在链表中添加元素很容易，只要将元素加入内存中任意位置，然后将此内存地址储存在上一个元素中

​		链表支支持顺序访问

### 读取元素

​		链表中若是想读取最后一个元素，需要从第一个元素开始找下一个元素的地址，直到找到最后一个元素。因此，如果要读取所有元素，链表的效率很高，但如果只是跳着读取元素，链表效率很低

### 插入元素

​		链表中插入元素只需要将前一个元素的地址改为新插入元素的地址，然后新插入元素所指向的地址为上一个元素原来所储存的地址。在插入元素时，链表是更好的选择

### 删除

​		链表中删除元素需要将上一个元素的地址改为删除元素所指向的地址即可



## 操作时间

| 操作 | 数组 | 链表 |
| :--: | :--: | :--: |
| 读取 | O(1) | O(n) |
| 插入 | O(n) | O(1) |
| 删除 | O(n) | O(1) |



# 栈

​		使用栈的时候，新加入的元素被放在栈顶，读取元素时也从栈顶元素开始，并同时将其删除。栈只有两种操作，压入（插入）和弹出（删除并读取）

## 调用栈

​		用于储存多个函数的变量，即为调用栈

```python
def greet(name):
	print("hello, " + name + "!")
	greet2(name)
	print("getting ready to say bye...")
	bye()
```

```python
def greet2(name):
	print("how are you, " + name + "?")
def bye():
	print("ok bye!")
```

1. 调用 greet("Maggie")，计算机先为其分配一部分内存，并print出 -> “hello, Maggie!"

2. 调用greet2("Maggie")，计算机为其分配另一部分内存，并且第二个内存块位于第一个上面，打印出”how are you, Maggie?" 之后从函数调用返回，栈顶的内存块被弹出。

3. 在弹出greet2的内存块后，栈顶内存块为greet，意味着我们返回了greet函数。这里要注意我们在调用greet2时，函数greet只执行了一部分。***调用另一个函数时，当前函数暂停并处于未完成状态，该函数所有变量的值都还存在内存中。*** 所以我们从greet2回来后，从离开的地方开始向下执行，打印出“getting ready to say bye...”

4. 调用bye()，计算机再为其分配一块内存放在栈顶，打印“ok bye!”

5. 再次弹出bye()的内存块，返回函数greet()，之后没有别的事情要做，就从函数greet()返回。

   

## 递归调用栈

​		用于计算factorial的递归函数:

```c++
int factorial(int x) {
	if (x == 1) {
		return 1;
	}
	else {
		return x * factorial(x-1);
	}
}
```

现在分析factorial(3)的时候的栈调用情况：

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302124629795.png" alt="image-20200302124629795" style="zoom: 50%;" />

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302124659281.png" alt="image-20200302124659281" style="zoom:50%;" />

使用栈虽然很方便，但储存的信息可能占用大量的内存，可能会导致溢出



# Graph

## 图的实现

图由节点（node）和边（edge）组成。

因为图中，每个节点都与临近节点相连，我们可以用hashmap来表示。Hash中key映射到value，图中是node映射到所有相邻node。

``` pytho
graph = []
graph["you"] = ["Alice", "Ben", "Claire"]
```

这里键值对的添加顺序不重要，因为hash是无序的

## 有向图 vs. 无向图

有向图（directed graph）有单向关系的箭头，如果A指向B，那么B是A的邻居，而A不是B的邻居

无向图（undirected graph）没有箭头，直接相连的节点互为邻居

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200310021132942.png" alt="image-20200310021132942" style="zoom:50%;" />

以上两图的关系是等价的

## 加权图 vs. 非加权图

每条边有weight的图是weighted graph，反之为unweighted graph

计算非加权图的最短路径可以用BFS，计算加权图的最短路径可以用Dijkstra‘s算法。考虑到环的出现只会增加路径的长度，所以Dijkstra's可用于有向无环图（directed acyclic graph）

# Queue

先进先出的数据结构，只支持两种操作：入队和出队。



# Heap

