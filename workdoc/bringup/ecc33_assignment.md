
# exc343 assignment 

## framework
&emsp;  该系统的把电脑传输过来的视频和显示投影到显示屏上。 
![picture 4](images/333d8bcdf5f7758bae6024cb8e542a539edb6d888099382d34c6e6f92883c9b8.png)  


## ToDO:
1. 7211b change to 7211uxc (load hex for iic)
- (1) learn how to update firmware，usd IIC
- (2) think
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
- describtion:
  The ECX335S is an OLED panel module with active matrix colour design, exceptional brightness of up to 3,000 cd/m², Full HD resolution with 1,920 x 1,080 RGB pixels, and a diagonal of 1.8 cm (0.71 inch). Its power consumption is low even at a frame rate of 60 fps. Supoort Mini LVDS
- used:
  

## Systerm description 

- 电脑usb3.0接口，连接ANX7327多路复用器，分解出两路通道，一路usb3.0 连接cpu,另一路连接7211b转接器，转换出LVDS信号。
- ANX7327分解出 usb3.0连接主控芯片MA2150，实现数据交付。
- 主控芯片，通过IIC 把更新7211b的fireware（hex）。通过svnyc中断，同步控制211b芯片。实现像素，显示size的控制。
