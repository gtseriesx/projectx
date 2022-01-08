# 雷蛇13英寸游戏本黑苹果 | Hackintosh for Razer Blade Stealth 13
贡献
https://apple.sqlsec.com/
https://blog.daliansky.net/
https://github.com/balena-io/etcher/
https://github.com/acidanthera/OpenCorePkg/
https://dortania.github.io/OpenCore-Install-Guide/
https://github.com/LuckyCrack/OpenCore-Themes/


笔记本基本配置
Specification
Details
Model
Razer Blade Steath 13 Late2019
Processor
Intel Core i7-1065G7 (Ice Lake U / 4C8H 1.3-3.9GHz)
Motherboard

Integrated Graphics
Intel® Iris® Plus
Discrete graphics card
GeForce GTX 1650 Max-Q(30W)
Memory
16GB RAM & 512GB SSD (Liteon CA3-8D512 PCIEx4)
Sound Card
Realtek ALC298
Wireless Card
Intel® Wi-Fi 6 AX201
Official Websit
https://www.razer.com/gaming-laptops/razer-blade-stealth

步骤
1. 下载Mac镜像
黑果小兵https://blog.daliansky.net/

2. 写入U盘或者移动硬盘
使用Balena-Etcher（https://github.com/balena-io/etcher/），写入完成后会有对应的EFI分区，使用DiskGenius查看
选择镜像->选择盘符->写入

Unable to paste block outside Docs
Unable to paste block outside Docs


3. EFI分区
（因为下载的dmg镜像使用的Clover引导，我们使用OpenCore，故删除EFI分区下相关文件）
Tips：EFI分区一般单系统200MB以上（FAT16/32），多系统建议300MB

4. 配置适合自己机型的OpenCore EFI文件
  1. 下载https://github.com/acidanthera/OpenCorePkg，Release 0.75版本
Unable to paste block outside Docs
  2. 解压得到如下，提取X64中EFI文件夹作为定制的基础；看到EFI下BOOT和OC，主要在OC中配置适合自己机型的驱动等文件
Docs：存放 OC 最新的配置文档、版本更新变化、ACPI 示例文件以及 Sample.list 配置文件模板
IA32：里面放着 32 位老机器使用的 EFI 引导文件
Utilities：OC 官方集成的小工具都放在这里
X64： 里面放着 64 位目前主流机器使用的 EFI 引导文件


  3. OC/Drivers（基础通用驱动）
精简，留下如下文件
详细介绍
保留文件
AudioDxe.efi：用与启动的时候播放 Duang 的声音，就像白苹果那样
CrScreenshotDxe.efi：OC 引导界面截图驱动，按 F10 会保存当前界面的截图到 EFI 分区的根目录下
HiiDatabase.efi：用于支持 UEFI 字体渲染，四代酷睿后一般不需要
NvmExpressDxe.efi：用于让老主板支持 NVME Express 设备，四代酷睿后一般不需要
OpenCanopy.efi：使用图形化 OC 主题必备驱动
OpenHfsPlus.efi：文件系统驱动，用于支持识别 HFS+ 的磁盘格式
OpenLinuxBoot.efi：OC 0.7.3 新增的驱动，用于引导 Linux 系统
OpenPartitionDxe.efi：分区管理驱动程序。用于加载旧版 macOS 的 DMG 映像
OpenRuntime.efi：OC 核心必备驱动，功能比较强大，大家记住这个是必备的驱动就行
OpenUsbKbDxe.efi：USB 键盘驱动，用于模拟苹果热键，是 KeySupport 的等效方案
Ps2KeyboardDxe.efi：PS/2 键盘驱动，这个 PS/2 键盘也太老了吧，我好多年没见过了
Ps2MouseDxe.efi：PS/2 鼠标驱动，同样也太老了，很多年没有见过了
UsbMouseDxe.efi：USB 鼠标驱动，有些虚拟机需要依赖改驱动才可以在引导界面使用鼠标
XhciDxe.efi：XHCI USB controller 驱动程序，基本上 2 代酷睿开始大多数固件都自带这个驱动程序了



  4. OC/Resources（启动主题）
我使用的是https://github.com/LuckyCrack/OpenCore-Themes

Unable to paste block outside Docs


  5. OC/ACPI（可以理解为主板总线等IO驱动）配置
我参考的是：（可以直接看iv部分结论）
    1. 提供方1：国光推荐的一部分
    2. 提供方2：国外友人LG Gram（同CPU大概率同主板芯片组通用）黑苹果的ACPI
    3. 提供方 3：D神的ACPI Getting Start项目中的Ice Lake
ACPI基于底层驱动，对系统很重要，对新手又不太友好，所以需要多方参考资料
提供方
提供方1：国光推荐

提供方 2：Lg Gram
https://github.com/AskDavis/LG-Gram-17Z90N/tree/master/EFI/OC/ACPI
提供示例
Coffee Lake Plus、Comet Lake
SSDT-PLUG-DRTNIA.aml
SSDT-EC-USBX-LAPTOP.aml
SSDT-XOSI.aml
触控板连接修复
需要配合 ACPI 补丁：Change _OSI to XOSI 来使用
NUC 不需要这个
如果这个不成功的话，可手动使用 MaciASL 编译 SSDT-GPI0.dsl.zip 来替代 XOSI
SSDT-PNLF-CFL.aml
SSDT-AWAC.aml
修复较新硬件上的系统时钟
支持以下主板：
B360、B365、H310、H370
Z370（具有较新 BIOS 版本的 Gigabyte 和 AsRock 主板）
Z390
B460、Z490
400系列 （Comet Lake）
495系列 （Ice lake）
SSDT-PMC.aml
用来支持适配 NVRAM
300 系列主板都需要此 SSDT（Z370除外）
支持以下主板：
B360、B365
H310、H370（HM370 应该不需要这个）
Z390

文件
共计5个，第4项失效
Unable to paste block outside Docs
Unable to paste block outside Docs
Unable to paste block outside Docs
SSDT-PNLF-CFL.aml
Unable to paste block outside Docs
Unable to paste block outside Docs
共计6个
Unable to paste block outside Docs与左2重复
Unable to paste block outside Docs与左1重复
Unable to paste block outside Docs补充左4项
Unable to paste block outside Docs
Unable to paste block outside Docs与左2重复
Unable to paste block outside Docs与左3重复

提供方3
适用于Ice Lake
参考：https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/icelake.html#acpi
Required SSDTs
Description
SSDT-PLUG
CPU电源管理，适用于Haswell及更新的CPU
Allows for native CPU power management on Haswell and newer, see Getting Started With ACPI Guide (opens new window)for more details.
SSDT-EC-USBX
板载管理及USB供电
Fixes both the embedded controller and USB power, see Getting Started With ACPI Guide (opens new window)for more details.
SSDT-GPIO
触控板（非笔记本不需要）
Creates a stub so VoodooI2C can connect, for those having troubles getting VoodooI2C working can try SSDT-XOSI (opens new window)instead. Note that Intel NUCs do not need this
SSDT-PNLF
显示器亮度调节（非笔记本不需要）
Fixes brightness control, see Getting Started With ACPI Guide (opens new window)for more details. Note that Intel NUCs do not need this
SSDT-AWAC
修复较新硬件上的系统时钟
This is the 300 series RTC patch (opens new window), required for most B360, B365, H310, H370, Z390 and some Z370 boards which prevent systems from booting macOS. The alternative is SSDT-RTC0 (opens new window)for when AWAC SSDT is incompatible due to missing the Legacy RTC clock, to check whether you need it and which to use please see Getting started with ACPI (opens new window)page.
SSDT-RHUB
取得最高权限（root）才需要，如果不需要则不需要
Needed to fix Root-device errors on many Icelake laptops
同时参考如下需求表
https://github.com/dortania/Getting-Started-With-ACPI/blob/master/ssdt-platform.md


    4. 所以我最终确认使用的是：可从D网址下载https://github.com/dortania/Getting-Started-With-ACPI/tree/master/extra-files/compiled
      1. SSDT-PLUG
      2. SSDT-EC-USBX
      3. SSDT-PNLF
      4. SSDT-XOSI
      5. SSDT-AWAC
      6. SSDT-RHUB
      7. SSDT-IMEI
  6. OC/Kexts（额外扩展驱动，通俗意义的“驱动”）配置
    1. 参考与理解，如需结果可直接跳至第二部分
提供方1：国光
https://apple.sqlsec.com/3-%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C/3-4.html
提供方2：OC官方
https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Kexts.md
准备 Kexts
基本概念
Kext 的英文全称叫做 Kernel Extension，即内核扩展，我们可以通俗的理解为这个就是 macOS 的驱动，使用方法只需要将这些 kext 文件放入到 EFI/OC/kexts 文件夹下面，然后编辑 OC 配置文件加载这些 kexts 并调节好顺序即可。
下载 Kexts
下面国光来简单列举一些下载 Kexts 的方法：
使用 OpenCore Configurator 这类第三方 OC 编辑器软件下载
搜索引擎搜索 Kexts 的Github 下面地址，手动去 Releases 里面寻找编译好的 kext 文件
使用 OC 官方的下载页面来下载常见的 kexts：https://dortania.github.io/builds/
Kexts 的分类
必备驱动
必备的 kexts 如果缺失的话，你的黑苹果系统就无法启动了
VirtualSMC.kext
模拟白苹果的 SMC 芯片
替代古老的 FakeSMC
只支持 OS X 10.6+ 版本的系统
Lilu.kext
很多著名 kexts 的依赖，没有 Liu 就无法正常使用 AppleALC、WhateverGreen、VirtualSMC 等
只支持 OS X 10.8+ 版本的系统
VirualSMC 插件
大家下载好 VirtualSMC 编译好的 kexts 的话，会发现里面还躺着其他的 kexts，这些其他的 kexts 就是 VirtualSMC 的插件了，下面来列举说明一下这些插件的作用：
SMCBatteryManager.kext
笔记本专用，用于正确读取显示电池容量
SMCDellSensors.kext
某些 Dell 机器专用，一般不是 Dell 的机器不需要使用
对支持 SMM（系统管理模式）的 Dell 机器的风扇进行更准确的监视和控制
SMCLightSensor.kext
笔记本专用，用于笔记本电脑上的环境光感应器
大多都的笔记本都没有这个感应器，所以就算用了也只是伪感光
SMCProcessor.kext
用于监控 CPU 温度，台式机和笔记本都适用
不支持 AMD 的 CPU
SMCSuperIO.kext
用于监控风扇的转速，台式机和笔记本都适用
不支持 AMD 的 CPU
显卡驱动
WhateverGreen.kext
基本上所有的核显和独显都要使用这个 kext
用于图形修补、DRM 修复、缓冲区修复等
只支持 OS X 10.8+ 版本的系统
声卡驱动
AppleALC.kext
用于 AppleHDA 修补，支持大多数的板载声卡驱动
文件夹下的 AppleALCU.kext 是 AppleALC 的精简版，仅支持数字音频
AMD 的主板和 CPU 可能会遇到一些问题，很少可以驱动麦克风
只支持 OS X 10.8+ 版本的系统
VoodooHDA.kext
比较古老且经典的声卡驱动，也叫万能声卡驱动
如果 AppeALC.kext 无法驱动话可以考虑这个
但是使用体验完美度肯定不如原生的 AppleALC.kext 的
只支持 OS X 10.6+ 版本的系统
USB 驱动
USBInjectAll.kext
RehabMan 康复者之前的 USB 驱动
18 年 11 月发布的 0.7.1 是最后应该版本，后面再没有更新过
用于在 ACPI 中没有定义 USB 端口的系统上注入 Intel USB 控制器
Skylake+ 的桌面CPU 不需要这个
AsRock 华擎主板的话可能还是需要这个
Coffee Lake 貌似也还是需要这个
Skykak 之前的 CPU 理论上也是需要
支持 OS X 10.11+ 版本的系统
写到这里的时候感慨万千，RehabMan 可以说是黑苹果届的元老了，他也是 Tonymacx86 的一位版主，很多著名的黑苹果 kexts 都出自于他的手里，但是因为种种原因 18 年之后就再也没有活跃了，销声匿迹仿佛这个人没有来过一样，但是江湖上依然还有他的传说：Can we all thank RehabMan
我真的是太佩服这种人了，十年如一日的在论坛里面解答问题，定期更新这开源的 kexts，甚至有些 Apple 苹果开发者都来像他学习。黑苹果比较讽刺的是，伸手党没有感恩之心的人太多了，也许他安装系统遇到困难的时候就去你的 Github 下面提交 issue 催着你更新，就好像你开源这个驱动就要为他负责到底一样；安装成功之后呢，提问的人也就消失了，甚至连句谢谢都没有说，更不会留下任何有价值的文档信息之类的。这样就会导致很多大佬们每天千篇一律的回答各种重复的毫无技术含量的问题，如果是国光我的话，我肯定坚持不了几天的，但是 RehabMan 坚持了 10 余年，这真的是太令人震撼了。RehabMan 在 TonymacX86 的最后一个帖子说道：“我还在，但忙于其他（现实生活）的事情。将无法回答这里的问题。人是需要学习阅读的。” 但愿，希望真的如此，而不是被这些没有感恩之心的人伤透了心。
USBInjectAll.kext
国内黑苹果小兵大佬维护的版本
在 RehabMan 基础性更新完善的版本，目前到了 0.7.7 版本
支持后面新的 400、500 系列主板的支持
说到黑苹果小兵，国光我也很佩服这样的人，初次接触认识他的时候，我还以为是一个 30 多岁的中年人，结果后面才知道他的儿子已经上大学了，而且他也快到了退休的年龄。
黑果小兵也像 RehabMan 一样，写了很多黑苹果教程文章，也开源了很多机型的驱动，而且常年来一直坚持提供黑苹果懒人镜像的下载包，最关键的是都快退休的年龄了，还坚持做黑苹果这个比较“时髦”的技术，真的厉害了，不知道国光我老的时候，还可以坚持做这些吗？哈哈~ 如果黑苹果技术还存在的话。
有线网卡驱动
AtherosE2200Ethernet.kext
Atheros 高通和 Killer 杀手 网卡 需要
注意：Atheros Killer E2500 型号实际上是基于 Realtek 的，所以请使用 RealtekRTL8111 驱动
支持 OS X 10.8+ 版本的系统
IntelMausi.kext
大多数 Intel 因特尔的网卡驱动
基于 I211 的芯片组的网卡需要使用 SmallTreeIntel82576 kext
官方支持 Intel 的 82578、82579、I217、I218 和 I219 网卡
详细支持驱动的有线网卡型号可以参考：https://github.com/acidanthera/IntelMausi
需要 OS X 10.9 或更新版本，10.6-10.8 的老用户可以使用 IntelSnowMausi 替代
LucyRTL8125Ethernet.kext
Realtek 的 2.5Gb 的网卡驱动
官方这个页面需要注册才可以下载，也可以下载国光我上传蓝奏云的版本
需要 macOS 10.15+ 版本的系统
RealtekRTL8111.kext
大多数 Realtek 的千兆网卡驱动
注意：有时最新版本的 kext 可能无法正常工作，这个时候可以尝试使用旧版本。
SmallTreeIntel82576.kext
I211 有线网卡驱动
大多数配备 intel 有线网卡的 AMD 主板需要
版本支持情况 OS X 10.9-12(v1.0.6)、macOS 10.13-14(v1.2.5)、macOS 10.15+(v1.3.0)
其他不需要 kext 的有线网卡
Intel I225-V
某些高端的 Comet Lake 主板会配备这个 I225-V 2.5GBe 有线网卡
OC 配置文件的设备属性里面添加 PciRoot(0x0)/Pci(0x1C,0x1)/Pci(0x0,0x0) 内容如下：
device-id F2150000 类型为 DATA 类型
如果上面添加后遇到 AppleIntelI210Ethernet kext 内核报错的话，那么可以换成以下路径：
PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)
需要 macOS 10.15 或更高版本
Intel I350
OC 配置文件的设备属性里面添加 PciRoot(0x0)/Pci(0x1,0x1)/Pci(0x0,0x0) 内容如下：
device-id 33150000 类型为 DATA 类型
需要 OS X 10.10 或更新版本
一些比较古老的百兆有线网卡驱动
AppleIntelE1000e.kext
主要与基于 10/100MBe 的 Intel 有线网卡相关
需要 10.6 或更高版本
RealtekRTL8100.kext
支持的网卡型号有 RTL8101E、RTL8102E、RTL8103E、RTL8401E、RTL8105E、RTL8402、RTL8106E、RTL8106EUS、RTL8107E
官方这个页面需要注册才可以下载，也可以下载国光我上传蓝奏云的版本
BCM5722D.kext
Broadcom 的有线网卡驱动
支持的网卡型号有 BCM5722、BCM5754、BCM5754M、BCM5755、BCM5755M、BCM57788、BCM5787、BCM5787M、BCM5906、BCM5906M
需要 OS X 10.6 或更新版本
无线网卡驱动
intel 无线网卡系列
国内 zxystd 大佬从 Linux OpenBSD 移植的驱动，非常硬核，完成度很高，接力也都可以正常使用，隔空投送目前只能识别，暂时还无法传输文件，不过已经很厉害了。
AirportItlwm.kext
Intel 网卡的 WiFi 驱动
支持驱动的 intel 无线网卡型号表：https://docs.oiw.workers.dev/itlwm/Compat.html
只支持 macOS 10.13 以及更高的版本
IntelBluetoothFirmware.kext 与 IntelBluetoothFirmware.kext
Intel 网卡的蓝牙驱动，与 AirportItlwm.kext 搭配使用
只支持 macOS 10.13 以及更高的版本
如果确定你的网卡型号支持驱动，但是蓝牙无法使用，那么多半是你的 USB 没有定制好
Broadcom 博通免驱系列
免驱网卡型号众多，可以参考 OC 官方的无线网卡购买指南
AirportBrcmFixup.kext
非苹果原装无线网卡或者非 Fenvi 奋威的博通网卡的无线网卡驱动
支持 OS X 10.10 以及更高的版本
Big Sur 后面的系统可能有些问题，可以参考官方的解决方案
BrcmPatchRAM 系列
所有非 Apple/非 Fenvi 无线网卡的蓝牙驱动
BrcmPatchRAM.kext 10.8-10.10 系统使用
BrcmPatchRAM2.kext 10.11-10.14 系统使用
BrcmPatchRAM3.kext 10.15+的系统使用
博通网卡的几个细节，Big Sur 以及后面的系统由于驱动有点异常，需要手动删除 AirPortBrcm4360_Injector.kext
蓝牙加载需要一定顺序，下面是 10.15+ 系统的蓝牙加载顺序 `Kernel -> Add `：
BrcmBluetoothInjector.kext
BrcmFirmwareData.kext
BrcmPatchRAM3.kext
其他驱动
CpuTscSync.kext
在某些 Intel 的 HEDT 和服务器主板上同步 TSC 需要,如果没有这个 macOS 可能会非常慢甚至无法启动。
不适用于 AMD CPU
需要 OS X 10.8 或更新版本
为具有MSR_IA32_TSC_ADJUST(03Bh) 的CPU 添加了 macOS 12 兼容性
NVMeFix.kext
用于修复非 Apple 苹果的 NVMe 上的电源管理和初始化
需要 macOS 10.14 或更高版本
HibernationFixup.kext
一个旨在修复休眠兼容性问题的 Lilu 插件
解决黑苹果系统睡眠后无法唤醒、死机、黑屏的问题
SATA-unsupported.kext
笔记本电脑 在 macOS 中无法看到 SATA 硬盘驱动器的话，可以考虑使用
CtlnaAHCIPort.kext
一般在 Big Sur 下笔记本电脑 在 macOS 中无法看到 SATA 硬盘驱动器的话，可以考虑使用
AMD 常用驱动
AMDRyzenCPUPowerManagement.kext
AMD 处理器的电源管理驱动
SMCAMDProcessor.kext
AMD 处理器的传感器监控和 VirtualSMC 插件
AppleMCEReporterDisabler.kext
用于关闭 AppleMCERReport
AppleMCERReport会导致AMD CPU的内核崩溃
某些双 CPU 的主板可能也有帮助
受影响的 SMBIOS 为：MacPro6,1、MacPro7,1、iMacPro1,1
需要 macOS 10.15 或更高版本
XLNCUSBFix.kext
AMD FX 系统的 USB 修复，不推荐用于 Ryzen
需要 macOS 10.13 或更高版本
VoodooHDA.kext
FX 系统的音频和 Ryzen 系统的前面板麦克风和外放的支持
请勿与 AppleALC 混合使用
比较古老且经典的声卡驱动，也叫万能声卡驱动
如果 AppeALC.kext 无法驱动话可以考虑这个
但是使用体验完美度肯定不如原生的 AppleALC.kext 的
只支持 OS X 10.6+ 版本的系统
笔记本专用驱动
输入设备驱动
VoodooPS2Controller.kext
适用于配备 PS2 键盘、鼠标和触控板的系统
MT2 (Magic Trackpad 2) 功能需要 macOS 10.11 或更新版本
RehabMan 的 VoodooPS2Controller.kext
对于带有 PS2 键盘、鼠标和触控板的旧系统，或者当您不想使用 VoodooInput 时
支持 macOS 10.6+ 支持
VoodooRMI.kext 和 VoodooSMBus.kext
对于带有 Synaptics SMBus 设备的触控板驱动
主要用于触控板和轨迹点，ThinkPad 小红点也可以驱动
MT2 (Magic Trackpad 2) 功能需要 macOS 10.11 或更新版本
VoodooSMBus.kext
对于带有基于 ELAN SMBus 的设备触控板驱动
主要用于触控板和轨迹点
目前支持 macOS 10.14 或更新版本
VoodooI2C.kext
用于修复 I2C 设备的触控板驱动
一般是一些更高级的触摸板和或者是触摸屏
MT2 (Magic Trackpad 2) 功能需要 macOS 10.11 或更新版本
VoodooI2C 的一些插件
VoodooI2CHID.kext：微软 HID 驱动，也支持某些型号的触控屏
VoodooI2CELAN.kext：ELAN 专用，ELAN1200+ 的版本需要 VoodooI2CHID.kext 代替
VoodooI2CSynaptics.kext：Synaptics 专用，Synaptics F12 协议需VoodooI2CHID代替
VoodooI2CFTE.kext：FTE1001 触控板
VoodooI2CAtmelMXT.kext：Atmel 多点触控协议
其他驱动
ECEnabler.kext
修复了在大多数笔记本上读取电池状态的问题（允许读取超过 8 位长的 EC 字段）
BrightnessKeys.kext
笔记本亮度快捷键驱动
AsusSMC.kext
华硕笔记本电脑专用的 VirtualSMC 插件
提供 ALS、键盘背光和 Fn 键驱动，支持电池监控充电
支持配备了 ATK 设备的华硕笔记本电脑
CPUFriend.kext 和 CPUFriendDataProvider.kext
可以实现对 macOS CPU 频率睿频性能的调整
需要配合脚本生成时候自己机型的 kexts，可参考官方教程
黑苹果 Kexts 大全?
由于 Kexts 太多太杂了，这个工作量太大了，我直接贴一些轮子的地址，大家自己去看看就行：
OpenCore 常用 Kexts
一些比较老的 Kexts
一些基于Liu 的 Kexts
如果你是一个有耐心的人的话，如果整理好 Kexts 完整列表的话，欢迎提交 PR
Kexts
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}EthernetMinKernel (Min macOS)MaxKernel (Max macOS)NoteAppleRTL8169Ethernet———AtherosE2200Ethernet.kext———AtherosL1cEthernet.kext———IntelMausi.kext13.0.0 (10.9)——IntelSnowMausi.kext10.0.0 (10.6)12.0.0 (10.8)Not testedIntelMausiEthernet.kext———NullEthernetInjector.kext———RealtekR1000SL.kext———RealtekRTL8100.kext———RealtekRTL8111.kext———
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}Wi-Fi and bluetoothMinKernel (Min macOS)MaxKernel (Max macOS)NoteAirPortAtheros40.kext18.0.0 (10.14)—From 10.13AirportBrcmFixup.kext12.0.0 (10.8)——ATH9KFixup.kext———BrcmPatchRAM14.0.0 (10.10)——IntelBluetoothFirmware———MT7610———RT5370———RTL8192CU———
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}Keyboard, trackpad and mouseMinKernel (Min macOS)MaxKernel (Max macOS)NoteBrightnessKeys.kext———GK701HIDDevice.kext———NoTouchID.kext17.0.0 (10.13)——SerialMouse.kext———VoodooI2C.kext16.0.0 (10.12)——VoodooPS2Controller.kext15.0.0 (10.11)——VoodooPS2Keyboard.kext15.0.0 (10.11)——VoodooPS2Mouse.kext15.0.0 (10.11)——VoodooPS2Trackpad.kext15.0.0 (10.11)——VoodooInput.kext15.0.0 (10.11)——VoodooSMBus.kext18.0.0 (10.14)——VoodooRMI.kext15.0.0 (10.11)——AlpsT4USB.kext———
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}Video and audioMinKernel (Min macOS)MaxKernel (Max macOS)NoteAppleALC.kext8.0.0 (10.4)——AppleALCU.kext8.0.0 (10.4)——EMUUSBAudio.kext———kXAudioDriver.kext———Nvidia CUDA drivers10.0.0 (10.6)17.9.9 (10.13)—Nvidia Web-drivers12.0.0 (10.8)17.9.9 (10.13)—SNBGraphicsMojaveInstaller18.0.0 (10.14)—From 10.13VoodooHDA.kext———WhateverGreen.kext10.0.0 (10.6)——Polaris22Fixup.kext18.0.0 (10.14)——
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}CPU and SMCMinKernel (Min macOS)MaxKernel (Max macOS)NoteAAAMouSSE.kext16.0.0 (10.12)——AppleMCEReporterDisabler.kext———AsusSMC.kext———CPUFriend.kext15.0.0 (10.11)——HWPEnabler.kext———OpcodeEmulator.kext———telemetrap.kext18.0.0 (10.14)——TSCAdjustReset.kext———VoodooTSCSync.kext———CpuTscSync.kext12.0.0 (10.8)——CpuTopologySync.kext19.0.0 (10.15)——FakeSMC-32.kext8.0.0 (10.4)11.9.9 (10.7)For VMs with EFI64VirtualSMC.kext8.0.0 (10.4)——SMCLightSensor.kext10.0.0 (10.6)—Not tested 10.6 and 10.7SMCSuperIO.kext10.0.0 (10.6)—Not tested 10.6 and 10.7SMCBatteryManager.kext8.0.0 (10.4)——SMCProcessor.kext11.0.0 (10.7)—Not tested 10.7SMCDellSensor.kext11.0.0 (10.7)—Not tested 10.7
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}USB and other portsMinKernel (Min macOS)MaxKernel (Max macOS)NoteIOElectrify.kext———Legacy_InternalHub-EHCx.kext15.0.0 (10.11)——Legacy_USB3.kext15.0.0 (10.11)——NVMeFix.kext18.0.0 (10.14)——USBWakeFixup.kext———SASMegaRAID.kext———Sinetek-rtsx.kext———VoodooSDHC.kext———RealtekCardReader.kext———
<!--br {mso-data-placement:same-cell;}--> td {white-space:pre-wrap;border:1px solid #dee0e3;}Other kextsMinKernel (Min macOS)MaxKernel (Max macOS)NoteAppleIntelInfo.kext———DebugEnhancer.kext12.0.0 (10.8)——HibernationFixup.kext14.0.0 (10.10)——Lilu.kext8.0.0 (10.4)——RestrictEvents.kext12.0.0 (10.8)——RTCMemoryFixup.kext12.0.0 (10.8)——WebCamera.kext———TOSMotionSensor.kext———FeatureUnlock.kext———MacHyperVSupport.kext10.0.0 (10.6)——
More complete list with legacy kexts is hosted here. Full Lilu plugin list with legacy kexts is hosted here. For developers only.
Acidanthera members are not affiliated with the authors of any kernel extensions but ones hosted at https://github.com/acidanthera. This list is provided for information purposes without warranty of any kind.

    2. 最终使用清单
类型
文件
子项
基础驱动
Unable to paste block outside Docs
Unable to paste block outside Docs
btw VirtualSMC中
SMCBatteryManager.kext
笔记本专用，读取显示电池容量
SMCLightSensor.kext
笔记本专用，环境光感应器
SMCProcessor.kext
用于监控 CPU 温度，台式机和笔记本都适用
SMCSuperIO.kext
用于监控风扇的转速，台式机和笔记本都适用
显卡驱动
Unable to paste block outside Docs

USB
Unable to paste block outside Docs

无线网卡
Unable to paste block outside Docs
注意选择对应系统
https://github.com/OpenIntelWireless/itlwm/releases/tag/v2.0.0
蓝牙
Unable to paste block outside Docs

触控板


其他驱动
Unable to paste block outside Docs
Unable to paste block outside Docs
Unable to paste block outside Docs
NVMeFix.kext
修复非苹果的 NVMe 上的电源管理和初始化
HibernationFixup.kext
一个旨在修复休眠兼容性问题的 Lilu 插件

  7. Config.plist（可以理解为整个OC的目录文件）
    1. 创建Config.plist文件
复制OP0.75/Docs/Sample.plist至EFI/OC，并命名至Config.plist
    2. 添加前几步在OC文件夹配置好的文件
Win下使用QtOpenCoreConfig编辑该文件，逐一将刚才的Drivers、Resources、ACPI、Kexts和Tools（可选）添加进来（删除所有默认的选项，因为都是空路径）
Unable to paste block outside Docs
OC文件夹内容
程序对应操作

OC/ACPI
ACPI/Add/拖入

OC/Kexts

Kernel/Add/拖入
*注意Kexts顺序，见下表

OC/Tools（可选，我用的OC解压出来就有的）
Misc/Tools/拖入

OC/Drivers
UEFI/Drivers/拖入






最终我的顺序
*Kexts顺序


必备Kexts
Lilu. kext
Virtualsmc. kext
Whatever Green. kext
Smcbattery Manager.kext(台式机不需要)
Smclightsensor.kext(台式机不需要)
Smcprocessor kext
Smcsuperlo. kext
Applealc. kext
Intel无线和蓝牙加载顺序
Airportltlwm. kext
Intelbluetoothinjector. kext
Intelbluetoothfirmware. kext
笔记本PS2键鼠、触控板加载顺序
Voodoops2controller. kext
Voodoo$ Controller. kext/Contents/Plugins/Voodoops2keyboard kext
Voodoop2controller kext/Contents/Plugins/Voodoops2mouse kext
Voodoops2controller kext/Contents/Plugins/Voodoops2trackpad kext
Voodoops2controller kext/Contents/Plugins/Voodoolnput kext
Brightnesskeys.kext(功能亮度调节按键驱动不一定需要)
笔记本2C和PS2配合驱动触控板加载顺序
Voodool2c kext/Contents/Plugins/Voodool2cservices kext
Voodool2c kext/Contents/Plugins/Voodoo GPIO. kext
Voodool2, kext
Voodool2chid. kext
Voodoops2 Controller kext
Voodoops2controller kext/Contents/Plugins/Voodoops2keyboard kext
Voodool2c kext/Contents/Plugins/Voodoolnput.kext
Brightnesskeys.kext(功能亮度调节按键驱动不一定需要)
 补充一个小姿势:因为IC2和PS2都有 Voodoolnput.kext,所以如果不删除掉或者禁用掉PS2的
 Voodoolnput.kext的话,会导致开机内核冲突卡死无法开机。

    3. 配置Quirks选项
程序对应操作
参考：https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/icelake.html#starting-point
释义
ACPI/Quirks
默认设置

Fadtenable Reset
在旧硬件上修复重启和关机,除非需要,否则不推荐开启
一些较新的笔记本可能也需要这个选项来修复重启和关机
Normalizeheaders
清除ACI头字段,只有 macos10.13需要
10.14后面的版本已经修复这个问题了
Rebase Regions
尝试试探性地重新定位AC内存区域,使用自定义DSDT则必须开启
Resethwsig
适用于无法在重新启动期间维护硬件签名井导致从休眠中唤醒问题的硬件
Resetlogostatus
无法在有BGRT表的系統上显示 DEM Windows标志的硬件需要开启
Synctablelds
这可以解決修补表与SLIC表不兼容导致旧 Windows操作系统中的许可问题。
Booter/Quirks
严格按下图设置

附


DP/Add
声卡
从网址内找到你声卡对应的地址https://github.com/acidanthera/AppleALC/wiki/Supported-codecs
比如我的ALC298，对应3，11，13，等等都可以

数据类型选择Number，数字我选的13

显卡
点击预置，选择第2项(0x2,0x0)添加

删除除下图所示外所有值，并设置为0000528A


Kernel/Quirks
严格按下图设置
*如果用的11.3之前系统则需要将XhciPortLimit勾上

附


Misc
Boot
启用主题：PickerMode=External，选择刚拷贝进去的主题

Debug

Security


NVRAM/Add
点击左侧第3个7C436110-AB2A-4BBB-A880-FE41995C9F82
boot-args，数据类型String，值
-v keepsyms=1 debug=0x100 alcid=1

prev-lang:kbd，数据类型data，值：
7A682D48616E733A323532

最终效果


PI/Generic
选择机型MacBookPro16,2，点击生成，保存


UEFI
全默认


至此EFI即完美配置完毕！！！
可点击下载
Unable to paste block outside Docs
5. 完成U盘整体制作
使用DiskGenius拖入EFI分区下，保持EFI分区下EFI文件夹等目录如下

6. 开机选择U盘，注意有两个EFI启动分区，我的是第一个是MAC，第三个是WINPE，各自情况不同进行选择
