# E34R glass had nose when audio paly （E34 在RK平台，有噪音问题）

- 【问题原因】E34在RK平台上会同时注册SPDIF声道和USB audio,实际上眼镜只使用USB audio， RK会同时输出，这个时候会存在破音。

- 【解决办法】把SPDIF通道先注释掉。
- 【维护】单独维护一个pitch。
  
【问题解决记录】 当前该问题由于当时没有记录，现在需要重新问题记录下来。


frameworks/av/services/audiopolicy/enginedefault/src
不要直接替换，你只关注getDevicesForStrategyInt函数，昨天我截图部分
我不清楚你们现在的版本和sdk最新的有啥差别，最好不要直接替换，你compare下，将我昨天截图部分, case STRATEGY_MEDIA:中的devices2.add(devices3);注释掉

代码修改后，在frameworks/av/services/audiopolicy/enginedefault目录下mm编译，将生成的libaudiopolicyenginedefault.so push到机器的/system/lib 和lib64下


![1](WeChat%20Image_20221110162637.png)

