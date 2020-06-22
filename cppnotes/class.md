# 类与对象

## 结构体

### 定义

#### 定义结构体的同时定义结构体变量

```c++
struct color 
{
    int red;
    int green;
    int blue;
} c1
```

这里c1是定义的结构体变量

#### 先定义结构体类型，再使用该类型定义结构体变量

```c++
typedef struct
{
    int red;
    int green;
    int blue;
} COLOR;
COLOR c1;
```

#### 定义一个无名的结构体类型的同时定义结构体变量

```c++
struct
{
	int red;
    int green;
    int blue;
} c1;
```

