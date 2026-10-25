## ROS Command

#### 查看软件包地址
    roscd roscpp

### 启动一个ROS节点

#### 0.为.bashrc添加环境变量
###### 为了让catkin和ros中的环境变量(功能包路径和可执行文件路径)加载到终端会话中, 需要添加环境变量
    source /opt/ros/noetic/setup.bash
    source ~/catkin_ws/devel/setup.bash

#### 1.启动roscore
    roscore

#### 2.启动节点
    rosrun rosetest_pkg rosetest_pkg_node
###### 若显示在包下找不到节点:
1.检查是否已经编译
2.检查是否已经有 rose_ws/devel/lib/rosetest_pkg/rosetest_pkg/rosetest_pkg_node
3.可能需要重新source opt/ros/noetic/setup.bash 和 source /rose_ws/devel/setup.bash

## ROS Code

#### 头文件:
    #include <ros/ros.h>

#### using namespace:
不建议在ros中使用任何using namespace std等, 因为这会把整个程序放在std作用域下, 污染命名空间
容易造成某个命名与标准库相同而产生冲突, 所以标准库提供的功能要加std::, 而ros提供的功能加ros::
如果实在是出现频率极高, 那么可以在某个.cpp文件开头或者某个函数内写 using std::string, 但不建议多用
    std::cout, cin, endl, to_string
    std::string, vector, map, array
    

#### 主函数:
    int main(int argc, char **argv) {}
###### 意为:主函数需要传入的参数, argc: argument count: 主函数需要传进的数据数
###### **argv: argumrnt vector: 主函数需要传进的数据vector, 类型是字符串数组
###### 用途: 在启动程序时将命令行的指令塞给main函数:`rosrun rosetest_pkg rosetest_pkg_node __name:=hello_node`
arg[0] = rosrun, argv[1] = my_pkg argv[2] = my_node argv[3] = __name:=hello
此时argc就等于4

#### ros::init:
    ros::init(argc, argv, "rosetest_pkg_node")
###### 将主函数接收的启动指令传给ros:init函数, 其中第三个参数"rosetest_pkg_node"是节点的名字
###### 若主函数含有__name:=hello则会强制改名为hello, init的名字是跑起来后显示的节点名字
###### CMakeLists.txt里面的包和节点名字将决定rosrun启动时需要输入的名字

#### 保持程序执行:
    while(ros::ok())
###### 如果不加循环的话, 程序只会从头到尾执行一次, 加`while(ture)`的话程序不会响应终端的Ctrl+C