# 上周投入
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
|大棚:现场解决问题| 中 |100%|线材问题引起的DP问题，配合debug线材问题集成固件|已经解决|1Ｄ|
|大棚:升级工具升级失败问题| 中 |10%|升级会超时，判断为线材的问题，升级那次固件启动失败，目前只在某些设备出现|解决中|1.5D|
|瑞航：磁模块，显示模块| 中 | 5% |和张工梳理地磁代码结构| 解决中|1H|
|资料整理| 中 |10%|整理CPU升级框架|输出固件|1.5D|


# 本周计划
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
|大棚：配合DP解决问题| 中 |0%|协助大鹏解决复现问题|解决中|2Ｄ|
|瑞航：磁模块，显示模块| 中 | 5% |一起看地磁代码| 解决中|1D|
|R500：安卓源码拉取和编译| 中 | 5% |配合鹏哥搭建DOCKER环境| 解决中|1D|

# 挂起任务
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
|框架优化 | 低 | 15%  | 输出一版细化，再次会议讨论结构体设计 | 待解决 | １０D|
|BUG：DP信号问题 | 中| ７０%  | EPSON２.0,3.0判断条件判断条件存在问题，导致逻辑结果存在误判,后续需要需要验证下对版本号是否对ＤＰ是否有影响|解决中 |２D|
|JunkeV2| 中 | 10%  |fisheye帧间隔问题imu上报数据问题| 提交代码|2D|
|E34R静电检测| 中 | 10%  |HID单步调试，解决| 提交代码|2D|
|固件组:资料整理| 中 |10%|打包，升级，flash存储，CPU运行框架|ppt|2D|

## 【缺陷解决】
1. <font color='red'> 【高优先级】  </font>大棚产线
- [问题1]有概率升级失败问题。
   - 升级会超时。
   - 升级会偶现固件启动失败，表现就是设备没有端口，重新插拔后解决。

- [问题6]仿真过程中，遇到FLASH读取指针崩溃的问题，仿真过程中出现，数据类型长度会出现异常长度值。
   - 仿真情况下，暂时不定位该问题的。

2. 总结framework的整体知识ppt

3.K40R audio的问题，需要定位。
地磁怎么转换方向角。
（1）裸data。
（2）怎么转换方向。
（3）怎么校准坐标。


错误


UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 1
UAC1DeviceI_StreamEvent:UAC Speaker disable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 4
audioControllerThread:-->AudioController thread: started!
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 4
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 0
UAC1DeviceI_StreamEvent:UAC Speaker enable
UAC1DeviceI_StreamEvent:UAC Speaker disable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Speaker enable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Speaker disable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Speaker enable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Microphone enable
UAC1DeviceI_StreamEvent:UAC Speaker disable
UAC1DeviceI_StreamEvent:UAC Microphone disable
UAC1DeviceI_StreamEvent:UAC Speaker enable
UAC1DeviceI_StreamEvent:UAC Microphone enable
audioControllerThread:audio speaker mode
I2S: I2SPlay enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: i2s record enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
I2S: I2SPlay enable
tof_algo_init_mt010:TOF: Finish reading the EEPROM data, took 2959.121 ms

audioMute:UnMmute speaker
UAC1DeviceI_StreamEvent:UAC Microphone disable
I2S: i2s record disable
lontium_wait_sync_start:lontium traning time:13816 ms
LontiumChipReset:Lontium chip reset
LontiumConnect:lontium reset,  lontium_connect_time = 1




lontium_wait_sync_start:lontium traning time:12002 ms
LontiumChipReset:Lontium chip reset
LontiumConnect:lontium reset,  lontium_connect_time = 2


会出现声音卡断，频繁的声音。


stop nxrsensor执行后，就不会出现。

USBVSC_PRINTK: vsc req start
slamCommands_setMode:HIDCMD_MODECONTROL:FEPower_ctl 0
camera_fe_control:fe_format = 2UAC1DeviceI_StreamEvent:UAC Speaker enable
I2S: I2SPlay enable
UAC1DeviceI_StreamEvent:UAC Speaker disable
I2S: I2SPlay disable
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 2
UsbPumpVscAppI_Event:
音频质量有问题。

如果关闭nxrsensor就不变，

硬件反馈，是之前的时钟3.072M没有满足，导致的这个问题。

如果把SLAM定位关闭就好。也会没有。

目前看，跟频率没有关系，

IMU上传100HZ无法解决。

ENABLE_MOVIDIUS_UAC 使用movidius官方的UAC协议

lingwei也是改问题。

[CRTICAL_ERORR]: [uac_spk_thread] line:376 speaker data read fails count:0 res:0
[CRTICAL_ERORR]: [uac_spk_thread] line:376 speaker data read fails count:0 res:0
[CRTICAL_ERORR]: [uac_spk_thread] line:376 speaker data read fails count:0 res:0



slamCommands_setMode:HIDCMD_MODECONTROL:FEPower_ctl 0
audioControllerThread:audio headphone mode
USBVSC_PRINTK: vsc req start
slamCommands_setMode:HIDCMD_MODECONTROL:FEPower_ctl 0
camera_fe_control:fe_format = 2hid_switch_dis_led:hid_switch_dis_led on
hid_switch_dis_led:hid_switch_dis_led off
hid_switch_dis_led:hid_switch_dis_led on
UAC1DeviceI_StreamEvent:UAC Speaker disable
I2S: I2SPlay disable
hid_switch_dis_led:hid_switch_dis_led off
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 2
UsbPumpVscAppI_Event:[UsbPumpVscAppI_Event] Event: 3



（1）USB2.0的试下效果更差，只能剩下抓.
（2）需要抓USB事件。
ENABLE_MOVIDIUS_UAC
demokit_vsc
zhanghang_VSC Hong_BD_VSC
HMS_FE_VSC
JunkeV2_VSC



是目前向存储中拷贝的步骤：
1.读取48个字节
2.切换到U盘模式，格式化，拷贝数据，检查拷贝的文件大小是否正确
3.将新的数据写入到48个字节的存储中(没有切换到正常模式，在U盘模式下直接写入)
4.格式化U盘，偶现FLASH specific区域前面64K个随机值是。

read flash size : 8388608


flash_erase 擦写过后，00000000 ~ 007FFFF8 全是FF.

问题设备：
007F0010 ~ 007F001C = 01 00 00 00 00 00 00 00 / 00 00 00 34  12 CD AB / AD 94 E0 F7 00 00 08 00 / 94 ED 28 00 


0X7C000265
0X25700273

SPECFlash= 640K


02 fd 66 01 24 01 

出现改问题是随机的
有问题的FLASH log地址：0x00720000 ~ 0x007200C3  数据有差异。195g个数据。
错误设备：看起来像是随机值
正常设备：数据有规律。

查看FLASH代码，每个分区都不是固定地址，而是相对偏移。


升级失败
- 超时的原因，设备没有接收的升级消息。
- 升级完成没有端口，是因为设备升级失败，校验出错，导致后面重启没有执行，没有端口。
- 
升级工具githup
http://internal.xvisio.org/hostdev/utilities




terminated notify to download
NDfuDemoI_RamLoad_Manifest 



