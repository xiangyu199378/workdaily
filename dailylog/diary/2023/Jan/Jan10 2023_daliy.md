# List
1. [任务]gerrit environment 
- nfs is setting read/write
- add user to confirm contral complie right .
2. DPVR-
- 休眠问题，说长时间进入不了休眠，应该是进入条件有点苛刻，
- 升级问题,目前发现升级过后，显示参数区，参数丢失。
- 出现DP状态没有正确上报，这个应该是HID读失败，或者不通导致的，状态线程不会存在
- 

音频EQ 5~9 号出了4版本

已经投入：共计16人天。
（1）[5人/天]调整HID接口，共计调整了两版，第一版本，由于DP助手发出离线版本，不能适用，又改了一版本兼容原来的HID接口，底层增加校验参数。同时更新文档。
（2）[2人/天]老线不支持120HZ,需要增加flash存储新老线标志，
（3）[5人/天]新增增加120hz功能后，引入120hz消失问题，不能正确获取到频率参数两个缺陷。修复缺陷+验证。
（3）[1人/天]新增120hz功能后，龙讯需要诠视更改频率切换方式。有GPIO配置改成IIC设置寄存器。
（4）[2.5人/天]配合龙讯更新固件，验证新增120hz引入的花屏，闪烁等问题。
剩余：预计5人/天
（1）[5人/天]新增增加120hz功能后,频率参数相关参数设置获取稳定性验证和缺陷解决。
 (2)其他，可能要配合龙讯不断更新固件。

 总结：该功能评估占时 21个工作日，工作量为20.5人/天。。

2022年12月1日~2023年1月10日 共计4人/天
第一周 0 
第二周 |大鹏| 中 |100%|DP闪烁，输出固件|解决|0.5Ｄ|
第三周 |DP:120hz频率选项切换缺失| 中 |10%|代码走读|解决中|1Ｄ|
第四周 |DP:flash参数数据异常| 中 |10%|flash存在获取一个错误的值，其他问题配和解决|解决中|0.5Ｄ|
第五周 |DP:flash参数数据异常| 中 |50%|显示想关参数没有做读写保护，增加读写保护|提供程序包验证|0.5Ｄ|
第六周 |DP:配合输出固件| 中 |50%|音频调试优化，120hz闪烁问题|提供程序包|1.5Ｄ|

    [2023-01-10 14:23:19.377][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 14:54:40.224][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:21:05.334][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:27:48.355][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:28:36.301][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:31:37.699][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:36:44.417][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 15:41:40.216][info] New USB device registered (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
    [2023-01-10 14:43:47.762][trac] HID write: 0x02dead01
    [2023-01-10 15:22:19.557][trac] HID write: 0x02dead01
    [2023-01-10 15:29:37.859][trac] HID write: 0x02dead01
    [2023-01-10 15:32:28.393][trac] HID write: 0x02dead01
    [2023-01-10 15:36:47.152][trac] HID write: 0x02dead01
    [2023-01-10 15:41:49.947][trac] HID write: 0x02dead01

[2023-01-10 14:23:20.500][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 14:23:20.500][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 14:34:53.197][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 14:34:53.197][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 14:43:48.064][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 14:43:48.064][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 14:54:41.354][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 14:54:41.354][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 15:21:07.470][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 15:21:07.470][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 15:28:37.426][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 15:28:37.426][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 15:31:38.820][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 15:31:38.820][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 15:36:45.539][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 15:36:45.539][debu] Unplugged device SN: XVISIO123456789
[2023-01-10 15:41:44.365][info] USB device unplugged (SN: "XVISIO123456789" manufacturer: "XVisio Technology" product: "XVisio vSLAM")
[2023-01-10 15:41:44.365][debu] Unplugged device SN: XVISIO123456789



（1）切换CD-ROOM后出现问题，助手读取不到设备。
（2）插拔USB接口，h助手读取不到设备。
（3）重启眼镜hid也不通。


（1）目前存在flash参数丢失的问题，偶现升级一次，升级起来用户显示参数区全是0，其他数据区也是空。
    仿真升级起来也遇到过一次。仿真的那次没有丢，

    需要检查标定参数区是否也丢失了。
    考虑把整个扇区都读出了。看看是否有问题。
    增加锁。

（2）目前遇到HID不稳定，老是报读不到数据。怎么确定不稳定是什么导致的。
-  CD-ROOM 模式会重启设备，如果在这个时候，获取相关参数，会有因为设备重启而导致获取失败，


（3）nfs用户，只有nfs用户能写入/out目录，


(1)直接把USB端口，隐射到虚拟机，
(2)footboot 网络烧写
(3)
