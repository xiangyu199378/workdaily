Sunday ,December 24 ,2021  work log


三星S5P6818之UBOOT网络配置 解决网络问题
https://blog.csdn.net/weixin_43536180/article/details/106277382?spm=1001.2014.3001.5501

4.更改板子中的UBOOT

拷贝 fip-nonsecure.img 文件到开发板中,也可以用读卡器把TF卡插到电脑。。
在开发板中更新UBOOT命令:
dd if=fip-nonsecure.img of=/dev/mmcblk0 bs=512 seek=3841
sync

如果用读卡器更新 比如SD卡生成是 sdb 使用命令：
dd if=fip-nonsecure.img of=/dev/sdb bs=512 seek=3841
sync

```
#########################################	
【1】. bootloader 概念 
	boot:引导 
	loader:加载
	bootloader:引导加载linux内核启动
【2】bootloader和u-boot关系？
	bootloader是一系列引导启动程序的统称。
	
	u-boot属于bootloader中的一种。
	bootloader包括u-boot，bios，vivi，redboot....
	
	嵌入式开发中常用u-boot引导程序。
	u-boot是一个开源的软件。
【3】u-boot的特点？
	1》u-boot是一个开源的软件 
	2》u-boot支持多种架构,支持的架构指令
		ARM，MIPS，PowerPC，X86...
	3》u-boot短小精悍
	4》u-boot是一个裸机程序
	5》u-boot完成部分硬件的初始化
		串口，时钟，内存等等
	6》u-boot用于引导linux内核启动，
		并给linux内核传递参数
	7》u-boot是一个短命鬼，
		u-boot启动内核之后，
		u-boot的生命周期就结束。
【4】u-boot中常用的命令？
	前提：uboot启动之后，进入uboot的交互界面。
		FS6818 # 
		
	1. help 
		查看uboot支持的所有命令
		help uboot命令名  ： 查看帮助手册
	
	2. loadb 
		格式：loadb mem_addr    
		下载二进制文件到内存的mem_addr地址中，
		使用kermit协议
	
	3. go 
		格式：go mem_addr
		从内存的mem_addr启动应用程序。
	
	4. printenv/pri/print  : 打印uboot的环境变量
 
	baudrate=115200
	bootargs=root=/dev/nfs nfsroot=192.168.0.171:/home/hqyj/nfs/rootfs rw console=/ttySAC0,115200 init=/linuxrc ip=192.168.0.222
	bootcmd=tftp 48000000 uImage;bootm 48000000
	bootdelay=50
	gatewayip=192.168.0.1
	ipaddr=192.168.0.222
	netmask=255.255.255.0
	serverip=192.168.0.171
	stderr=serial
	stdin=serial
	stdout=serial
 
	baudrate	：波特率
	bootargs	：自启动参数 （后边详细讲解）
	bootcmd 	：自启动命令 （后边详细讲解）
	bootdelay	：启动倒计时时间
	gatewayip	：网关
	ipaddr		：开发板IP地址
	netmask		：子网掩码
	serverip	：ubuntu服务器的IP地址
 
	uboot中的命令可以部分匹配。
 
	5. 修改uboot中的环境变量(增删改查)
	setenv  - set environment variables
		设置环境变量。
		设置完环境变量默认保存在内存中，
		只对当前有效，重新上电将无效。
	
	saveenv - save environment variables to persistent storage
		保存环境变量。
		保存环境变量从内存到flash中
	
	1> 增加环境变量
		setenv  新的环境变量名  环境变量的值
		eg1:
		setenv board_name  FS6818 
		@ 变量名和值之间的等号自动填充
		saveenv
		
		eg2：
		setenv no zuo no die  666666
		变量名字之间不要出现空格
	2> 删除环境变量 
		setenv 要删除的环境变量名
		eg：
		setenv no 
		saveenv
	
	3> 修改环境变量
		setenv 要修改的环境变量名  变量值 
		eg: 
		setenv bootdelay 10
		saveenv 
		
		注意：切记不要删除bootdelay环境变量
			不要将bootdelay环境变量修改为0。
	6. 	tftpboot/tftp 命令 
		tftp命令使用tftp服务下载程序从ubuntu到开发板中
		格式：tftpboot [loadAddress] [bootfilename]
			下载bootfilename文件到内存的loadAddress地址中。
		
		bootfilename文件应该放在安装tftp服务时指定tftp服务的路径下。
		
		我们tftp服务的指定路径是  ~/tftpboot
		
	7. ping命令 
		ping开发板和ubuntu是否联网
		格式：ping  ubuntu服务器的IP地址
		
	8. md 命令 
		查看内存地址中的值
		格式：md  mem_addr
			回显一块连续内存空间中的值。
		
	9. nm 命令 
		修改内存地址中的值
		格式：nm  mem_addr
		eg:
		
		FS6818# nm 0xc001a004
		  地址     初始值      修改值
		c001a004: 00000000 ? 10000000   回车
		c001a004: 10000000 ? ctrl + c 
		
【5】tftp和ping命令的使用	
	【准备】：
		1》ubuntu需要安装tftp服务 
		2》关闭windows和ubuntu的防火墙
		3》修改电脑的有线网卡为百兆全双工
		控制面板--》网络和 Internet--》网络和共享中心--》
		更改适配器设置--》以太网(PCIE)右键属性--》
		网络窗口--》点击配置--》高级窗口--》Speed & Duplex
		--》修改为：100MBPs Full Duplex--》	确定
		
		对于没有有线网卡的电脑需要自备一个usb转网卡。
【配置ubuntu的网络和IP地址】
        ubuntu使用PCIE网卡或者USB转网卡，Ubuntu的IP地址设置为固定的
    参考图：本篇前三张图
 
               注：ubuntuIP地址的设置网段和windows的网段保持一致。
 
	【开发板uboot中IP地址的设置】
		设置开发板uboot中以下几个环境变量
		gatewayip	：网关
		ipaddr		：开发板IP地址
		netmask		：子网掩码
		serverip	：ubuntu服务器的IP地址
		
		setenv gatewayip 192.168.1.1
		setenv ipaddr    192.168.1.222
		setenv netmask   255.255.255.0
		setenv serverip  192.168.1.250
		saveenv
	【测试开发板和ubuntu系统是否可以ping通】
		开发板启动之后，在串口工具中输入ping ubuntu地址
 
		FS6818# ping 192.168.1.250
		
		打印以下信息，表示开发板屏ubuntu失败
		dwmac.c0060000 Waiting for PHY auto negotiation to complete..... done
		Speed: 100, full duplex
		Using dwmac.c0060000 device
		ping failed; host 192.168.1.250 is not alive
		
		可能原因：
		1. 检查ubuntu和windows防火墙是否关闭
		*2. 检查ubuntu的网络配置
		*3. 检查uboot的环境变量 ipaddr serverip设置是否正确
			serverip是否跟ubuntu ip地址一致
			开发板的ip地址是否和ubuntu在同一个网段
		*4. 检查网线是否有问题，
			连接问题--》是否插好
			网线是否坏了---》更换一根网线试试
		5. uboot不支持网卡驱动--》直接更换支持网卡驱动的uboot
		
		打印以下信息表示开发板可以ping通ubuntu
		Speed: 100, full duplex
		Using dwmac.c0060000 device
		host 192.168.1.250 is alive
 
	【使用tftp命令下载程序】
		1. 首先将.bin文件拷贝到~/tftpboot目录下
			学习ARM阶段的代码，拷贝一个.bin文件到tftpboot。
		2. tftp下载程序 
			FS6818# tftpboot 0x43c00000 interface.bin
			或者 
			FS6818# tftp 0x43c00000 interface.bin
			
			出现以下内容，表示下载成功：
			Speed: 100, full duplex
			Using dwmac.c0060000 device
			TFTP from server 192.168.1.250; our IP address is 192.168.1.222
			Filename 'interface.bin'.三星S5P6818之UBOOT网络配置
			done
			Bytes transferred = 9032 (2348 hex)
			FS6818#
			
			注意： 
			T  ： 超时（TIMEOUT）
			
			解决思路：
				*1. 重启tftp服务即可，
				sudo service tftpd-hpa restart 
				重启tftp服务的命令会经常使用到。
				2. 检查开发板是否可以ping通ubuntu。
				
		3. 开发板上运行程序 
			FS6818# go 0x43c00000
 
		
		hexdump  命令 ：可以查看二进制文件
```        