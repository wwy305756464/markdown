# C++中的Class

比如我们创建一个练习sorting的函数，将数组引入、method选择和output的打印放在main函数里面。同时创建一个header并在header里面定义一个class，里面有各个排序算法作为成员函数。另外还有一个cpp文件，写的是各种排序方式的算法。



Sorting.cpp 包含了main函数

```c++
#include <iostream>
#include "sorting.h"
using namespace std;

void printArray (int array[], int size) {
    for (int i = 0; i<size; ++i) {
        cout << array[i] << " ";
    }
    cout << endl;
    
}

int main() {
	Sorting Sortingarray;
    int array[] = { 23, 62, 34, 88, 16, 11, 22, 65 };
    int n = sizeof(array) / sizeof(array[0]);
    Sortingarray.selectionSort(array, n);
    cout << "Sorted Array: \n";
    printArray(array, n);
    return 0;

}
```



sorting.h 头文件，包含Sorting这个class

``` c++
#include <iostream>
#include <cmath>

class Sorting {
    public: 
    	void selectionSort(int array[], int n);
};
```



Algorithm_sorting.cpp 辅助程序，包含各种sorting的算法和side function

```c++
#include <iostream>
#include "sorting.h"
using namespace std;

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void Sorting::selectionSort(int array[], int n) {
    int i, j, min;
    for (i = 0; i < n - 1; ++i) {
        min = i;
        for (j = i + 1; j < n; ++j) {
            if (array[j] < array[min]) {
                min = j;
            }
        }
        swap(&array[min], &array[i]);
    }
}
```

