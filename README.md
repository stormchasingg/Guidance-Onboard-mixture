# Guidance-Onboard-mixture (unfinished)
这份文档的内容需要硬件支持  
功能：将 Guidance-SDK 嵌入到 Onboard-SDK, Guidance-SDK-ROS-demo 嵌入到 Onboard-SDK-ROS  
(备选功能：将 Onboard-SDK 嵌入到 Guidance-SDK, Onboard-SDK-ROS 嵌入到 Guidance-SDK-ROS-demo)  
## 硬件和环境准备
1 DJI M100, Guidance (避障系统)  
2 manifold (tk1), ubuntu-tegra (v14.04), CUDA, OpenCV4tegra, OpenCV-2.4.10, ros-indigo (opencv-2.4.8), cmake(>=2.8)  
3 windows-PC (e.g. win10), DJI-Asistant-2 (仿真), Guidance 调参软件  
4 DJI GO App  
## 1 Guidance-SDK 嵌入到 Onboard-SDK (OSDK)
不包含 ros  
在开发者网站上注册一个账号 https://developer.dji.com  
登录后，进入右上角 Developer Center, 申请一个 app  
```Bash
SDK: Onboard SDK | APP Name: 随意 | Category: 自己选 | Description: 随意
```
获得 APP ID 和 APP Key  

连接 win10, M100, manifold, Guidance, 遥控器, 安卓手机  
打开 DJI-Asistant-2 (win10), DJI GO App (手机), Guidance 调参软件, 联网更新固件  
ubuntu: 下载 Onboard SDK 3.4 https://github.com/dji-sdk/Onboard-SDK  
ubuntu: 下载 Guidance SDK https://github.com/dji-sdk/Guidance-SDK  
win10: 在 DJI-Asistant-2 中打开 API 控制, 确认波特率为 230400  
win10: 在 Guidance 调参软件中打开 USB 和 UART, 不标定, 直接使用 SDK 中的双目相机内外参数    
## 编译 Onboard-SDK 
```Bash
cd OSDK-3.4
mkdir build & cd build
cmake..
make # 编译所有例子
sudo make install djiosdk-core
# cp ../sample/linux/common/UserConfig.txt bin/
cd bin
```
将 App ID, Key 和串口 "ttyTHS1" (对应 manifold 中的 uart 2) 填入到 UserConfig.txt  
路径：~/Onboard-SDK/sample/linux/common/UserConfig.txt  
遥控器拨到 F 档  
```Bash
./djiosdk-flightcontrol-sample ../../sample/linux/common/UserConfig.txt
# ./djiosdk-flightcontrol-sample UserConfig.txt
```
按 A 或 B 键 启动例程  
## 编译 Guidance-SDK
略  
## 嵌入
将 Guidance-SDK 复制到 Onboard-SDK 的 flight-control 中  
改写 flight-control 中的 main.cpp:  
```Bash
# 伪代码
#include "Guidance 头文件"
```
改写 flight-control中的 CMakeLists.txt:  
```Bash
# 伪代码
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread -g -fno-stack-protector -O0")
add_executable(${PROJECT_NAME} ${SOURCE_FILES} # 删除原来的
Guidance 头文件对应的 cpp 文件）
target_link_libraries(${PROJECT_NAME} djiosdk-core  
   ${PROJECT_SOURCE_DIR}/include/libDJI_guidance.so)
```
include 文件夹中的 libDJI_guidance.so 更换为 XU3 的 libDJI_guidance.so  
```Bash
sudo cp libDJI_guidance.so /usr/local/lib # 或者复制到执行文件同一目录下
```
重新编译 OSDK  
## 2 Guidance-SDK-ROS-demo 嵌入到 Onboard-SDK-ROS
前提：已经建好了 catkin_ws  
## 编译 Onboard-SDK-ROS
同样将 App ID, Key 和串口 "ttyTHS1" 填入到 sdk.launch  
路径: ~/catkin_ws/src/Onboard-SDK-ROS/dji_sdk/launch/sdk.launch  
```Bash
cd catkin_ws
catkin_make
roslaunch dji_sdk sdk.launch
rosrun dji_sdk_demo demo_flight_control
```
## 编译 Guidance-SDK-ROS-demo
略
## 嵌入
改写 ~/catkin_ws/src/discontinuity-balance_strategy 中的 CMakeLists.txt:  
```Bash
project(dji_sdk_guidance)
...
add_executable(dji_sdk_node_guidance src/dji_sdk_node_guidance.cpp ${DJI_SDK_LIB_SOURCES})
...
target_link_libraries(dji_sdk_node_guidance ...)
```
改写 package.xml: 
```Bash
<name>dji_sdk_guidance</name>
<description>The dji_sdk_guidance package</description>
```
