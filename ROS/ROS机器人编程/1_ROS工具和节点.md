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

``` c++
#include <ros/ros.h>
#include <std_msgs/Float64.h>
	// ros和消息的头文件

int main(int argc, char ** argv) {
    ros::init(argc, argv, "my_minimal_publisher");
    ros::NodeHandle n;
    	// 固定的两句，初始化一个名为"my_minimal_publisher"的节点，并为其创造一个句柄
    
    ros::Publisher my_publisher_object = n.advertise<std_msgs::Float64>("topic1", 1);
    	// 初始化一个publisher，话题名为"topic1"，队列长度为1，消息类型为std_msgs::Float64
    
    std_msgs::Float64 input_float;
    	// 定义一个消息对象，名为input_float，可以用
    	// rosmag show std_msgs/Float64 在terminal中输入，可以得到这个消息的类型
    input_float = 0.0; 
    
    while(ros::ok()){
        input_float.data = input_float.data + 0.01;
        my_publisher_object.publish(input_float);
    }
    	// while(ros::ok()) 说明只要roscore在运行，这个循环就一直进行
    	// publish 那一句说明通过my_publisher_object这个对象发送名为input_float的信息
}
```

### 规划节点时间

引入`naptime`来合理规划节点发布信息的时间

```c++
ros::Rate naptime(1.0);
...
naptime.sleep();
```

这样可以使节点发布消息的速率变为每一hz发布一条信息

## 编写 Subscriber

```c++
#include <ros/ros.h>
#include <std_msgs/Float64.h>
	// ros和消息的头文件

void myCallback(const std_msgs::Float64 message_holder){
    ROS_INFO("received value is: %f", message_holder.data);
}
	// 这里回调函数唯一的作用就是显示接收到的数据
	// ROS_INFO即相当于C中的printf()，也可以用ROS_INFO_STREAM(), 与cout的用法类似

int main(int argc, char ** argv) {
    ros::init(argc, argv, "my_minimal_subscriber");
    ros::NodeHandle n;
    	// 固定的两句，初始化一个名为"my_minimal_subscriber"的节点，并为其创造一个句柄
    
    ros::Subscriber my_subscriber_object = n.subscribe("topic1", 1, myCallback);
    	// 初始化一个subscriber，队列长度为1，在"topic1"上有消息进来的时候需要调用回调函数myCallback
    	// 这里队列长度为1，如果回调函数无法跟上发布数据的频率，数据就需要排队。一般subscriber要和		   publisher的队列长度相同
    
    ros::spin();
    	// 回调函数一般需要和spin()一起使用，在回调函数被唤醒的时候，主程序通过spin()为回调函数的响应			提供一些时间。
    
    return 0;
}
```



## 编译、运行节点

新建文件夹，将上面的cpp文件放进去，假设文件夹名为`my_minimal_node`

编写好节点后，需要用catkin_make进行编译，一般在ros_ws中进行。

```c++
catkin_make
```

在terminal中运行完这句之后，我们还需要修改`CMakeLists.txt`的文件：

##### find_package

```xml
find_package(catkin REQUIRED COMPONENTS
	roscpp
	std_msgs
)
```

##### 添加新节点并连接库

```
## Declare a cpp executable
add_executable(my_minimal_publisher src/minimal_publisher.cpp)

## Specify libraries to link a library or executable target against
target_link_libraries(my_minimal_publisher ${catkin_LIBRARIES} )
```

这里，add_executbale的第一个参数是为生成的可执行文件选定名字，一般名字不重复 ，第二个参数是对包目录而言在哪里找到源代码

之后再用`catkin_make`编译，会得到一个新的文件夹，里面有一个新的可执行文件叫做`my_minimal_publisher`

编译完成后，我们需要运行这个节点，先在一个terminal中输入`roscore`，然后重新打开一个terminal输入：

```
rosrun my_minimal_nodes my_minimal_publisher
```

这里遵循格式`rosrun package_name executable_name`

​	packages_name是当前文件夹的名字，executable_name是我们在CMakeLists中定义的可执行文件的名字



## rostopic工具

rostopic后面有很多command可以跟：

```cpp
rostopic bw topic_name // display bandwidth used by topic
rostopic echo topic_name // 输出topic中的内容
rostopic find topic_name // find topic by type
rostopic hz topic_name // 显示消息发布的频率
rostopic info topic_name // 显示当前topic的信息
rostopic list // 显示所有active的topic
rostopic pub topic_name // publish data to topic
rostopic type topic_name // print topic type
```



## 其他ROS工具

#### catkin_simple

catkin_simple可以简化CMakeLists.txt

#### launch文件自动启动多个节点

#### rqt_console来监控ROS消息

#### rosbag来记录并回放数据

## 一个节点同时充当 Publisher 和 Subscriber

### minimal_simulator

```c++
#include<ros/ros.h>
#include<std_msgs/Float64.h>

std_msgs::Float64 g_velocity;
std_msgs::Float64 g_force;

void myCallback(const std_msgs::Float64& message_holder) {
    ROS_INFO("received force value is: %f", message_holder.data);
    g_force.data = message_holder.data;
}

int main(int argc, char **argv) {
    ros::init(argc, argv, "minimal_simulator");
    ros::NodeHandle nh;
    ros::Subscriber my_subscriber_object = nh.subscribe("force_cmd", 1, myCallback);
    ros::Publisher my_publisher_object = nh.advertise<std_msgs::Float64>("velocity", 1);

    double mass = 1.0;
    double dt = 0.01;
    double sample_rate = 1.0/dt;
    g_velocity.data - 0.0;
    g_force.data = 0.0;

    ros::Rate naptime(sample_rate);

    while(ros::ok()) {
        g_velocity.data = g_velocity.data + (g_force.data/mass) * dt;
        my_publisher_object.publish(g_velocity);

        ROS_INFO("velocity = %f", g_velocity.data);
        ros::spinOnce();
        naptime.sleep();
    }

    return 0;
}
```

### minimal_controller

```cpp
#include<ros/ros.h>
#include<std_msgs/Float64.h>

std_msgs::Float64 g_velocity;
std_msgs::Float64 g_vel_cmd;
std_msgs::Float64 g_force;

void myCallbackVelocity(const std_msgs::Float64& message_holder) {
    ROS_INFO("received force value is: %f", message_holder.data);
    g_velocity.data = message_holder.data;
}

void myCallbackVelCmd(const std_msgs::Float64& message_holder) {
    ROS_INFO("received velocity command value is: %f", message_holder.data);
    g_vel_cmd.data = message_holder.data;
}

int main(int argc, char **argv) {
    ros::init(argc, argv, "minimal_controller");
    ros::NodeHandle nh;
    ros::Subscriber my_subscriber_object_1 = nh.subscribe("velocity", 1, myCallbackVelocity);
    ros::Subscriber my_subscriber_object_2 = nh.subscribe("vel_cmd", 1, myCallbackVelCmd);
    ros::Publisher my_publisher_object = nh.advertise<std_msgs::Float64>("force_cmd", 1);

    double Kv = 1.0;
    double dt_controller = 0.01;
    double sample_rate = 1.0/dt_controller;
    double vel_err = 0.0;
    g_velocity.data = 0.0;
    g_force.data = 0.0;
    g_vel_cmd.data = 0.0;

    ros::Rate naptime(sample_rate);

    while(ros::ok()) {
        vel_err = g_vel_cmd.data - g_velocity.data;
        g_force.data = Kv * vel_err;
        my_publisher_object.publish(g_force);

        ROS_INFO("force command = %f", g_force.data);
        ros::spinOnce();
        naptime.sleep();
    }
    
    return 0;
}
```







