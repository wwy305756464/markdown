# ROS 基础

## 编写节点

### 创建 package

```c++
catkin_create_pkg package_name depend1 depend2 depend3
```

e.g.

```c++
catkin_create_pkg my_minimal_nodes roscpp std_msgs
```

`my_minimal_nodes` 是创建的package的名字，`roscpp`和`std_msgs`是这个package要depend on 的已经存在的程序包。一般`roscpp`说明了这个package用cpp，`std_msgs`是ROS中预定义的数据类型格式

### package.xml文件

```xml
...
<name> my_minimal_nodes </name>
<description> The my_minimal_nodes package </description> 
...
<buildtool_depend> catkin </buildtool_depend>
<build_depend> roscpp </build_depend>
<build_depend> std_msgs </build_depend>
<run_depend> roscpp </run_depend>
<run_depend> std_msgs </run_depend>
```

`my_minimal_nodes`必须与创建的package名词保持一致

如果之后要新加入其他的dependence，就需要将它加入build_depend和run_depend



## 编写 Publisher



## 编写 Subscriber



## 编译、运行节点



## 节点时间规划



## 其他ROS工具



## 一个节点同时充当 Publisher 和 Subscriber





