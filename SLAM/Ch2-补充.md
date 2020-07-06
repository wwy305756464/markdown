## 在Linux中创建编译运行c++文件
### 用g++编译
step1: 在文件夹中创建cpp文件 `hello.cpp`
step2: 在当前文件夹打开terminal，输入 `g++ hello.cpp`，这时会在文件夹中出现一个 `a.out`文件
step3: 在当前terminal中输入 `./a.out` 即编译运行完成

### 用cmake编译
step1: 在文件夹中创建cpp文件 `hello.cpp`
step2: 在文件夹中创建 `CMakeLists.txt`

```
# 声明要求的cmake的最低版本
cmake_minimum_required( VERSION 2.8 )

# 声明一个cmake工程
project( HelloSLAM )

# 添加一个可执行程序，语法：
# add_executable( 程序名 源代码文件 )
add_executable( helloSLAM helloSLAM.cpp )
```
step3: 新建文件夹 `build`，并在新文件夹中打开terminal
step4: 使用cmake对该工程进行编译，在终端输入 `cmake .`，这时生成makefile和一系列其他的
step5: 用make对其进行编译，在终端中输入 `make`
step6: 运行该文件，在当前终端中输入 `./helloSLAM` 即可

## 使用库（library)
##### Step1: 编写库
```
#include <iostream>
using namespace std;

void printHello(){
    cout << “Hello SLAM” << endl;
}
```
这里我们看到库中没有main函数，而是提供了一个printHello函数，调用这个函数将输出一条信息
##### Step2: 在CMakeLists.txt中添加如下内容
`add_library( hello libHelloSLAM.cpp )`
这句话告诉了cmake，我们想把这个文件编译成一个叫做hello的库
然后我们需要用cmake和make进行编译
这里我们发现build文件夹中生成了一个叫做 `libhello.a` 的文件，这就是我们的库
 
在Linux中，库有静态库和共享库两种，静态库以 .a 作为后缀名，共享库以 .so 结尾。所有库都是一些函数打包后的集合，差别在于静态库每次被调用都会生成一个副本，而共享库则只有一个副本，更省空间。如果我们想要生成共享库，那么在CMakeLists中我们应该添加：
`add_library( hello_shared SHARED libHelloSLAM.cpp )`
##### Step3: 添加头文件
在上面添加库后，我们需要提供一个头文件来说明这些库里面都有什么，从而方便自己和别人调用。只要有了头文件和库文件，我们就可以调用这个库

```
#ifndef LIBHELLOSLAM_H_ // 为了防止重复引用这个头文件而引起的重定义错误
#define LIBHELLOSLAM_H_
void printHello();
#endif
```
##### Step4: 写可执行程序来调用这个库和其中的函数 // useHello.cpp

```
#include “libHelloSLAM.h”

int main(int argc, char **argv){
    printHello();
    return 0;
}
```
##### Step5: 在CMakeLists中添加可执行程序的生成命令，链接到刚才的库上

```
add_executable( useHello useHello.cpp )
target_link_libraries( useHello hello ) // 这里useHello是add_executable中创建的，hello是上面编译库的时候命名的库的名字
```



