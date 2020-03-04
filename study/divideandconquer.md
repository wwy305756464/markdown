# Divide and Conquer

## 工作原理

1. 找出简单的基线条件 （base case）
2. 确定如何缩小问题的规模，使其符合基线条件



## 求数组的sum

```c++
int sum(int Arr[], int n) {
	if (n <= 0) {
		return 0;
	}
	else {
		return sum(Arr, n-1) + Arr[n-1];
	}
}

int main() {
    int Arr[] = { 1, 2, 3, 4, 5};
    int n = sizeof(A) / sizeof(A[0]);
    printf( "%dn", sum(A, n));
    return 0; 
}
```

第一步：找出base case
$$
\begin{equation}sum=
\left\{
             \begin{array}{lr}
             0, & Array = \phi \\
             Array[0], & Array[0] \neq 0, Array[others]=0\\
             \end{array}
\right.
\end{equation}
$$
第二步：缩小问题的规模，使其向base case靠拢
$$
sum = sum(2,4,6) = 2+sum(4,6) =2+(4+sum(6))
$$
<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302143830319.png" alt="image-20200302143830319" style="zoom:67%;" />

