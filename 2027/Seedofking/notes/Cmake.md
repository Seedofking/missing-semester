### 编译过程
1. 预处理：将.h文件复制到.cpp, 并且展开include部分的代码 .i
2. 编译: 生成汇编代码 .s
3. 汇编: 生成二进制文件 .o
4. 链接: 生成可执行文件
###### 在有多个源代码的时候可以实现文件的增量更改, 即单独构建指定的文件, 已构建的其他文件可以不构建








### 该笔记由一个CMakeList.txt改为md, 既是可运行代码也是笔记

``` cmake

cmake_minimum_required(VERSION 3.28)#指定CMake可用的最低版本，比该版本低会报错

project(BigBuild)#指定要构建项目的名字，随便写

set(CMAKE_CXX_STANDARD 20)#指定C++语言的标准版本

set(CMAKE_EXE_LINKER_FLAGS "-static")
#设置链接方式为静态链接，(cmake默认是动态链接)，动态链接会让.exe可执行文件每次运行时会寻找其依赖文件，将.exe移出文件夹其就会失效
#而静态链接方式生成的.exe文件不会有外部依赖，放在各种地方都可以运行

#单文件夹单工程多文件编译
#add_executable(Class_1 Class_1_main.cpp Class_1_App.cpp)
#executable是可执行的意思
#Class_1是指定生成的可执行文件(.exe)的名字，会生成在cmake-build-debug里面
#工程名加一个空格 后面可以跟一个或多个.cpp文件，.h文件可不写，可以用空格间隔，可以用; 间隔

#[[
若想避免手写，则可以使用file来收集目录下所有的文件，存在一个名字里
file(GLOB Class_1_Files(名字自取）  ————如果只想取该目录下一层的文件，则用GLOB，如果想取该目录下全部子文件里的文件，则用GLOB_RECURSE
        "${PROJECT_SOURCE_DIR}/tests/*.h"
        "${PROJECT_SOURCE_DIR}/tests/*.h"
    )
add_executable(Class_1 ${Class_1_Files})
]]

#单文件夹多工程编译
add_executable(Function_Test Function_Test_App.cpp main.cpp)    #多文件
add_executable(Class_2 Class_2.cpp)   #单文件
add_executable(Class_3 Class_3.cpp)
add_executable(Class_4 Class_4.cpp)
add_executable(Class_Test CLass_Test.cpp)
add_executable(First_Time First_Time.cpp)
add_executable(IO_Test IO_Test.cpp)
add_executable(Memory_Test Memory_Test.cpp)
add_executable(Pointer_Test Pointer_Test.cpp)
add_executable(Reference Reference.cpp)
#一个个写即可

#target_include_directories(Class_1 PRIVATE ${PROJECT_SOURCE_DIR})

set(A ON) #定义A为ON，ON属于关键字
if (A)   #A是ON，if会被判定为true
    #message可以将信息打印出来，点击重新加载cmake项目时可在cmake日志查看，是--开头的，比如-- A:D:/3-Clion/C++/Class_1
    #message(STATUS "   ")可显示冒号内的内容
    #$(   )的作用是查看其内容的值
    message(STATUS "A:${PROJECT_NAME}") #显示工程的名称
    message(STATUS "A:${PROJECT_SOURCE_DIR}")   #显示工程的文件夹源文件目录，即包含CMakeLists.txt文件的目录
    message(STATUS "A:${PROJECT_BINARY_DIR}")   #显示当前CMake运行的二进制文件目录
endif ()


#[[工具链设置：
0.如果改编译配置先把原来存在的编译文件删掉
1.要在设置里把工具链配好，有stm32和mingGW
2.要在调试那里把Debug的工具链选为mingGW，Generator也会自动跟着工具链选好
3.设置运行时可能需要执行的操作和参数
]]

#[[
CMAKE_CXX_FLAGS 指定编译C++代码时使用的编译选项
CMAKE_C_FLAGS   指定编译C代码时使用的编译选项
CMAKE_EXE_LINKER_FLAGS  指定链接可执行文件时的链接选项
CMAKE_SYSTEM_NAME   指定当前操作系统名称(Windows, Linux)
CMAKE_SYSTEM_PROCESSOR  指定当前处理器类型(x86, x86_64等)
CMAKE_CXX_COMPILER_ID   指定当前使用的C++编译器，同理可得编译器对应的名字

include_directories(${如PROJECT_SOURCE_DIR}/include)   也是暴露一个目录，用于在编译阶段暴露头文件
link_directories( ${如PROJECT_SOURCE_DIR}/lib)将一个目录的文件都暴露给整个Cmake工程，{}里面是一个路径，主要在链接阶段暴露库文件(.lib .a .so)
add_library(自己取的库名称，文件如main.cpp) 将后面的文件生成一个库，供其他程序使用
target_link_libraries(工程名PROJECT_NAME，库名称)  给工程链接一个库，使用之前要先用link_directories暴露库路径

]]
```