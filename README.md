# Guidance-Onboard-mixture (unfinished)
这份文档的内容需要硬件支持  
功能：将 Guidance-sdk 嵌入到 Onboard-sdk, Guidance-sdk-ros 嵌入到 Onboard-sdk-ros  
(备选功能：将 Onboard-sdk 嵌入到 Guidance-sdk, Onboard-sdk-ros 嵌入到 Guidance-sdk-ros)  
## 硬件和环境准备
1 DJI M100, Guidance (避障系统)  
2 manifold (tk1), ubuntu-tegra (v14.04), ros-indigo (opencv-2.4.8), cmake(>=2.8)  
3 windows-PC (e.g. win10), DJI-Asistant-2 (仿真), Guidance 调参软件  
4 DJI GO App  
## 安装 Onboard-SDK (OSDK)
在开发者网站上注册一个账号 https://developer.dji.com  
登录后，进入右上角 Developer Center, 申请一个 app  
```Bash
SDK: Onboard SDK | APP Name: 随意 | Category: 自己选 | Description: 随意
```
获得 APP ID 和 APP Key  

连接 win10, M100, manifold, Guidance, 遥控器, 安卓手机  
打开 DJI-Asistant-2 (win10), DJI GO App (手机), 联网更新固件  
ubuntu: 下载 Onboard SDK https://github.com/dji-sdk/Onboard-SDK/releases  
win10: 在 DJI-Asistant-2 中打开 API 控制, 确认波特率为 230400  
