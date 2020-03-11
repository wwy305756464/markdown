# Breath-First Search

广度优先搜索可以解决两类问题：

1. 从节点A出发，有前往节点B的路径么？
   * 先查找一度关系，再查找二度关系，即一度关系在二度关系之前加入查找名单
2. 从节点A出发，前往节点B的哪条路径最短？
   * 当在记录一度关系，二度关系，…… 的列表中查找的时候，BFS不仅找到了从A到B的路径，而且找到的是最短路径

## 算法

假如说我们要在一个图（树）中找到芒果商，我们可以用以下算法：

1. 创建一个队列用于储存要检查的人
2. 从队列中弹出一个人
3. 检查这个人是否是芒果商
4. 如果这个人是芒果商，则结束程序
5. 如果不是，则将这个人的所有邻居加入队列中，从第二步开始重复
6. 直到队列为空就结束，可以把所有芒果商找出来

``` python
from collection import deque // 双端队列
search_queue = deque()  // 1. 创建队列
search_queue += graph["you"]  // graph["you"]是一个数组，包含了所有的邻居

while search_queue:  // 6. 只要队列不为空就一直循环
    person = search_queue.popleft()  // 2. 从队列中弹出一个人
    if person_is_seller(person):  // 3. 检查这个人是不是芒果商
        print (person + " is a mango seller")  // 4. 如果是，则结束
        return True
    else:
        search_queue += graph[person]  // 5. 如果不是，则加入这个人的邻居
return False

def  person_is_seller(name):
    return name[-1] == "m"
```

这样的算法存在问题，如果A既是B的朋友也是C的朋友，BFS会将其加入队列两次，这样会导致做无用功，严重时甚至会导致死循环，比如一个图中出现了环，就会出现死循环。那么我们可以在检查完一个人之后，将其标记为已检查，之后就不用再检查他。

``` python
from collection import deque // 双端队列
search_queue = deque()  // 1. 创建队列
search_queue += graph["you"]  // graph["you"]是一个数组，包含了所有的邻居
searched = []  // 创建一个空数组，用于记录已经检查过的人

while search_queue:  // 6. 只要队列不为空就一直循环
    person = search_queue.popleft()  // 2. 从队列中弹出一个人
    if not person in searched:  // 另外多加if条件，仅当这个人没有被标记过才检查
        if person_is_seller(person):  // 3. 检查这个人是不是芒果商
            print (person + " is a mango seller")  // 4. 如果是，则结束
            return True
        else:
            search_queue += graph[person]  // 5. 如果不是，则加入这个人的邻居
            searched.append(person) // 将检查过的加入到记录检查过的人的数组中
return False

def  person_is_seller(name):
    return name[-1] == "m"
```

## 运行时间

如果在图中进行BFS，我们需要沿每条边前行，因此运行时间至少为：O(E)

同时我们用一个队列，将每个人加入队列都是O(1)，我们需要将每个人都加入队列，所以时间是O(V)

总体运行时间是：O(E+V)

