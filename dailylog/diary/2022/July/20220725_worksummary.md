

# 上周投入
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
|大棚:呼吸灯ioctl函数返回错误值| 中 |50%  |呼吸灯芯片复位会导致ioctl函数异常| 解决中|0.5Ｄ|
|大棚:USB没有端口| 中 |50%  |USB数据包异常| 解决中|2Ｄ|
|大棚:USB枚举成U2| 中 |50%  |增加重新枚举逻辑|输出测试包|1Ｄ|
|固件组手册项目| 中 | 5%  |建立项目| 完成|0.5D|
|RH：display| 中 | 5%  |任务分解| 完成|1D|




# 本周计划
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
|大棚:问题解决| 中 |50%  |ioctl失败问题，USB没有端口，MIC识别不出来| 解决中|1Ｄ|
|RH：地磁| 中 | 10%  |业务和模块梳理| 投入|3D|
|RH：display| 中 | 5%  |任务分解| 投入|3D|
|JunkeV2| 中 | 10%  |fisheye帧间隔问题imu上报数据问题| 提交代码|1.5D|

# 挂起任务
| 任务| 优先级 | 进度 | 问题| 状态|计划时间 |
|-----|-------| ---- | ---|----|--------|
| oppo项目 | 低 | ５%  | 7911uxc代码合入ｂｒａｎｃｈ分支| 待解决| ２Ｄ  |
| 框架优化 | 低 | 15%  | 输出一版细化，再次会议讨论结构体设计 | 待解决 | １０D|
| RK平台开发| 中 | ６%  |内核裁剪与裁剪| 解决中| ２Ｄ|
| BUG：DP信号问题 | 中| ７０%  | EPSON２.0,3.0判断条件判断条件存在问题，导致逻辑结果存在误判,后续需要需要验证下对版本号是否对ＤＰ是否有影响|解决中 |２D|
|JunkeV2| 中 | 10%  |E34R静电检测,HID单步测试，每个状态的复位问题| 提交代码|1D|


# 任务说明：
## 【缺陷】
【缺陷】<font color='red'> 【中优先级】  </font>E34R静电检测
- 结论：
    -　优化后最短时间１最后有３％的概率 无法点亮为ＥＰＳＯＮ光机无法点亮．
    -  新的机器DVT2设备，ESD功能失效，
- 计划：    
    - 单步ＨＩＤ命令，当出现问题后，单步查看每个步骤．
    - 目前让测试人员收集日志。



## 【缺陷解决】
1. <font color='red'> 【高优先级】  </font>大棚产线
 
- [问题1]呼吸灯 ioctl函数偶现会调用失败问题。
   - ioctl函数返回值是一个错误的值。
   - 返回值没有置位的问题，

- [问题2]USB识别不出端口问题
   - 当前通过USB分析仪器，显示数据包全是未知的数据包。
   - 未定位到具体原因 
   - main函数，判断USB枚举成功与否，1.5s.超时等待
   - JTAG 看下能否他出现。在Jtag仿真运行的情况下，始终是U3并且能稳定识别出来。
   - 就需要看一下，仿真运行下和插拔运行下，有什么不一样。
   - 仿真运行下，USB没有断过电源，是不是插拔的过程会导致USB信号不稳定。
   - 目前这个问题，在HUB的情况下可以复现，USB线材那边反应，可以先不看hub
   - 如果只升级USBloader，能识别出来端口，但是升级。
   - 这个问题，只是在HUB情况下，可以出现。目前待验证。

- [问题3]MIC识别不出来
   - 目前没有复现，目前让DPVR帮忙抓取日志
   - 催下DP提供下日志。

- [问题4]USB2枚举成U3问题
  - 在DP断开的情况下，始终是U3. 重新枚举。
  -
 
1. <font color='red'> 【中优先级】  </font>RH:地磁的问题，
- 进展：
- imu业务。了接项目的背景。
- 
3. <font color='red'> 【中优先级】  </font>RH:显示模块
- 进展：
- 分解和设计模块。


4. <font color='red'> 【中优先级】  </font>fisheye 间隔问题，imu
- 进展：裸板无法写入校验参数，无法调试，还需投入。

5. <font color='red'> 【中优先级】  </font> junke no imu 版本，SDK发现错误的HID命令，导致imu会一直上报数据，
- 进展：待验证




## 【组织建设】
1.已经建设一个Gitlab库，
2.已经建立内部小组wiki

* [https://www.mediawiki.org/wiki/Special:MyLanguage/Manual:Configuration_settings MediaWiki配置设置列表]
* [https://www.mediawiki.org/wiki/Special:MyLanguage/Manual:FAQ/zh-hans MediaWiki常见问题]
* [https://lists.wikimedia.org/postorius/lists/mediawiki-announce.lists.wikimedia.org/ MediaWiki发布邮件列表]
* [https://www.mediawiki.org/wiki/Special:MyLanguage/Localisation#Translation_resources 本地化MediaWiki到您的语言]
* [https://www.mediawiki.org/wiki/Special:MyLanguage/Manual:Combating_spam 了解如何在您的wiki上打击破坏]


wiki项目部署服务器

192.168.20.22 root用户，密码是ubuntu ,个人用户xvisio ,密码Ab1234
地址：
http://192.168.20.22/mediawiki/index.php/

张工9211驱动拆分：
1. 当前的MIPI来源，外部接口获取当前MIPI输入通道。A通道，B通道
2. 9211 上电时序
3. 设置当前视频分辨率
4. 根据通道，初始对应的通道，设置为mipi dsi输入
5. 设置为LVDS 1 个port输出。



