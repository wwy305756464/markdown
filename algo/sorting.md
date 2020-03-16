[TOC]

![img](https://pic3.zhimg.com/80/v2-b92f60067804733195466a192e0b3603_1440w.jpg)



[https://github.com/CyC2018/CS-Notes/blob/master/notes/%E7%AE%97%E6%B3%95%20-%20%E6%8E%92%E5%BA%8F.md](https://github.com/CyC2018/CS-Notes/blob/master/notes/算法 - 排序.md)

https://zhuanlan.zhihu.com/p/57088609

# Select Sort 选择排序

​		选择排序是最基础的排序方式，每次遍历一遍数组，找到符合的一个元素（从最小的到最大的），将其加入新列表中，然后重复这个操作，直到将所有的元素排序到新列表中。时间复杂度为O(n^2)

![img](https://camo.githubusercontent.com/7d5779d6bf5f57e00e5e48e49437a74a4d7e3cf7/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f62633662653264302d656435652d346465662d383965352d3361646139616661383131612e676966)

## 代码

```c++
void Sorting::selectionSort(int array[], int n) {
    int i, j, min;
    for (i = 0; i < n - 1; ++i) {
        min = i;
        for (j = i + 1; j < n - 1; ++j) {
            if (array[min] < array[j]) {
                min = j;
            }
        }
        swap (&array[min], &array[i]);
    }
}
```



# Bubble Sort 冒泡排序

​		把第一个元素和第二个元素比较，如果第一个比第二个大，则交换他们的位置。接着比较第二个和第三个元素，如果第二个比第三个大，则交换他们的位置，以此类推。我们对每一对相邻的元素做相同的工作，这样遍历一遍下来之后，最右边的元素就是最大的。这样我们完成了第一层。接着我们回到最左边的元素，做相同的工作，但是只用做到倒数第二个元素，因为最右已经是最大的了。如此重复下去，直到排序完成。

<img src="https://pic1.zhimg.com/v2-1543c0b97237bb55063e033959706ca0_b.webp" alt="img" style="zoom:80%;" />

## 代码

``` c++
void Sorting::bubbleSort (int array[], int n) {
    int i, j;
    for (i = 0; i < n; ++i) {					// 这里i不参与运算，只是遍历n遍
        for (j = 0; j < n - i - 1; ++j) {		 // 每一层不用到最后，而是逐层递减
            if (array[j+1] < array[j]) {
                swap(&array[j+1], &array[j]);
            }
        }
    }
}
```



## 优化

​		对于冒泡算法，我们发现如果在某一层遍历时，右边的都比左边的元素大，那么就已经排序完成，没有必要再进行下一层排序。因此我们可以对此进行优化：

``` c++
void Sorting::bubbleSort (int array[], int n) {
    int i, j;
    for (i = 0; i < n; ++i) {					// 这里i不参与运算，只是遍历n遍
        bool flag = true;
        for (j = 0; j < n - i - 1; ++j) {		 // 每一层不用到最后，而是逐层递减
            if (array[j+1] < array[j]) {
                flag = false;
                swap(&array[j+1], &array[j]);
            }
        }
        if (flag == true) {
            break;							   // 在每一层遍历的时候设置flag，如果发生交换，则变													为false。如果在某一层没有改变，证明已经排序完												 成，可以直接break掉。
        }
    }
}
```



# Insertion Sort 插入排序

从数组的第二个位置开始抽取元素，把它与左边第一个元素进行比较，如果左边第一个元素比它大，则继续与左边第二个元素比较，直到遇到比它小的元素，并把其插到这个元素的右边。按照相同的步骤继续选择第三，第四个元素，选择适当的位置插入。

<img src="https://pic2.zhimg.com/v2-f87ad7d8ad54379dd81f02fcf9b91f49_b.webp" alt="img" style="zoom:80%;" />

## 代码

``` c++

```



# Shell Sort 希尔排序



# Merge Sort 归并排序



# Quick Sort 快速排序

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



# Heap Sort 堆排序

# Counting Sort 计数排序



# Bucket Sort 桶排序

# Radio Sort 基数排序









