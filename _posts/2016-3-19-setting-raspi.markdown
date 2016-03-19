---
layout: post
title:  "开始使用树莓派，开机启动及基础配置"
subtitle: "陆宽"
date:   2016-03-18 23:34:01
categories: [Embedded, Raspberry]
---
# 目的

1. 了解嵌入式板卡一般情况；
2. 熟悉pcDuino的供电等接线方式；
3. 复习Linux启动过程（操作系统课）；
4. 复习通过Linux获得硬件数据（操作系统课）
5. 熟练掌握串口在PC上的使用；
6. 熟练掌握Linux的以太网和WiFi配置；
7. 熟练掌握Linux的SSH配置；
8. 熟练掌握PC上的SSH软件。
9. 掌握嵌入式板卡和PC建立文件共享的方式；
10. 寻找和安装交叉编译环境，理解交叉编译；
11. 熟悉嵌入式板卡的Linux下的编程环境；
12. 了解远程访问嵌入式板卡图形桌面的方式。

---

# 器材

__硬件__

|序号|名称|数量|
|--|:--:|--|
|1|RaspberryPi|1|
|2|microUSB线|1|
|3|USB-TTL串口线一根（PL2303芯片）|1|
|4|Windows PC|1|
|5|Mac PC|1|
|6|以太网线|1|
|7|HDMI显示器|1|
|8|HDMI线|1|
|9|USB键盘|1|
|10|USB鼠标|1|  

__软件__

|序号|名称|
|:--|:---:|
|1|[PL2303驱动 for Win/Mac](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=225&pcid=41)|
|2|minicom for Mac|
|3|[Putty for Windows](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)|
|4|交叉编译软件|

---

# 步骤

1. 在PC上安装好USB串口驱动和串口终端软件；
	- [下载PL2303驱动软件](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41)
	- 安装minicom 
	<pre><code> $ brew install minicom </code></pre>
	安装完成重启后，输入
	<pre><code> $ ls /dev/tty.us* </code></pre>
	如果显示`/dev/tty.usbserial`说明驱动安装完成
	- 配置minicom 在终端下输入如下命令
	<pre><code> $ minicom -s </code></pre>
	- 在minicom中设置Serial port为上图
	![](/Users/kuanlu/Desktop/嵌入式系统/snap1.png)
2. 按照图纸要求，将USB串口线与树莓派连接好，并连接好以太网（如果打算采用WiFi，可不连接以太 网）。如有 条件，接上HDMI线和HDMI显示器；
	- __连接示意图__
	![](/Users/kuanlu/Desktop/嵌入式系统/连接线.png)
  	- __实际连接照片1__
	![](/Users/kuanlu/Desktop/嵌入式系统/FullSizeRender.jpg)
  	- __实际连接照片2__
	![](/Users/kuanlu/Desktop/嵌入式系统/FullSizeRender 2.jpg)
  	- __HDMI显示__
	![](/Users/kuanlu/Desktop/嵌入式系统/FullSizeRender 3.jpg)
3. 给pcDuino上电，记录启动过程的输出以及解释，解释见每一行后面的注释；
<pre></code>Uncompressing Linux... done, booting the kernel.
[    0.000000] Booting Linux on physical CPU 0x0     //Linux从物理地址0开始执行
[    0.000000] Initializing cgroup subsys cpu        //启动cpu
[    0.000000] Initializing cgroup subsys cpuacct
[    0.000000] Linux version 3.18.7+ (dc4@dc4-XPS13-9333) (gcc version 4.8.3 20140303 (prerelease) (crosstool-NG linaro-1.13.1+bzr2650 - Linaro GCC 2014.03) ) #755 PREEMPT Thu Feb 12 17:14:31 GMT 2015                                //Linux内核版本是3.18.7 gcc版本是4.8.3
[    0.000000] CPU: ARMv6-compatible processor [410fb767] revision 7 (ARMv7), cr=00c5387d  //CPU是 ARMv6架构的
[    0.000000] CPU: PIPT / VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
[    0.000000] Machine model: Raspberry Pi Model B   //机器型号是树莓派Model B
[    0.000000] cma: Reserved 8 MiB at 0x1b800000     
[    0.000000] Memory policy: Data cache writeback   //内存cache的策略，写回
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 113792
[    0.000000] Kernel command line: dma.dmachans=0x7f35 bcm2708_fb.fbwidth=656 bcm2708_fb.fbheight=416 bcm2708.boardrev=0xf bcm2708.serial=0x21630876 smsc95xx.macaddr=B8:27:EB:63:08:76 bcm2708_fb.fbswap=1 sdhci-bcm2708.emmc_clock_freq=250000000 vc_mem.mem_base=0x1ec00000 vc_mem.mem_size=0x20000000  dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait
[    0.000000] PID hash table entries: 2048 (order: 1, 8192 bytes)   //进程哈希表的起始地址
[    0.000000] Dentry cache hash table entries: 65536 (order: 6, 262144 bytes) //目录项的哈希表的起始地址
[    0.000000] Inode-cache hash table entries: 32768 (order: 5, 131072 bytes)   //索引节点的哈希表的起始地址
[    0.000000] Memory: 437208K/458752K available (5926K kernel code, 358K rwdata, 1876K rodata, 340K init, 734K bss, 21544K reserved)    //内存大小437MB
[    0.000000] Virtual kernel memory layout:    //虚拟内存的地址映射
[    0.000000]     vector  : 0xffff0000 - 0xffff1000   (   4 kB)
[    0.000000]     fixmap  : 0xffc00000 - 0xffe00000   (2048 kB)
[    0.000000]     vmalloc : 0xdc800000 - 0xff000000   ( 552 MB)
[    0.000000]     lowmem  : 0xc0000000 - 0xdc000000   ( 448 MB)
[    0.000000]     modules : 0xbf000000 - 0xc0000000   (  16 MB)
[    0.000000]       .text : 0xc0008000 - 0xc07a6ad8   (7803 kB)
[    0.000000]       .init : 0xc07a7000 - 0xc07fc000   ( 340 kB)
[    0.000000]       .data : 0xc07fc000 - 0xc085588c   ( 359 kB)
[    0.000000]        .bss : 0xc085588c - 0xc090d128   ( 735 kB)
[    0.000000] SLUB: HWalign=32, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] Preemptible hierarchical RCU implementation.
[    0.000000] NR_IRQS:522
[    0.000023] sched_clock: 32 bits at 1000kHz, resolution 1000ns, wraps every 2147483648000ns
[    0.000073] Switching to timer-based delay loop, resolution 1000ns
[    0.000355] Console: colour dummy device 80x30
[    0.001414] console [tty1] enabled
[    0.001460] Calibrating delay loop (skipped), value calculated using timer frequency.. 2.00 BogoMIPS (lpj=10000)
[    0.001534] pid_max: default: 32768 minimum: 301
[    0.001909] Mount-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.001973] Mountpoint-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.002982] Initializing cgroup subsys memory   //初始化其他子系统
[    0.003075] Initializing cgroup subsys devices  
[    0.003135] Initializing cgroup subsys freezer
[    0.003190] Initializing cgroup subsys net_cls
[    0.003241] Initializing cgroup subsys blkio
[    0.003365] CPU: Testing write buffer coherency: ok
[    0.003480] ftrace: allocating 19479 entries in 58 pages
[    0.111547] Setting up static identity map for 0x55d058 - 0x55d0b4
[    0.114374] devtmpfs: initialized
[    0.131831] VFP support v0.3: implementor 41 architecture 1 part 20 variant b rev 5
[    0.135030] pinctrl core: initialized pinctrl subsystem
[    0.137696] NET: Registered protocol family 16
[    0.143219] DMA: preallocated 4096 KiB pool for atomic coherent allocations
[    0.171321] cpuidle: using governor ladder
[    0.201379] cpuidle: using governor menu
[    0.201886] bcm2708.uart_clock = 3000000
[    0.204988] No ATAGs?
[    0.205053] hw-breakpoint: found 6 breakpoint and 1 watchpoint registers.
[    0.205116] hw-breakpoint: maximum watchpoint size is 4 bytes.
[    0.205185] mailbox: Broadcom VideoCore Mailbox driver
[    0.205349] bcm2708_vcio: mailbox at f200b880
[    0.205825] bcm_power: Broadcom power driver
[    0.205880] bcm_power_open() -> 0
[    0.205909] bcm_power_request(0, 8)
[    0.706651] bcm_mailbox_read -> 00000080, 0
[    0.706699] bcm_power_request -> 0
[    0.706899] Serial: AMBA PL011 UART driver   //启动串口
[    0.707134] dev:f1: ttyAMA0 at MMIO 0x20201000 (irq = 83, base_baud = 0) is a PL011 rev3
[    1.096559] console [ttyAMA0] enabled
[    1.169621] SCSI subsystem initialized
[    1.173759] usbcore: registered new interface driver usbfs
[    1.179554] usbcore: registered new interface driver hub
[    1.185071] usbcore: registered new device driver usb
[    1.192254] Switched to clocksource stc
[    1.226652] FS-Cache: Loaded
[    1.229971] CacheFiles: Loaded
[    1.250189] NET: Registered protocol family 2    //配置网络
[    1.256144] TCP established hash table entries: 4096 (order: 2, 16384 bytes)
[    1.263561] TCP bind hash table entries: 4096 (order: 2, 16384 bytes)
[    1.270122] TCP: Hash tables configured (established 4096 bind 4096)
[    1.276631] TCP: reno registered
[    1.279899] UDP hash table entries: 256 (order: 0, 4096 bytes)
[    1.285825] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[    1.292565] NET: Registered protocol family 1
[    1.297570] RPC: Registered named UNIX socket transport module.
[    1.303656] RPC: Registered udp transport module.
[    1.308387] RPC: Registered tcp transport module.
[    1.313152] RPC: Registered tcp NFSv4.1 backchannel transport module.
[    1.320822] bcm2708_dma: DMA manager at f2007000
[    1.325853] vc-mem: phys_addr:0x00000000 mem_base=0x1ec00000 mem_size:0x20000000(512 MiB)
[    1.335729] futex hash table entries: 256 (order: -1, 3072 bytes)
[    1.341997] audit: initializing netlink subsys (disabled)
[    1.347721] audit: type=2000 audit(1.100:1): initialized
[    1.368611] VFS: Disk quotas dquot_6.5.2
[    1.373119] Dquot-cache hash table entries: 1024 (order 0, 4096 bytes)
[    1.382613] FS-Cache: Netfs 'nfs' registered for caching
[    1.389718] NFS: Registering the id_resolver key type
[    1.395031] Key type id_resolver registered
[    1.399246] Key type id_legacy registered
[    1.404793] msgmni has been set to 869
[    1.411082] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    1.419096] io scheduler noop registered
[    1.423247] io scheduler deadline registered (default)
[    1.428807] io scheduler cfq registered
[    1.435383] BCM2708FB: allocated DMA memory 5bc00000
[    1.440450] BCM2708FB: allocated DMA channel 0 @ f2007000
[    1.451904] Console: switching to colour frame buffer device 82x26
[    1.464133] bcm2708-dmaengine bcm2708-dmaengine: Load BCM2835 DMA engine driver
[    1.473771] uart-pl011 dev:f1: no DMA platform data
[    1.481172] vc-cma: Videocore CMA driver
[    1.486922] vc-cma: vc_cma_base      = 0x00000000
[    1.493338] vc-cma: vc_cma_size      = 0x00000000 (0 MiB)
[    1.500318] vc-cma: vc_cma_initial   = 0x00000000 (0 MiB)
[    1.520910] brd: module loaded
[    1.532957] loop: module loaded
[    1.538132] vchiq: vchiq_init_state: slot_zero = 0xdb800000, is_master = 0
[    1.547775] Loading iSCSI transport class v2.0-870.
[    1.556054] usbcore: registered new interface driver smsc95xx
[    1.563710] dwc_otg: version 3.00a 10-AUG-2012 (platform bus)
[    1.771309] Core Release: 2.80a
[    1.776147] Setting default values for core params
[    1.782518] Finished setting default values for core params
[    1.989786] Using Buffer DMA mode
[    1.994686] Periodic Transfer Interrupt Enhancement - disabled
[    2.002083] Multiprocessor Interrupt Enhancement - disabled
[    2.009261] OTG VER PARAM: 0, OTG VER FLAG: 0
[    2.015248] Dedicated Tx FIFOs mode
[    2.020757] WARN::dwc_otg_hcd_init:1047: FIQ DMA bounce buffers: virt = 0xdbc14000 dma = 0x5bc14000 len=9024
[    2.033818] FIQ FSM acceleration enabled for :
[    2.033818] Non-periodic Split Transactions
[    2.033818] Periodic Split Transactions
[    2.033818] High-Speed Isochronous Endpoints
[    2.056969] WARN::hcd_init_fiq:412: FIQ on core 0 at 0xc040116c
[    2.064561] WARN::hcd_init_fiq:413: FIQ ASM at 0xc0401444 length 36
[    2.072496] WARN::hcd_init_fiq:438: MPHI regs_base at 0xdc806000
[    2.080202] dwc_otg bcm2708_usb: DWC OTG Controller
[    2.086801] dwc_otg bcm2708_usb: new USB bus registered, assigned bus number 1
[    2.095792] dwc_otg bcm2708_usb: irq 32, io mem 0x00000000
[    2.102977] Init: Port Power? op_state=1
[    2.108485] Init: Power Port (0)        //配置usb设备
[    2.113707] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002
[    2.122161] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    2.131051] usb usb1: Product: DWC OTG Controller
[    2.137403] usb usb1: Manufacturer: Linux 3.18.7+ dwc_otg_hcd
[    2.144786] usb usb1: SerialNumber: bcm2708_usb
[    2.152043] hub 1-0:1.0: USB hub found 
[    2.157655] hub 1-0:1.0: 1 port detected
[    2.164554] usbcore: registered new interface driver usb-storage
[    2.172787] mousedev: PS/2 mouse device common for all mice
[    2.180804] bcm2835-cpufreq: min=700000 max=700000
[    2.187706] sdhci: Secure Digital Host Controller Interface driver
[    2.195619] sdhci: Copyright(c) Pierre Ossman
[    2.201848] DMA channels allocated for the MMC driver    //为片上存储开启DMA
[    2.242336] Load BCM2835 MMC driver
[    2.249407] sdhci-pltfm: SDHCI platform and OF driver helper
[    2.265253] ledtrig-cpu: registered to indicate activity on CPUs
[    2.273355] hidraw: raw HID events driver (C) Jiri Kosina
[    2.285079] usbcore: registered new interface driver usbhid
[    2.293478] usbhid: USB HID core driver
[    2.301518] TCP: cubic registered
[    2.307738] Initializing XFRM netlink socket
[    2.315889] NET: Registered protocol family 17
[    2.322221] Key type dns_resolver registered
[    2.330013] registered taskstats version 1
[    2.336079] vc-sm: Videocore shared memory driver
[    2.342540] [vc_sm_connected_init]: start
[    2.349386] [vc_sm_connected_init]: end - returning 0
[    2.358046] Waiting for root device /dev/mmcblk0p2...
[    2.365179] Indeed it is in host mode hprt0 = 00021501
[    2.376596] mmc0: host does not support reading read-only switch, assuming write-enable
[    2.404429] mmc0: new high speed SDHC card at address b368
[    2.422369] mmcblk0: mmc0:b368 SDC   7.51 GiB    //SD卡大小
[    2.433879]  mmcblk0: p1 p2
[    2.499426] EXT4-fs (mmcblk0p2): mounted filesystem with ordered data mode. Opts: (null)  //文件系统为EXT-4
[    2.511082] VFS: Mounted root (ext4 filesystem) readonly on device 179:2.
[    2.521520] devtmpfs: mounted
[    2.527433] Freeing unused kernel memory: 340K (c07a7000 - c07fc000)
[    2.572511] usb 1-1: new high-speed USB device number 2 using dwc_otg
[    2.582414] Indeed it is in host mode hprt0 = 00001101
[    2.792946] usb 1-1: New USB device found, idVendor=0424, idProduct=9512
[    2.801616] usb 1-1: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[    2.814282] hub 1-1:1.0: USB hub found
[    2.821181] hub 1-1:1.0: 3 ports detected
[    3.102638] usb 1-1.1: new high-speed USB device number 3 using dwc_otg
[    3.223015] usb 1-1.1: New USB device found, idVendor=0424, idProduct=ec00
[    3.231967] usb 1-1.1: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[    3.263821] smsc95xx v1.0.4
[    3.329763] smsc95xx 1-1.1:1.0 eth0: register 'smsc95xx' at usb-bcm2708_usb-1.1, smsc95xx USB 2.0 Ethernet, b8:27:eb:63:08:76
[    3.442781] usb 1-1.3: new high-speed USB device number 4 using dwc_otg
[    3.574444] usb 1-1.3: New USB device found, idVendor=0bda, idProduct=8176
[    3.585395] usb 1-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    3.596235] usb 1-1.3: Product: 802.11n WLAN Adapter
[    3.610533] usb 1-1.3: Manufacturer: Realtek
[    3.619688] usb 1-1.3: SerialNumber: 00e04c000001
[    4.296382] udevd[159]: starting version 175
[    7.427900] random: nonblocking pool is initialized
[    7.898316] usbcore: registered new interface driver rtl8192cu
[   10.788988] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)
[   11.267126] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)
Raspbian GNU/Linux 7 raspberrypi ttyAMA0   //终端信息，通过串口登录的
raspberrypi login:
</code></pre>

__树莓派基本信息__  
CPU信息通过`lscpu`命令获得  
![](/Users/kuanlu/Desktop/嵌入式系统/snap1001.png)  
或者通过`cat /proc/cpuinfo`  
![](/Users/kuanlu/Desktop/嵌入式系统/snap1002.png)  
内存信息通过`free -h`获得（以MB为单位）  
![](/Users/kuanlu/Desktop/嵌入式系统/snap1003.png)  
或者以`cat /proc/meminfo`获得  
![](/Users/kuanlu/Desktop/嵌入式系统/snap1004.png)  
磁盘信息以`lsblk`获得  
![](/Users/kuanlu/Desktop/嵌入式系统/snap1005.png)
	 
|硬件|参数|
|--|--|
|CPU型号|ARMv6 compatible|
|时钟频率|700MHZ|
|内存|437MB|
|磁盘|7.5GB|

4. 从终端登陆树莓派的Linux  
用户名：pr 密码：raspberry
![](/Users/kuanlu/Desktop/嵌入式系统/snap2.png)
5. 配置网络和/或WiFi，从pcDuino和PC两端证明网络已连接  
	- 采用网线连接Windows PC与树莓派的方式，首先配置Windows的网络设置  
		- 打开网络连接，右键WLAN属性，勾选如下，将家庭网络连接设为以太网
		![](/Users/kuanlu/Desktop/嵌入式系统/snap4.png)
		- cmd下通过`ipconfig`查看本机IP地址
		![](/Users/kuanlu/Desktop/嵌入式系统/snap3.png)
	- 将网线两端分别连接PC和树莓派
	- 配置树莓派下的网络设置  
		- 终端下修改/etc/network/interfaces如下
		<pre><code>auto lo iface lo inet loopback 
		iface eth0 inet static
		\#静态IP地址	
		address 192.168.137.2 
		\#子网掩码
		netmask 255.255.255.0
		\#网关地址，即PC机地址
		gateway 192.168.137.1
		allow-hotplug wlan0		
		iface wlan0 inet manual
		wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf	
		iface default inet dhcp</code></pre>
	- 重启树莓派
	- 验证
		- 树莓派ping PC的IP地址（注意要关掉PC端的防火墙）
		![](/Users/kuanlu/Desktop/嵌入式系统/snap5.png)
		- PC ping 树莓派的IP地址  
		![](/Users/kuanlu/Desktop/嵌入式系统/snap8.png)  
		配置树莓派的SSH，可尝试采用各种不同的认证方式；
6. 从PC通过SSH登陆pcDuino；
	- PC端打开Putty，设置如下,HostName是主机地址也就是树莓派的地址，端口默认22
		![](/Users/kuanlu/Desktop/嵌入式系统/snap6.png)  
	- 登录输入用户名：pi，密码：raspberry
		![](/Users/kuanlu/Desktop/嵌入式系统/snap7.png)  
7. 看到多个不同端口的登陆（本机键盘/屏幕、串口和SSH)
	- 在任何一个终端输入`w`命令显示如下
	<pre><code>pi@raspberrypi:~$ w
 13:55:42 up 35 min,  3 users,  load average: 0.00, 0.03, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
pi       ttyAMA0                   13:32    1.00s  2.28s  0.05s w
pi       pts/0    192.168.137.1    13:44   10:46   1.31s  1.31s -bash
pi       pts/1    :0.0             13:44   11:25   0.67s  0.67s /bin/bash</pre></code>
	- 并运用Linux的write来互相通信  
		- 使用串口登录的终端与ssh登录的PC和raspberry自带终端通信
		![](/Users/kuanlu/Desktop/嵌入式系统/snap9.png)
		- 使用ssh登录的终端与串口登录的终端和raspberry自带终端通信
		![](/Users/kuanlu/Desktop/嵌入式系统/snap10.png)
		- 使用rasberry自带终端和ssh登录终端和串口登录终端通信
8. 配置嵌入式板卡上的SAMBA客户端，使它能访问PC上共享的目录
	- raspberry 上安装samba
	<pre><code>sudo apt-get install samba 
	sudo apt-get install samba-common-bin</code></pre>
	- 配置samba 安装完成后，我们在/ect/samba/文件夹中找到这个文件smb.conf，先将smb.conf重命名为smb.conf.backup。然后用下面的smb.conf替换原来的smb.conf，保存完毕后输入命令：
sudo /etc/init.d/samba retsart
	<pre><code>#声明全局参数
	[global]
	\#设置Samba Server日志文件的存储位置以及日志文件名称。在文件名后加个宏%m（主机名)表示对每台访问Samba Server的机器都单独记录一个日志文件。如果pc1、pc2访问过Samba Server，就会在/var/log/samba目录下留下log.pc1和log.pc2两个日志文件。
	log file = /var/log/samba/log.%m
\#共享参数，共享名为tmp
[tmp]
        \#共享文件夹的描述
        comment = Temporary file space
        \#共享文件夹在树莓派下的路径
        path = /tmp
        \#是否只读
        read only = no
        \#是否公共文件夹
        public = yes</code></pre>
        
   - 添加用户
   输入命令：`sudo useradd lucas`这时系统就新建了一个名为lucas的用户，在/etc/samba/文件夹下建立smbpasswd文件，`sudo touch /etc/samba/smbpasswd`再给samba添加用户名为lucas的用户：`sudo smbpasswd -a lucas` 输入密码的，设完了显示：Added user lucas
   - 使用samba
 	在Windows下的网络目录中可以看到名为RASPBERRYPI的计算机，其中的tmp文件夹就是共享文件夹
 	![](/Users/kuanlu/Desktop/嵌入式系统/snap12.png)
 	将文件拖入该文件夹，在raspberry端就会在设定的目录下出现，下图中可以看到由添加的用户lucas传输到文件夹中的pcDuino.txt  
 	![](/Users/kuanlu/Desktop/嵌入式系统/snap11.png)
12. 通过sftp传递；
	- 下载FileZilla
	<pre><code>sudo apt-get install filezilla</code></pre>
	- 进入文件，站点管理器，新建站点，ip地址为树莓派自己的IP地址，登录选项为normal输入用户名pi和密码raspberry  
	![](/Users/kuanlu/Desktop/嵌入式系统/snap002.png)
	- 直接将文件拖动到服务器端的目录中即可实现文件共享
	![](/Users/kuanlu/Desktop/嵌入式系统/snap003.png)
13. 通过串口XModem协议传递；
	- 在PC机和树莓派上安装配置minicom  
		- 树莓派上用`sudo apt-get install`安装minicom， 首先配置PC机上的minicom，设置上传，下载文件夹
	![](/Users/kuanlu/Desktop/嵌入式系统/snap14.png)  
		- 然后配置树莓派上的minicom, 设置上传下载文件夹  
	![](/Users/kuanlu/Desktop/嵌入式系统/snap16.png)  
		- 设置串口（改为树莓派的串口ttyAMA0）
	![](/Users/kuanlu/Desktop/嵌入式系统/snap15.png)
	- PC->树莓派传输
		- 打开PC和树莓派上的minicom
		- 在PC端打开minicom，按下Meta-Z进入选项界面，选择S后选择xmodem，会出来文件选择界面，选择想要传输的文件并选择okay
		![](/Users/kuanlu/Desktop/嵌入式系统/snap17.png)
		- 在树莓派端打开minicom，Meta-Z进入选项界面后选择R，输入待接收文件名
		![](/Users/kuanlu/Desktop/嵌入式系统/xmodem.png)
		- 传输结果
		![](/Users/kuanlu/Desktop/嵌入式系统/xmodem1.png)
	- 树莓派->PC
		- 调换PC端和树莓派端的收发模式和顺序即可 
14. 三种文件传递方式对比（82M的文件测试）

	|传输方式|速率|安全性|易用性|
	|---|---|---|---|
	|samba|7.98MB/s|相对于sftp低，无加密|直接在网络文件夹中即可找到，易用性高|
	|sftp|2.55MB/s|安全性高，加密的传输|通过Filezilla访问，易用性适中|
	|xmodem|低|相对sftp低，无加密|易用性非常非常低，实在太难用了|
		
15. 选择和安装PC上的交叉编译环境；
	- 下载ARM GCC交叉编译器
	<pre><code>git clone git://github.com/raspberrypi/tools.git</code></pre>
	- 加入在~/.bashrc最后一行环境变量
	<pre><code>sudo gedit ~/.bashrc
	export PATH=$PATH:$HOME/rpi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin</code></pre>
	- 运行`source ~/.bashrc`
	- 运行一下命令以验证环境是否搭建成功
	<pre><code>arm-linux-gnueabihf-gcc -v</code></pre>
	如果搭建成功，显示如下
	![](/Users/kuanlu/Desktop/嵌入式系统/snap18.png)
16. 交叉编译C语言的浮点运算程序到pcDuino上去运行，证明所编译的程序是ARM的；
	- 首先编辑一段带有浮点数运算的源代码
	- 在终端中用交叉编译工具编译
	<pre><code>arm-linux-gnueabihf-gcc test.c -o test</code></pre>
	- 证明编译的程序是ARM的，汇编成汇编指令
	<pre><code>arm-linux-gnueabihf-gcc -S test.c -o test.asm</code></pre>
	打开test.asm显示的汇编语言为ARM的  
	<pre><code>	.arch armv6
	.eabi_attribute 27, 3
	.eabi_attribute 28, 1
	.fpu vfp
	.eabi_attribute 20, 1
	.eabi_attribute 21, 1
	.eabi_attribute 23, 3
	.eabi_attribute 24, 1
	.eabi_attribute 25, 1
	.eabi_attribute 26, 2
	.eabi_attribute 30, 6
	.eabi_attribute 34, 1
	.eabi_attribute 18, 4
	.file	"test.cpp"
	.section	.rodata
	.align	2
.LC0:
	.ascii	"Hello World\000"
	.align	2
.LC1:
	.ascii	"%.2f\012\000"
	.text
	.align	2
	.global	main
	.type	main, %function
main:
	.fnstart
.LFB0:
	@ args = 0, pretend = 0, frame = 8
	@ frame_needed = 1, uses_anonymous_args = 0
	stmfd	sp!, {fp, lr}
	.save {fp, lr}
	.setfp fp, sp, #4
	add	fp, sp, #4
	.pad #8
	sub	sp, sp, #8
	ldr	r3, .L3
	str	r3, [fp, #-8]	@ float
	ldr	r0, .L3+4
	bl	puts
	flds	s15, [fp, #-8]
	fadds	s15, s15, s15
	fcvtds	d7, s15
	ldr	r0, .L3+8
	fmrrd	r2, r3, d7
	bl	printf
	mov	r3, #0
	mov	r0, r3
	sub	sp, fp, #4
	@ sp needed
	ldmfd	sp!, {fp, pc}
.L4:
	.align	2
.L3:
	.word	1078523331
	.word	.LC0
	.word	.LC1
	.fnend
	.size	main, .-main
	.ident	"GCC: (crosstool-NG linaro-1.13.1+bzr2650 - Linaro GCC 2014.03) 4.8.3 20140303 (prerelease)"
	.section	.note.GNU-stack,"",%progbits</code></pre>
	- 通过sftp上传到板子上直接运行，结果如下
	![](/Users/kuanlu/Desktop/嵌入式系统/snap19.png)
17. 尝试嵌入式板卡上的三个语言的开发环境：C/C++、Python和Java；
	- C/C++ 开发环境，CodeBlocks,通过`sudo apt-get install codeblocks`安装
	![](/Users/kuanlu/Desktop/嵌入式系统/C++code.png)
	运行结果  
	![](/Users/kuanlu/Desktop/嵌入式系统/C++res.png)
	- python 开发环境，文本编辑器，编辑完成了直接终端使用`python helloworld.py`执行
	![](/Users/kuanlu/Desktop/嵌入式系统/python.png)
	- Java 开发环境，文本编辑器，编辑完了直接终端使用`javac Hello.java`和`java Hello`执行
	![](/Users/kuanlu/Desktop/嵌入式系统/java.png)
18. x-window（通过SSH）远程访问图形桌面。
	- 首先下载Windows下的X server: [X Win32](http://download.csdn.net/download/salmon5/5037800)
	- 安装完成后打开Putty，勾选ssh选项中的X11中的enable X11 forwarding
	- 在树莓派上安装X-term
	<pre><code>sudo apt-get install xterm</code></pre>
	- 用Putty ssh登录树莓派用以下命令开启x11 forwarding
	<pre><code>xterm -ls</code></pre>
	执行后会自动打开X-Win32的终端，在那个终端中就可以执行图形化引用程序了，比如说codeblocks IDE
	![](/Users/kuanlu/Desktop/嵌入式系统/snap21.png)
	![](/Users/kuanlu/Desktop/嵌入式系统/snap20.png)

---

# 拓展步骤
1. 尝试HDMI-VGA线  
	![](/Users/kuanlu/Desktop/嵌入式系统/hdmi.png)
2. 配置SAMBA 见上文samba配置文件，已经实现了PC端写共享文件
5. 嵌入式GUI IDE C++ IDE codeblocks
	![](/Users/kuanlu/Desktop/嵌入式系统/C++code.png)