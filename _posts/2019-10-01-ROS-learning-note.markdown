---
layout: post
title:  "ROS学习笔记---Beginner Level常用操作"
date:   2019-10-01 22:49:52 +0800
categories: ROS Linux
tags: C_or_CPP
---
@[TOC](ROS学习笔记（二）---Beginner Level)

# 初学ROS做的笔记，权当备忘录

旨在梳理beginner  level的常用操作，以便后续参考。

## 1.安装并配置ROS环境

### 1.1安装
- 设置source.list
  首先<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd>打开终端，输入以下指令：
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
- 设置keys
```bash
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
也可以使用另外的仓库==hkp://pgp.mit.edu:80 or hkp://keyserver.ubuntu.com:80==代替
- 安装
首先update源：

```bash
sudo apt-get update
```
然后安装全功能包：

```bash
sudo apt-get install ros-kinetic-desktop-full
```
- 设置环境变量

```bash
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
- 安装依赖

```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
   - 安装ros-dep

```bash
sudo apt install python-rosdep
```
 - 初始化ros-dep
 

```bash
sudo rosdep init
rosdep update
```

### 1.2 管理环境
确认ROS的环境变量是否设置好了，ROS_PACKAGE_PATH.

```bash
printenv | grep ROS
```
如果没有设置，需要source（根据实际安装的版本来）

```bash
source /opt/ros/kinetic/setup.bash
```

### 1.3 创建ROS空间
创建一个catkin workspace空间

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make               #编译指令
```

## 2.ROS文件系统介绍


## 3.创建并编译ROS程序包

 在当前目录创建一个package的命令如下，也可以同时指明这个package所依赖的其他package：
 

```bash
roscreate-pkg [package_name] [depend1] [depend2] [depend3]
```
#### 3.1 创建新的ROS package
切换到~/catkin_ws 并创建package

```bash
cd ~/ros_workspace
roscreate-pkg beginner_tutorials std_msgs rospy roscpp
```
创建package成功以后可以使用roscd切换到package目录

```bash
roscd beginner_tutorials 
```
如果不能使用roscd命令，则需要再次source

```bash
source ./devel/setup.sh
```

#### 3.2编译package
在catkin_ws目录下

```bash
catkin_make
```
build 目录是build space的默认所在位置，同时cmake 和 make也是在这里被调用来配置并编译你的程序包。devel 目录是devel space的默认所在位置, 同时也是在你安装程序包之前存放可执行文件和库文件的地方。
## 4.理解ROS节点和话题
安装一个轻量级的模拟器：

```bash
sudo apt-get install ros-kinetic-ros-tutorials
```
#### 4.1图的概念
- Nodes:节点,一个节点即为一个可执行文件，它可以通过ROS与其它节点进行通信。

- Messages:消息，消息是一种ROS数据类型，用于订阅或发布到一个话题。

- Topics:话题,节点可以发布消息到话题，也可以订阅话题以接收消息。

- Master:节点管理器，ROS名称服务 (比如帮助节点找到彼此)。

- rosout: ROS中相当于stdout/stderr。

- roscore: 主机+ rosout + 参数服务器 (参数服务器会在后面介绍)。
#### 4.2节点
一个节点其实只不过是ROS程序包中的一个可执行文件。ROS节点可以使用ROS客户库与其他节点通信。节点可以发布或接收一个话题。节点也可以提供或使用某种服务。

（节点是ros中非常重要的一个概念，为了帮助初学者理解这个概念，这里举一个通俗的例子：

例如，咱们有一个机器人，和一个遥控器，那么这个机器人和遥控器开始工作后，就是两个节点。遥控器起到了下达指 令的作用；机器人负责监听遥控器下达的指令，完成相应动作。从这里我们可以看出，节点是一个能执行特定工作任 务的工作单元，并且能够相互通信，从而实现一个机器人系统整体的功能。在这里我们把遥控器和机器人简单定义为两个节点，实际上在机器人中根据控制器、传感器、执行机构等不同组成模块，还可以将其进一步细分为更多的节点，这个是根据用户编写的程序来定义的。）
#### 4.3客户端库
ROS客户端库允许使用不同编程语言编写的节点之间互相通信:
- rospy = python 客户端库
- roscpp = c++ 客户端库
#### 4.4 roscore
roscore 是你在运行所有ROS程序前首先要运行的命令。

```bash
roscore
```
#### 4.5使用rosnode
rosnode 显示当前运行的ROS节点信息。rosnode list 指令列出活跃的节点:

```bash
rosnode list
```
rosnode info 命令返回的是关于一个特定节点的信息。

```bash
$ rosnode info /rosout
```

#### 4.6使用rosrun
rosrun 允许你使用包名直接运行一个包内的节点(而不需要知道这个包的路径)。用法：

```bash
rosrun [package_name] [node_name]
```
用一下命令运行一个turtlesim_node

```bash
rosrun turtlesim turtlesim_node
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062823135026.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1aml6aGlzaGFuZw==,size_16,color_FFFFFF,t_70#pic_center)
## 5.理解ROS服务和参数
#### 5.1 ROS Services
服务（services）是节点之间通讯的另一种方式。服务允许节点发送请求（request） 并获得一个响应（response）
rosservice可以很轻松的使用 ROS 客户端/服务器框架提供的服务。rosservice提供了很多可以在topic上使用的命令，如下所示：
使用方法:


```bash
rosservice list         #输出可用服务的信息
rosservice call         #调用带参数的服务
rosservice type         #输出服务类型
rosservice find         #依据类型寻找服务find services by service type
rosservice uri          #输出服务的ROSRPC uri
```

```bash
rosservice call [service] [args]
```

```bash
rosservice type [service]
```

#### 5.2 Using rosparam
rosparam使得我们能够存储并操作ROS 参数服务器（Parameter Server）上的数据。参数服务器能够存储整型、浮点、布尔、字符串、字典和列表等数据类型。rosparam使用YAML标记语言的语法。一般而言，YAML的表述很自然：1 是整型, 1.0 是浮点型, one是字符串, true是布尔, [1, 2, 3]是整型列表, {a: b, c: d}是字典. rosparam有很多指令可以用来操作参数，如下所示:
使用方法:

```bash
rosparam set            #设置参数
rosparam get            #获取参数
rosparam load           #从文件读取参数
rosparam dump           #向文件中写入参数
rosparam delete         #删除参数
rosparam list           #列出参数名
```

```bash
rosparam set [param_name]
rosparam get [param_name]
```

```bash
rosparam dump [file_name]
rosparam load [file_name] [namespace]
```

## 6. 使用rqt_console和roslaunch
安装：

```bash
sudo apt-get install ros-<distro>-rqt ros-<distro>-rqt-common-plugins ros-<distro>-turtlesim
```
#### 6.1使用rqt_console和rqt_logger_level
开两个终端分别运行：

```bash
rosrun rqt_console rqt_console
```

```bash
rosrun rqt_logger_level rqt_logger_level
```
再开一个新的终端运行：

```bash
rosrun turtlesim turtlesim_node
```
#### 6.2使用roslaunch
roslaunch可以用来启动定义在launch文件中的多个节点。用法：

```bash
roslaunch [package] [filename.launch]
```
先切换到beginner_tutorials程序包目录下：

```bash
roscd beginner_tutorials
```
如果roscd执行失败了，记得设置你当前终端下的ROS_PACKAGE_PATH环境变量，设置方法如下

```bash
export ROS_PACKAGE_PATH=~/catkin_ws/:$ROS_PACKAGE_PATH
```
创建一个launch文件夹

```bash
mkdir launch
cd launch
```
创建一个名为turtlemimic.launch的launch文件，件里面

```bash
touch turtlemimic.launch
```
并复制粘贴以下内容到该文件
```xml
   <launch>  <!--以launch标签开头以表明这是一个launch文件 -->
   <!--
   在这里我们创建了两个节点分组并以'命名空间（namespace)'标签来区分，其中一个名为turtulesim1，另一个名为turtlesim2，两个组里面都使用相同的turtlesim节点并命名为'sim'。这样可以让我们同时启动两个turtlesim模拟器而不会产生命名冲突。 
   -->
     <group ns="turtlesim1">
       <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
     </group>
   
     <group ns="turtlesim2">
       <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
     </group>
   <!--
   在这里我们启动模仿节点，并将所有话题的输入和输出分别重命名为turtlesim1和turtlesim2，这样就会使turtlesim2模仿turtlesim1。
   -->
     <node pkg="turtlesim" name="mimic" type="mimic">
       <remap from="input" to="turtlesim1/turtle1"/>
       <remap from="output" to="turtlesim2/turtle1"/>
     </node>
   
   </launch>
```

现在让我们通过roslaunch命令来启动launch文件：

```bash
roslaunch beginner_tutorials turtlemimic.launch
```
在将会有两个turtlesims被启动，然后我们在一个新终端中使用rostopic命令发送速度设定消息

```bash
rostopic pub /turtlesim1/turtle1/command_velocity turtlesim/Velocity -r 1 -- 2.0  -1.8
```

## 7.创建ROS消息和ROS服务
- 消息(msg): msg文件就是一个描述ROS中所使用消息类型的简单文本。它们会被用来生成不同语言的源代码。

- 服务(srv): 一个srv文件描述一项服务。它包含两个部分：请求和响应。

msg文件存放在package的msg目录下，srv文件则存放在srv目录下。

msg文件实际上就是每行声明一个数据类型和变量名。可以使用的数据类型如下：
- int8, int16, int32, int64 (plus uint*)
- float32, float64
- string
- time, duration
- other msg files
- variable-length array[] and fixed-length array[C]
在ROS中有一个特殊的数据类型：Header，它含有时间戳和坐标系信息。在msg文件的第一行经常可以看到Header header的声明.
#### 7.1 使用msg
在之前创建的package里定义新的消息

```bash
cd ~/catkin_ws/src/beginner_tutorials
mkdir msg
echo "int64 num" > msg/Num.msg
```
接下来，还有==关键==的一步：我们要确保msg文件被转换成为C++，Python和其他语言的源代码：
查看package.xml, 确保它包含一下两条语句:

```xml
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
```
在 CMakeLists.txt文件中，利用find_packag函数，增加对message_generation的依赖，这样就可以生成消息了。 你可以直接在COMPONENTS的列表里增加message_generation，就像这样：

```cmake
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
)
```
还有运行依赖

```cmake
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES beginner_tutorials
  CATKIN_DEPENDS roscpp std_msgs
#  DEPENDS system_lib
  message_runtime
)

```

```cmake
add_message_files(
  FILES
  Num.msg
)
```
手动添加.msg文件后，我们要确保CMake知道在什么时候重新配置我们的project。 确保添加了如下代码:

```cmake
generate_messages()
```
具体文件见GitHub：[CMakeList.txt](https://github.com/DarrenSong/catkin_ws/blob/master/src/beginner_tutorials/CMakeLists.txt)
##### 使用

```bash
rosmsg show [message type]
```
具体方法

```bash
rosmsg show beginner_tutorials/Num  #rosmsg show Num
```

#### 7.2 使用msv
在刚刚那个package中创建一个服务：

```bash
roscd beginner_tutorials
mkdir srv
```
这次我们不再手动创建服务，而是从其他的package中复制一个服务。 roscp是一个很实用的命令行工具，它实现了将文件从一个package复制到另外一个package的功能。
使用方法:

```bash
 roscp [package_name] [file_to_copy_path] [copy_path]
```
从rospy_tutorials package中复制一个服务文件了：

```bash
roscp rospy_tutorials AddTwoInts.srv srv/AddTwoInts.srv
```
还有很关键的一步：我们要确保srv文件被转换成C++，Python和其他语言的源代码。
还是再CMakeLists.txt中增加：

```cmake
add_service_files(
  FILES
  AddTwoInts.srv
)
```
##### 使用

```bash
 rossrv show <service type>
```

```bash
rossrv show beginner_tutorials/AddTwoInts #  rossrv show AddTwoInts
```
修改MakeLists.txt

```cmake
generate_messages(
  DEPENDENCIES
  std_msgs
)
```
编译

```bash
cd ~/catkin_ws/
catkin_make
```

## 8.编写简单的消息发布器和订阅器并测试（C++）
#### 8.1 编写消息发布器
『节点』(Node) 是指 ROS 网络中可执行文件。接下来，我们将会创建一个发布器节点("talker")，它将不断的在 ROS 网络中广播消息
切换到之前创建的 beginner_tutorials package 路径下：

```bash
cd ~/catkin_ws/src/beginner_tutorials
```
创建一个src文件

```bash
mkdir -p ~/catkin_ws/src/beginner_tutorials/src
```
在src中新建一个talker.cpp文件，复制[talker.cpp](https://github.com/DarrenSong/catkin_ws/blob/master/src/beginner_tutorials/src/talker.cpp)的内容

```cpp
//ros/ros.h 是一个实用的头文件，它引用了 ROS 系统中大部分常用的头文件。
#include "ros/ros.h"
//这引用了 std_msgs/String 消息, 它存放在 std_msgs package 里，是由 String.msg 文件自动生成的头文件。需要关于消息的定义，可以参考 msg 页面。
#include "std_msgs/String.h"

#include <sstream>

int main(int argc,char **argv)
{
/*
初始化 ROS 。它允许 ROS 通过命令行进行名称重映射——然而这并不是现在讨论的重点。在这里，我们也可以指定节点的名称——运行过程中，节点的名称必须唯一。

这里的名称必须是一个 base name ，也就是说，名称内不能包含 / 等符号
*/
  ros::init(argc,argv,"talker");
  /*
为这个进程的节点创建一个句柄。第一个创建的 NodeHandle 会为节点进行初始化，最后一个销毁的 NodeHandle 则会释放该节点所占用的所有资源。
*/
  ros::NodeHandle n;
/*
chatter 话题的节点，将要有数据发布。第二个参数是发布序列的大小。如果我们发布的消息的频率太高，缓冲区中的消息在大于 1000 个的时候就会开始丢弃先前发布的消息。

NodeHandle::advertise() 返回一个 ros::Publisher 对象,它有两个作用： 1) 它有一个 publish() 成员函数可以让你在topic上发布消息； 2) 如果消息类型不对,它会拒绝发布。
*/  
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter",1000);
/*
ros::Rate 对象可以允许你指定自循环的频率。它会追踪记录自上一次调用 Rate::sleep() 后时间的流逝，并休眠直到一个频率周期的时间。

在这个例子中，我们让它以 10Hz 的频率运行。
*/
  ros::Rate loop_rate(10);
/*
roscpp 会默认生成一个 SIGINT 句柄，它负责处理 Ctrl-C 键盘操作——使得 ros::ok() 返回 false。

如果下列条件之一发生，ros::ok() 返回false：

SIGINT 被触发 (Ctrl-C)
被另一同名节点踢出 ROS 网络
ros::shutdown() 被程序的另一部分调用

节点中的所有 ros::NodeHandles 都已经被销毁

一旦 ros::ok() 返回 false, 所有的 ROS 调用都会失效。
*/
  int count=0;
  while(ros::ok())
  {
    /*
    我们使用一个由 msg file 文件产生的『消息自适应』类在 ROS 网络中广播消息。现在我们使用标准的String消息，它只有一个数据成员 "data"。当然，你也可以发布更复杂的消息类型。
    */
    std_msgs::String msg;
    std::stringstream ss;
    ss<<"hello world"<<count;
    msg.data=ss.str();
   /*
ROS_INFO 和其他类似的函数可以用来代替 printf/cout 等函数
*/ 
    ROS_INFO("%s",msg.data.c_str());

    chatter_pub.publish(msg);//这里，我们向所有订阅 chatter 话题的节点发送消息。
/*
在这个例子中并不是一定要调用 ros::spinOnce()，因为我们不接受回调。然而，如果你的程序里包含其他回调函数，最好在这里加上 ros::spinOnce()这一语句，否则你的回调函数就永远也不会被调用了。
*/
    ros::spinOnce();
    loop_rate.sleep();
    ++count;
  }

  return 0;
}
```

#### 8.2 编写订阅器
在src下新建listener.cpp文件，复制[listener.cpp](https://github.com/DarrenSong/catkin_ws/blob/master/src/beginner_tutorials/src/listener.cpp)

```cpp
#include "ros/ros.h"
#include "std_msgs/String.h"
/*
这是一个回调函数，当接收到 chatter 话题的时候就会被调用。消息是以 boost shared_ptr 指针的形式传输，这就意味着你可以存储它而又不需要复制数据。
*/
void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
    ROS_INFO("I heard: [%s]",msg->data.c_str());
}

int main(int argc,char **argv)
{

    ros::init(argc,argv,"listener");
    ros::NodeHandle n;
/*
告诉 master 我们要订阅 chatter 话题上的消息。当有消息发布到这个话题时，ROS 就会调用 chatterCallback() 函数。第二个参数是队列大小，以防我们处理消息的速度不够快，当缓存达到 1000 条消息后，再有新的消息到来就将开始丢弃先前接收的消息。

NodeHandle::subscribe() 返回 ros::Subscriber 对象,你必须让它处于活动状态直到你不再想订阅该消息。当这个对象销毁时，它将自动退订 chatter 话题的消息。

有各种不同的 NodeHandle::subscribe() 函数，允许你指定类的成员函数，甚至是 Boost.Function 对象可以调用的任何数据类型。roscpp overview 提供了更为详尽的信息。
*/
    ros::Subscriber sub = n.subscribe("chatter",1000,chatterCallback);
/*
ros::spin() 进入自循环，可以尽可能快的调用消息回调函数。如果没有消息到达，它不会占用很多 CPU，所以不用担心。一旦 ros::ok() 返回 false，ros::spin() 就会立刻跳出自循环。这有可能是 ros::shutdown() 被调用，或者是用户按下了 Ctrl-C，使得 master 告诉节点要终止运行。也有可能是节点被人为关闭的。
*/
    ros::spin();



    return 0;
}
```
编译：
在CMakeLists.txt中增加以下内容

```cmake
include_directories(include ${catkin_INCLUDE_DIRS})

add_executable(talker src/talker.cpp)
target_link_libraries(talker ${catkin_LIBRARIES})

add_executable(listener src/listener.cpp)
target_link_libraries(listener ${catkin_LIBRARIES})
```
切换到catkin_ws目录编译

```bash
cd ~/catkin_ws
catkin_make
```

#### 8.3 测试
确保roscore可用，并运行：

```bash
roscore
```
==catkin specific== 如果使用catkin，确保你在调用catkin_make后，在运行你自己的程序前，已经source了catkin工作空间下的setup.sh文件：

```bash
cd ~/catkin_ws
source ./devel/setup.bash
```
运行发布器

```bash
rosrun beginner_tutorials talker
```
运行订阅器

```bash
rosrun beginner_tutorials listener
```

## 9.编写简单的服务器和客户端并测试（C++）

#### 9.1 服务器
进入先前你在catkin workspace教程中所创建的beginner_tutorials包所在的目录：

```bash
cd ~/catkin_ws/src/beginner_tutorials
```
创建src/add_two_ints_server.cpp文件

```cpp
#include "ros/ros.h"
//beginner_tutorials/AddTwoInts.h是由编译系统自动根据我们先前创建的srv文件生成的对应该srv文件的头文件。
#include "beginner_tutorials/AddTwoInts.h"
/*
这个函数提供两个int值求和的服务，int值从request里面获取，而返回数据装入response内，这些数据类型都定义在srv文件内部，函数返回一个boolean值。
*/
bool add(beginner_tutorials::AddTwoInts::Request  &req,
         beginner_tutorials::AddTwoInts::Response &res)
{
  res.sum = req.a + req.b;
  ROS_INFO("request: x=%ld, y=%ld", (long int)req.a, (long int)req.b);
  ROS_INFO("sending back response: [%ld]", (long int)res.sum);
  return true;
}

int main(int argc, char **argv)
{
  ros::init(argc, argv, "add_two_ints_server");
  ros::NodeHandle n;

  ros::ServiceServer service = n.advertiseService("add_two_ints", add);
  ROS_INFO("Ready to add two ints.");
  ros::spin();

  return 0;
}
```

#### 9.2 客户端
创建src/add_two_ints_client.cpp文件

```cpp

#include "ros/ros.h"
#include "beginner_tutorials/AddTwoInts.h"
#include <cstdlib>

int main(int argc, char **argv)
{
  ros::init(argc, argv, "add_two_ints_client");
  if (argc != 3)
  {
      ROS_INFO("usage: add_two_ints_client X Y");
      return 1;
    }
  
    ros::NodeHandle n;
    ros::ServiceClient client = n.serviceClient<beginner_tutorials::AddTwoInts>("add_two_ints");
    beginner_tutorials::AddTwoInts srv;
    srv.request.a = atoll(argv[1]);
    srv.request.b = atoll(argv[2]);
    if (client.call(srv))
    {
      ROS_INFO("Sum: %ld", (long int)srv.response.sum);
    }
    else
    {
      ROS_ERROR("Failed to call service add_two_ints");
      return 1;
    }
  
    return 0;
  }
```
编辑一下beginner_tutorials里面的CMakeLists.txt
添加下面代码

```cmake
add_executable(add_two_ints_server src/add_two_ints_server.cpp)
target_link_libraries(add_two_ints_server ${catkin_LIBRARIES})
add_dependencies(add_two_ints_server beginner_tutorials_gencpp)

add_executable(add_two_ints_client src/add_two_ints_client.cpp)
target_link_libraries(add_two_ints_client ${catkin_LIBRARIES})
add_dependencies(add_two_ints_client beginner_tutorials_gencpp)
```
编译

```bash
cd ~/catkin_ws
catkin_make
```

#### 9.3 测试
- 运行service

```bash
rosrun beginner_tutorials add_two_ints_server
```

- 运行client

```bash
rosrun beginner_tutorials add_two_ints_client 1 3
```

## 参考

[ROS 官方教程](http://wiki.ros.org/roschina/%E6%95%99%E7%A8%8B)
[github源码](https://github.com/DarrenSong/catkin_ws)