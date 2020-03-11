[TOC]



# 选择排序

​		选择排序是最基础的排序方式，每次遍历一遍数组，找到符合的一个元素，将其加入新列表中，然后重复这个操作，直到将所有的元素排序到新列表中。时间复杂度为O(n^2)

## 代码



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



# Merge Sort

# Bubble Sort



