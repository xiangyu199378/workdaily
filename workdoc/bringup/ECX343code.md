
代码流程简要介绍
<一>
1. Framework/leon/main.c
-  POSIX_Init -> DisplayControllerInit()
2. framework\...\DisplayController.c
-  DisplayControllerInit() -> DisplayControllerThread-> DisplayControllerMain() 
-  
3. cougar_mdk\...\LT7211b.C
- DisplayControllerMain()  create a thread to contral dispaly(SLEEP,WAKUP)

<二>
1. osBoardInitSPI -> osBoardInitDisplay -> DisplayBringupTask_Creak() -> DisplayBringupTask
- DisplayBringupTask ->  DisplayUpdate -> lt7211b_update_fw（update 7211b hex）
- DisplayBringupTask ->  display_init -> display_first_open（open ecx343 display）


POSIX_Init -> osBoardInitialise-> osBoardInitSpi 这条路实现7211初始化和ecx335开屏操作。我需要重点梳理这里的代码。




总结流程：
- （1）第一步，mopen ，打开OSLT7211B_NODE /dev/i2c.lt7211b
- (2)检测更新固件
  - 获取版本号。LT7211B_VERSION,
  - 对比获取的版本号和我们要的版本号是否一致。
  - 不一致，发送LT7211B_UPDATA 命令，接收端lt7211b_Contral执行该命令。实现固件更新。
  问题：这个版本号从哪里获取。
- (3)display_init 函数实现EXC开屏函数。
   - display_first_open 实现第一次开屏操作。
   - display_dp_panel_status_report 报告DP状态。
  


