
# Bringup 任务总结报告 --向莹光
| 任务| 进度 | 问题 | 下一步计划|
| ------ | ------ | ------ |------ |
| Bringup显示 | 未完成| ECX343无法点亮 |1.问题解决研讨会




## <center> Hard framework 
&emsp;  该系统的把电脑和手机上显示的内容传输过来的视频和显示投影到显示屏上。相当于外屏幕
![picture 3](../images/b8b86e24f7d4adbca2c65a4ebf76afb57187b4ea5d6a2e5ed19b5113d5cf08a6.png)  


## ToDO:
1. 7211b change to 7211uxc (load hex for iic)
- (1) learn how to update firmware，usd IIC
- (2) learn 
2. ecx333 change to exc343 
- (1) MIPI/LVDS drivers(60HZ -> 120HZ)
- (2) learn optimize 

## component description
1. 7211b
- The LT7211B is a high performance Type-C/DP1.2 to LVDS chip for VR/Display application.
- DP-Display Port
- LT7211B   
  ![picture 3](images/527d09fa0067a9b817be36315bf92f5c470286b1c951fe50acd6bc3033c380b7.png)  

1. ANX7327 
- ANX7327是一款USB-PD v3.0端口控制器，具有集成的多路复用器，支持以高达10Gbps的数据速率进行交换。ANX7327支持高达的高速接口.
- USB-PD (USB Power Delivery功率传输协议)
  
3. CPU (Intel Movidius Myriad 2 - MA2150)
- Movidius Myriad 2是一个视觉处理单元（VPU），可在不同目标应用中提供低功耗、高性能的视觉处理解决方案，其中包括嵌入式深度神经网络、位姿估计、3D深度感应、视觉惯性测距，以及手势/眼部跟踪
  

4. EXC335
- The ECX335S is an OLED panel module with active matrix colour design, exceptional brightness of up to 3,000 cd/m², Full HD resolution with 1,920 x 1,080 RGB pixels, and a diagonal of 1.8 cm (0.71 inch). Its power consumption is low even at a frame rate of 60 fps. Supoort Mini LVDS

## Systerm description 

- 电脑usb3.0接口，连接ANX7327多路复用器，分解出两路通道，一路usb3.0 连接cpu,另一路DP信号连接7211b，转换出LVDS信号。
- ANX7327分解出 usb3.0连接主控芯片MA2150，实现数据交付。
- 主控芯片，通过IIC 把更新7211b的fireware（hex）。通过SPI配置ECX343显示屏，实现分辨率，刷新频率等更新。

## <center> Soft framework
### 开发调试总结
1. 首先，开发任务梳理清楚。
- （1）本次任务，主要是更新桥接芯片为LT7911UXC
- （2）重新配置343屏幕，更新屏幕。
2. 开发调试过程。
- 首先需要把所要实现的任务原理和代码流程分析和梳理出来。
- 为代码梳理要修改的点。
- 软件编写调试，检查没有问题后。排查硬件是否正常工作。
- 当软件和硬件都正常工作后，需要排查，第三方提供固件，版本，参数是否正确。
- 如果版本和参数，固件都没有问题，一定是软件流程有问题，检测软件流程。
3. 当期遇到问题
- <font color='red'> 0x07,0X97和亮度寄存器配置和获取不一样。
- ECX343只在仿真调试下，引脚供电电源才是正常的。</font> 
- LT7911uxc异常INT 没有使用，屏的VSYNC信号没有。
### 软件整体框架
![picture 2](../images/c7bfc945a36b0ae94e9cd63209ab99718075a145e2680f2a523a3f8d8025ba1a.png)  

### <center>LT7911代码流程
1. 软件流程
![picture 3](../images/877b1f5aae1a4c0a7efad9ea2e510a8807ece99bf0bdde2365d6f9edd93f5aa5.png)  

2. 7911上电
- 伪代码： 
  - #清除状态
  - GPIO59-3.3V 置位0 
  - 延时2ms （rtems_task_wake_after(2)）
  - GPIO64-1.8V 置位0 ，清除状态
  - 延时2ms （rtems_task_wake_after(2)）
  - GPIO42-1.1V 置位0 ，清除状态     
  - 延时1s （rtems_task_wake_after(1000)）,等待电容充分放电
  - #上电
  - GPIO42-1.1V 置位1 
  - 延时5ms （rtems_task_wake_after(5)）
  - GPIO64-1.8V 置位1 ，上电
  - 延时5ms （rtems_task_wake_after(5)）
  - GPIO59-3.3V 置位1 ，上电
  - 延时150ms （rtems_task_wake_after(150)）
  - #等待开始工作
  - GPIO19- 复位 置位1 
  - sleep(3) 3s等待芯片工作
  - GPIO18- 模式切换 置位0 设置为2D，上电
- 时序图：
  ![picture 4](../images/ef7c6f4210884f564f562b2963d3d61e5eec5fc2a6c144f2888abbd855a4fd9f.png)  

- 上电lt7211过程
![picture 5](../images/f4168fea240c2b2b5afa972acbf20285a12831e9b8e0588d8f68843008a815c3.png)  
- 上电lt7911过程
![picture 6](../images/22534dc4ad6f0fd1ee939d3bfe18c844d78251f5559f330ce37e02cd26c5ab50.png)  

### <center>ECX343代码流程
1. 首先介绍下343当前代码与之前倪工写的代码的差异。
  - LT7211帧同步信号， 只判断LT7211B_TE为高。
  - 在倪工的源代码中，开屏完成后，多配置了，0x71,0x6D，0x71,0x72,0X03,0X00,0X01这几个寄存器。这个我尝试放开没有效果。
  - 在倪工的之前版本代码值中，第一步和第二步寄存器配置完成后。重新把10v拉高，这个没有作用我去掉了。

2. ECX343 判断是否接收到帧同步信号。display_init 函数实现
- 流程图
![picture 7](../images/e43b5b8aed92c6bc2b59ff08cbe731c7819ac7522a7c5e1fa9fa2f6de88a6892.png)  
- 代码
![picture 8](../images/651cc3f5839f230acee71a34fab404ac00ce17d39718db92ca8ce0de6c8c0b3f.png)  



1. ECX上电过程
- 硬件时序图
  ![picture 13](../images/3d28154d2d392905466461f1aff87bb3f1d091fc9c96a5cf21af0b58e20a9e50.png)  
- 开屏流程
![picture 14](../images/e4161addd11e2f4d2c0be4503c457ce4a6f4a37a28b60f0f0afd7566394c5f1d.png)  
- 开屏伪代码
#1. 开屏上电： 1.8V 已经在7911上电时，提供了。
  -  负6.6V(gpio41)引脚置为 1 ，拉高防止漏电。
  - 屏幕10V放电。(gpio20)
  - 左右屏幕 （gpio 14/15）  置为0 。
  - delay 5ms
  - 给屏幕 10V （gpio 0 ）
  - 负6.6V(gpio41)引脚置为 0 
  - delay 5ms
  - 左右屏幕复位 （gpio 14/15）置为1 。
#2. 寄存器配置 
  - (1)0x01~0xFA 寄存器写入250个值。使用表格中第5组数据。
  - delay 2ms
  - (2)0x10,0x12,0x13 寄存器分别写入 0x0A,0x01,0x3f
  - delay 2ms
  - (3)0x00寄存器写入0x01
  - delay 2ms
  - (4)0x00寄存器写入0x03
  - delay 0.2ms
  - (5)0x00寄存器写入0x07
  - <font color='red'> 等待屏幕vsync信号。当前是没有的</font> 

