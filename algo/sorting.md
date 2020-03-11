[TOC]

![image-20200311105957101](C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200311105957101.png)



[https://github.com/CyC2018/CS-Notes/blob/master/notes/%E7%AE%97%E6%B3%95%20-%20%E6%8E%92%E5%BA%8F.md](https://github.com/CyC2018/CS-Notes/blob/master/notes/算法 - 排序.md)



# Select Sort

​		选择排序是最基础的排序方式，每次遍历一遍数组，找到符合的一个元素，将其加入新列表中，然后重复这个操作，直到将所有的元素排序到新列表中。时间复杂度为O(n^2)

![img](https://camo.githubusercontent.com/7d5779d6bf5f57e00e5e48e49437a74a4d7e3cf7/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f62633662653264302d656435652d346465662d383965352d3361646139616661383131612e676966)

## 代码



# Bubble Sort



# Insertion Sort



# Shell Sort



# Merge Sort



# Quick Sort

## 步骤

1. 选择元素作为基准值（pivot）
2. 找出比基准值小的元素和比基准值大的元素，分区（partitioning）
3. 现在得到三个group:
   * 一个由所有小于基准值的数字组成的子数组
   * 基准值
   * 一个由所有大于基准值的数字组成的子数组
4. 因为现在小于的和大于的子数组都是无序的，我们可以继续在这两个子数组上进行前三步操作，即对这两个子数组进行快速排序

## 代码

```python
def quicksort(array):
	if len(array) < 2:
        return array
    else:
        pivot = array(0)
        left = [i for i in array[1:] if i <= pivot]
        right = [i for i in array[1:] if i > pivot]
        return quicksort(less) + [pivot] = quicksort(right)
print quicksort ([10, 5, 2, 6, 3, 9])
```

快速排序在最糟糕的情况下运行时间为 O(n^2)，平均情况为O(nlogn)



# Heap Sort







