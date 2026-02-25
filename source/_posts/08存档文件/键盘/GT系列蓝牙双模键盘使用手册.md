---
title: GT系列蓝牙双模键盘使用手册
tags:
  - 工具
categories: 键盘
abbrlink: 42f323cf
date: 2026-02-24 17:44:21
---
# GT系列蓝牙双模键盘使用手册

GT系列当前包括BLE60 、 Omega45 、 Farad69、Omega50四款键盘，四款键盘均采用的LotKB开源固件进行驱动，功能按键等设定基本一致：

## 功能按键说明

**休眠按键：P** – 同时按下Lshift+Rshift时按此键可以手动进入休眠模式，按任意键可以唤醒。(此功能按键修改配列不会改变）

**关机按钮：ESC或~** – 同时按下Lshift+Rshift时按此键可以手动进入关机模式，关机后需要按背部的Bootloader按钮才可重新开机。长期不用或携带外出建议关机。

**切换蓝牙设备：Q/W/E** – 同时按下Lshift+Rshift时按Q/W/E可以在已绑定的蓝牙设备之间进行切换。

**重新开启蓝牙广播：R** – 同时按下Lshift+Rshift时按R可以重新开启蓝牙广播，用于切换设备后进行绑定。

**显示蓝牙键盘状态灯：L 或 ?** – 在电池供电情况下，状态灯会自动熄灭，如要显示当前状态灯，同时按下Lshift+Rshift时按L或？，将显示状态灯5秒。

**锁定WIN键：G** – 同时按下Lshift+Rshift时按此键，将锁定WIN键，再次同时按下Lshift+Rshift时按此键，将恢复WIN键，重启将自动恢复WIN键。

**切换连接模式按键：M** – 在通过USB和蓝牙同时连接一台设备（也可通过USB连接一台设备、蓝牙连接另一台设备）的情况下，同时按下Lshift+Rshift时按此键可以切换连接模式。如未同时使用USB模式和蓝牙模式，此按键无效。

**进入DFU刷机模式：B** – 同时按下Lshift+Rshift时按此键可以重启到DFU刷机模式。也可在唤醒的同时按Space+B进入DFU刷机模式。

**清空当前设备蓝牙绑定信息按键：O** – 同时按下Lshift+Rshift时按此键可以清空当前蓝牙绑定信息。仅清空当前设备，其余绑定设备不会清空。

**重置键盘：I** – 同时按下Lshift+Rshift时按此键可以重置键盘。重置键盘会清空所有的蓝牙绑定信息、自定义的配列、存储的键盘配置信息。

**RGB灯效控制：调整颜色： A/S/D/F/C/V** – 同时按下Lshift+Rshift时按A/S/D/F/C/V，分别是增加饱和度、降低饱和度、增加亮度、降低亮度、增加色调、降低色调。

**RGB灯效控制：调整模式：Z** – 同时按下Lshift+Rshift时按Z，循环灯效。

**RGB灯效控制：开关RGB：X** – 同时按下Lshift+Rshift时按X，切换RGB灯的开关。

PCB**背部多功能按钮** – 背部按钮属于多功能按钮。键盘正常模式下，按下一秒后松手键盘关机；按下三秒后松手键盘进入DFU刷机模式；按下10秒后松手，键盘将重置。在断电状态，按下背部按钮不放，同时按下键盘第一行首个按键或最后一个按键，然后通电，将强制进入DFU模式。

如果要绑定并切换多设备，按以下步骤操作：

## **一、初次绑定**

1、键盘上按下双Shift+Q，然后再按下双shift+R，在需要绑定键盘的设备上搜索蓝牙键盘，在搜索到相应蓝牙键盘名称后，将第一个设备绑定到键盘Q键上；

2、键盘上按下双Shift+W，然后再按下双shift+R，然后绑定第二个设备到键盘W键；；

3、键盘上按下双Shift+E，然后再按下双shift+R，然后绑定第三个设备到键盘E键；

4、通过双shift+Q/W/E进行输入设备的切换

## **二、更换绑定**

## 其他说明



**状态提示灯说明** 

GT系列蓝牙键盘状态灯一般为3颗，以Omega45为例：

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/02/24/mXRnsA.png)

如要更换某个设备的绑定，如要新绑定一个设备到W键上。
1、将W键绑定设备上的蓝牙键盘进行删除（如windows系统进入系统设备删除蓝牙键盘；Android系统进入设置-蓝牙后删除蓝牙键盘）；

2、键盘上按下双Shift+W，切换到W设备；

3、然后键盘上再按下双shift+O，删除W上绑定的设备，绑定新的设备到键盘W键上；

**相关参数的设定** – 考虑到耗电问题，正常键盘扫描按键输入为10ms一次，回报率为100Mhz；如果15秒不按键转入慢速扫描，100ms一次，当有按键按下，又自动转入正常扫描速度10ms一次；如果20分钟无按键行为将自动转入休眠模式，此时要重新启用键盘，只需要按任意键就可唤醒，唤醒动作后约1-2s可以正常输入。

**在线编译**

通过在线编辑，可以可视化配置按键配列、生成固件。

通过此网站 ：lotkb.glab.online  或 访问导航中“在线编译” 。可以可视化修改按键，更改按键更加方便；可以更改一些参数设定，如自动休眠时间、RGB灯数量等。



LotKB采用的低功耗蓝牙：Bluetooth Low Energy，并且是5.0版本，可向下兼容蓝牙4.0，但不支持蓝牙3.0及以下。由于采用的较新的协议，需要较新的系统才能支持。如：
iOS 6以上；Android 4.3以上；Windows 8.1以上；Windows Phone 8.1；较新的MacOS。
除了操作系统以外，也需要硬件设备支持，至少要硬件设备支持蓝牙4.0以上才行。从时间上来说，可能要2011年以后的设备才能支持蓝牙4.0，也才可能使用此键盘的蓝牙功能

# 键盘使用说明[¶](https://wiki.glab.online/manual/#_1)

本页面最后修改时间2025-06-09

- 本页面操作说明全系列键盘通用，所有内容基于最新版固件，如与当前键盘实际不符，建议升级固件
- 更多键盘专有说明可以访问键盘[产品信息](https://wiki.glab.online/keyboard/volta9/)页面

## 相关概念[¶](https://wiki.glab.online/manual/#_2)

### 开机[¶](https://wiki.glab.online/manual/#_3)

GT系列三模键盘PCB没有硬件电源开关，只需要接入电池或插入USB通电后，键盘就会自动开机。

软关机后，可以插入USB线 / 按击一下背部多功能按钮 / 按下旋钮中键 实现开机。

### 关机[¶](https://wiki.glab.online/manual/#_4)

按下背部多功能按钮1秒以上后（不超过3秒）松手 或 系统功能按键Lshift+Rshift+Backspace或~进入关机状态。

插入 USB 时，无法关机（关机后马上会被唤醒）。

### 自动睡眠[¶](https://wiki.glab.online/manual/#_5)

在一定时间不敲击按键后，键盘将自动进入睡眠模式以便省电。进入自动睡眠的时间可以自由设定。

自动睡眠后，按任意键可唤醒键盘重新工作。

插入 USB 后，由 USB 供电，自动睡眠功能不会启用，如处于睡眠状态也将自动唤醒。

### 手动睡眠[¶](https://wiki.glab.online/manual/#_6)

同时按下Lshift+Rshift+P可以手动进入睡眠模式

手动睡眠后，按任意键可唤醒键盘重新工作。

如果启用了`唤醒按键`功能，手动睡眠后唤醒时需要按Space+U才能唤醒键盘。

插入 USB 后，将直接唤醒键盘重新工作，并且无法手动睡眠。

### 蓝牙连接设备[¶](https://wiki.glab.online/manual/#_7)

如需通过 蓝牙连接 进行输入，确保键盘处于蓝牙连接模式，并完成 [配对蓝牙设备](https://wiki.glab.online/manual/#配对蓝牙设备) 的过程。

### 2.4G连接设备[¶](https://wiki.glab.online/manual/#24g)

如需通过 2.4G无线 进行输入，确保键盘处于2.4G无线连接模式，将 2.4G接收器 插入设备的USB口，并完成[2.4G接收器配对](https://wiki.glab.online/manual/#2.4G接收器配对)。

### USB 连接设备[¶](https://wiki.glab.online/manual/#usb)

直接将 USB 线缆插入到键盘的 USB 接口，键盘即自动切换至 USB 模式，使用 USB 输入。

### 2.4G接收器模式[¶](https://wiki.glab.online/manual/#24g_1)

将 USB 线缆插入到键盘的 USB 接口，将键盘2.4G无线模式切换至 2.4G接收模式，此时当前键盘可作为其他2.4G键盘的接收器使用。

### 有线 / 无线状态切换[¶](https://wiki.glab.online/manual/#_8)

同时按下Lshift+Rshift+M可以在USB / 无线 状态之间切换，如未插入USB线，此切换功能无效。

USB连接状态下，可通过Lshift+Rshift+Q/W/E直接跳转到对应蓝牙或2.4G通道。

### 省电模式[¶](https://wiki.glab.online/manual/#_9)

在没有接入 USB 电源的情况下，为提升续航，键盘处于省电模式。

在省电模式下，所有的指示灯仅在变化后亮起 5 秒，然后熄灭。键盘在无操作 15 秒后转入慢速扫描模式，无操作 600 秒后自动睡眠。（前述三个时间均支持通过配置工具自定义）

慢速扫描模式下，按键检测将可能略有延迟。在慢速扫描模式下按任意按键即可恢复到快速扫描模式。

自动睡眠后，按下键盘的任意按键可唤醒键盘。

### 电量显示[¶](https://wiki.glab.online/manual/#_10)

电量百分比仅供参考

由于测量数据是根据电压估算，显示电量和实际可能存在误差，损耗较大的电池电量百分比可能无法达到 100%。

在新版本的 iOS、安卓和 Windows 上，应该能够正常的显示蓝牙设备的电量百分比。

对于安卓手机，如果没有正常的显示电量，可以尝试下载 BatON 软件来获取蓝牙设备电量。

如USB与2.4G无线连接状态，或其他无法显示电量的情况，可以通过Lshift+Rshift+H输出电量

通过Lshift+Rshift+H输出电量时，N代表未能检测到数值，F代表电池电量满，数值代表电量百分比。

新版固件支持通过[键盘控制面板](https://wiki.glab.online/control/)显示电量

### 全键无冲（NKRO）[¶](https://wiki.glab.online/manual/#nkro)

此键盘支持 NKRO（全键无冲）模式。USB、蓝牙、2.4G模式均支持全键无冲模式。

### 唤醒按键[¶](https://wiki.glab.online/manual/#_11)

启用`唤醒按键`功能后，手动睡眠后唤醒需按Space+U唤醒键盘。
自动睡眠时，不需要按唤醒按键，可任意键唤醒。

默认不启用`唤醒按键`功能，可通过Lshift+Rshift+I启用或禁用。

### DFU 模式[¶](https://wiki.glab.online/manual/#dfu)

说明

采用nRF52810/nRF52811芯片的键盘,由于空间不足不支持DFU模式，请采用线刷方式更新固件

DFU模式为键盘内置的蓝牙刷机模式，在通过手机nRF Connect程序升级时，必须确保在蓝牙通讯模式，并进入DFU模式。

同时按下Lshift+Rshift+B可以重启到DFU刷机模式。也可长按PCB背部多功能按钮4秒以上后（不超过9秒）松手重启到DFU刷机模式。

### USB ISP 模式[¶](https://wiki.glab.online/manual/#usb-isp)

USB ISP 模式为USB芯片的烧录模式，在更新USB固件时需要先进入USB ISP模式。

短接PCB背部K1位置两个焊盘，并插入USB线可手动进入USP ISP模式;

用烧录工具烧录USB固件时，烧录工具会自动跳转USB ISP模式，无需手动短接K1。

### 键盘控制面板[¶](https://wiki.glab.online/manual/#_12)

[键盘控制面板](https://wiki.glab.online/control/)是一个网页应用程序，可通过网页程序控制键盘的各种功能。

包括获取键盘各种信息、控制键盘功能、更改灯光和层级、更改输出模式等。

键盘需2024年12月7日后固件支持接入键盘控制面板，接收器需2025年1月4日后固件支持接入键盘控制面板。

[键盘控制面板](https://wiki.glab.online/control/)支持作为网页程序进行安装，安装后支持离线使用。

### 轴灯版PCB[¶](https://wiki.glab.online/manual/#pcb)

轴灯版PCB采用键盘主控直接驱动RGB灯，所以无复杂灯效，仅有单色常亮、单色呼吸和彩虹循环，可手动调色，可视为单色轴灯的增强版本。

轴灯分为轴灯模式和指示灯模式，出厂默认为指示灯模式，可通过Lshift+Rshift+L 或 通过配置工具设置一颗状态灯开关按键 在指示灯模式和轴灯模式之间切换。

指示灯模式时，不可控制，以不同颜色指示不同状态，详细参考 [状态提示灯说明](https://wiki.glab.online/manual/#状态提示灯说明)

要控制轴灯，请先切换到轴灯模式。轴灯模式时，采用Lshift+Rshift+Z X C V等RGB控制功能调整轴灯。

### RGB轴灯版PCB[¶](https://wiki.glab.online/manual/#rgbpcb)

RGB轴灯版PCB采用WS2812 RGB灯，支持各种丰富绚丽的灯效（同QMK灯效），支持键盘状态指示灯、USB与蓝牙状态指示灯。

可采用Lshift+Rshift+Z X C V等RGB控制功能调整RGB轴灯。

或 接入配置工具，找到 灯光 功能，将RGB阵列相关按键设定到你指定的按键上控制RGB灯光。

RGB轴灯同时兼容指示灯，指示灯可通过配置工具设置一颗`状态灯开关`按键进行开关

指示灯支持独立运行，建议在使用电池时，关闭RGB轴灯，开启指示灯，指示灯将可自动关闭节能

## 系统功能按键说明[¶](https://wiki.glab.online/manual/#_13)

<details class="tip" open="open" style="box-sizing: inherit; background-color: var(--md-admonition-bg-color); border: 0.075rem solid rgb(0, 191, 165); border-radius: 0.2rem; box-shadow: var(--md-shadow-z1); color: rgba(0, 0, 0, 0.87); display: flow-root; font-size: 0.64rem; margin: 1.5625em 0px; padding: 0px 0.6rem; break-inside: avoid; transition: box-shadow 125ms; overflow: visible; font-family: Roboto, -apple-system, &quot;system-ui&quot;, Helvetica, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: border-box; background-color: rgba(0, 191, 165, 0.1); border-top: none; border-right: none; border-bottom: none; border-left: 0.2rem none; border-image: initial; font-weight: 700; margin: 0px -0.6rem; padding: 0.4rem 1.8rem 0.4rem 2rem; position: relative; cursor: pointer; display: block; min-height: 1rem; overflow: hidden; border-top-left-radius: 0.1rem; border-top-right-radius: 0.1rem; -webkit-tap-highlight-color: transparent; outline: none;">特别提示</summary><ul style="box-sizing: border-box; margin-bottom: 0.6rem; margin-top: 1em; list-style-type: disc; padding: 0px; margin-left: 0.625em; display: flow-root;"><li style="box-sizing: inherit; margin-bottom: 0.5em; margin-left: 1.25em;">默认采用<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Lshift</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Rshift</kbd>做为功能键触发按键</li><li style="box-sizing: inherit; margin-bottom: 0.5em; margin-left: 1.25em;">可以通过自行编译固件使用<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Lctrl</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Rctrl</kbd><span>&nbsp;</span>或<span>&nbsp;</span><kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Win</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">ESC</kbd>等做为触发按键。</li><li style="box-sizing: inherit; margin-bottom: 0.5em; margin-left: 1.25em;">更改后下表所列功能按键需做相应调整，如触发按键更改为<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Win</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">ESC</kbd>，则按下<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Win</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">ESC</kbd>+<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">P</kbd>为睡眠。</li><li style="box-sizing: inherit; margin-bottom: 0.5em; margin-left: 1.25em;">系统功能按键为内置功能，不因修改了按键而消失，只要您确保<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">LShift</kbd>/<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">RShift</kbd>等功能触发按键存在即可使用</li><li style="box-sizing: inherit; margin-bottom: 0.5em; margin-left: 1.25em;">PAD数字键盘等较少按键的键盘，没有<kbd style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: var(--md-code-font-family); color: var(--md-default-fg-color); direction: ltr; font-variant-ligatures: none; background-color: var(--md-typeset-kbd-color); border-radius: 0.1rem; box-shadow: 0 .1rem 0 .05rem var(--md-typeset-kbd-border-color),0 .1rem 0 var(--md-typeset-kbd-border-color),0 -.1rem .2rem var(--md-typeset-kbd-accent-color) inset; display: inline-block; font-size: 0.75em; padding: 0px 0.666667em; vertical-align: text-top; word-break: break-word;">Shift</kbd>等按键,下列系统功能可以通过配置工具-层级/功能-键盘功能进行设定。</li><li style="box-sizing: inherit; margin-bottom: 0px; margin-left: 1.25em;">新版固件也支持通过<a href="https://wiki.glab.online/control/" style="box-sizing: inherit; -webkit-tap-highlight-color: transparent; color: var(--md-typeset-a-color); text-decoration: none; word-break: break-word; transition: color 125ms;">键盘控制面板</a>控制各种系统功能</li></ul></details>

| 功能                      | 按键                        | 功能说明                                                     |
| :------------------------ | :-------------------------- | :----------------------------------------------------------- |
| 睡眠                      | Lshift+Rshift+P             | 手动进入睡眠模式，按任意键可以唤醒。                         |
| 关机                      | Lshift+Rshift+~或Backspace  | 手动进入关机模式。新版固件将ESC更换成了Backspace。 关机后需要插入USB线 或 短按背部多功能按钮开机。 长期不用或携带外出建议关机。 |
| 重启                      | Lshift+Rshift+N             | 手动重启键盘，用于遇到部分故障后重启修正。                   |
| 有线/无线状态切换         | Lshift+Rshift+M             | 在USB有线和无线同时工作时，可以切换有线/无线连接模式。 如未同时使用USB有线和无线，按键无效。 |
| 无线模式切换              | Lshift+Rshift+U             | 在键盘支持BLE5.0与2.4G无线两种无线模式时，在两种模式之间切换。 |
| 无线收发模式切换          | Lshift+Rshift+J             | 最新固件中，当处于2.4G无线模式时，可在2.4G接收 [接收器] 和2.4G发送 [键盘] 两种2.4G无线模式之间切换。 |
| 切换蓝牙设备/无线接收器   | Lshift+Rshift+Q/W/E         | 可以在已配对的蓝牙设备/无线接收器之间进行切换，Q/W/E代表不同蓝牙连接通道/无线接收器 |
| 重启蓝牙广播 2.4G无线配对 | Lshift+Rshift+R             | 蓝牙模式：重新开启蓝牙广播，用于手动进行连接或切换设备后配对。 2.4G无线模式：发起配对通讯，与2.4G接收器进行配对。 |
| 进入DFU                   | Lshift+Rshift+B             | 重启到DFU刷机模式。 也可长按PCB背部多功能按钮4秒以上后松手重启到DFU刷机模式。 |
| 清空当前配对              | Lshift+Rshift+O             | 蓝牙模式：清空当前蓝牙设备配对信息。仅清空当前设备，其余配对设备不会清空。 2.4G无线模式：解除与当前2.4G接收器的配对 |
| 输出电量                  | Lshift+Rshift+H             | 通过键盘输出当前键盘剩余电量。输出N为检测未稳定，F为满电，数字为电量百分比。 |
| 状态灯开关                | Lshift+Rshift+L             | 无轴灯PCB/RGB轴灯PCB：开启或关闭状态指示灯显示（注：不包括键盘大小写等）。 轴灯版PCB：使轴灯在轴灯模式和指示灯模式之间切换 |
| 多功能按钮                | PCB背部按钮属于多功能按钮。 | 键盘正常模式下，按下1秒以上后（不超过3秒）松手键盘关机； 按下4秒以上后（不超过9秒）松手键盘进入DFU刷机模式； 按下10秒以上后松手，键盘将重置。 关机状态短按一下开机。 |
| 唤醒按键                  | Space+U                     | 启用`唤醒按键`功能后，手动睡眠后唤醒需按Space+U唤醒键盘。 自动睡眠时，不需要按唤醒按键，可任意键唤醒。 |
| 切换唤醒按键              | Lshift+Rshift+I             | 启用或禁用`唤醒按键`功能。 启用`唤醒按键`功能后，手动睡眠后唤醒需按Space+U唤醒键盘 |
| 切换默认层                | Lshift+Rshift+数字键        | Lshift+Rshift+1切换默认层到第2层。 Lshift+Rshift+0切换默认层到第1层。 睡眠或关机后唤醒自动恢复第1层为默认层 |
| RGB调整颜色               | A/S/D/F/C/V                 | 轴灯版（轴灯模式下）为控制轴灯；非轴灯版为控制底灯 同时按下Lshift+Rshift时按A/S/D/F/C/V， 分别是增加饱和度、降低饱和度、增加亮度、降低亮度、增加色调、降低色调。 |
| RGB灯效循环               | Lshift+Rshift+Z             | RGB轴灯版为控制RGB轴灯在灯效之间循环 轴灯版（轴灯模式下）为控制轴灯在常亮、呼吸、多彩变换之间循环 非轴灯版为控制底灯在32种灯效之间循环 |
| RGB灯开关                 | Lshift+Rshift+X             | RGB轴灯版为RGB轴灯开关 轴灯版（轴灯模式下）为轴灯开关 非轴灯版为RGB底灯开关 |

## 配对蓝牙设备[¶](https://wiki.glab.online/manual/#_14)

#### 初次配对[¶](https://wiki.glab.online/manual/#_15)

1. 键盘上按下Lshift+Rshift+Q，切换到蓝牙通道1️⃣，然后再按下Lshift+Rshift+R开启广播；
2. 在需要连接键盘的设备上搜索蓝牙键盘，搜索到相应蓝牙键盘名称后，将第一个设备配对到键盘蓝牙通道1️⃣（Q键）；
   - 各个操作系统 [配对蓝牙键盘](https://www.baidu.com/s?wd=配对蓝牙键盘) 的方式请自行搜索相关文档
   - 如果仅有一个蓝牙设备，可以配对到任意蓝牙通道，但是建议配对到蓝牙1️⃣
   - 如不需要多设备切换，可不再配对设备到蓝牙通道2️⃣和蓝牙通道3️⃣
3. 键盘上按下Lshift+Rshift+W，切换到蓝牙通道2️⃣，然后再按下Lshift+Rshift+R开启广播，再配对第二个设备到键盘蓝牙通道2️⃣（W键 ）；
4. 键盘上按下Lshift+Rshift+E，切换到蓝牙通道3️⃣，然后再按下Lshift+Rshift+R开启广播，再配对第三个设备到键盘蓝牙通道3️⃣（E键 ）；
5. 通过Lshift+Rshift+Q/W/E切换到不同的蓝牙通道，可在多个配对设备之间切换。

#### 更换配对[¶](https://wiki.glab.online/manual/#_16)

如需更换某个设备的配对，例：要重新配对一个设备到蓝牙通道2️⃣（W键 ）。

1. 将W键配对的设备上的蓝牙键盘进行删除（如windows系统进入系统设备删除蓝牙键盘；Android系统进入设置-蓝牙后删除蓝牙键盘）；
2. 键盘上按下Lshift+Rshift+W，切换到蓝牙通道2️⃣；
3. 键盘上再按下Lshift+Rshift+O，删除蓝牙通道2️⃣配对的设备；
4. 如有必要，按下Lshift+Rshift+R开启广播；
5. 在需要配对键盘的设备上搜索蓝牙键盘，搜索到相应蓝牙键盘名称后，将新的设备配对到键盘蓝牙通道2️⃣（W键 ）；

Tip

上述切换蓝牙通道及删除配对设备等，也可通过BT 1 / BT 2 / BT 3 / 无线 解除配对 等按键进行操作

采用BT 1 / BT 2 / BT 3按键切换蓝牙通道后，需要按下无线 广播配对手动开启蓝牙广播

## 2.4G接收器配对[¶](https://wiki.glab.online/manual/#24g_2)

1. 将接收器插入电脑USB口通电
2. 按下Lshift+Rshift+U 将键盘切换到2.4G无线模式
3. 键盘未配对：可多次按下Lshift+Rshift+R启动配对通讯，直到正确连接输出按键
4. 键盘已配对：按下Lshift+Rshift+O删除配对信息，可多次按下Lshift+Rshift+R启动配对通讯，直到正确连接输出按键
5. 配对多个接收器：按下Lshift+Rshift+Q/W/E切换不同的无线通道，然后重复执行2-4三个步骤完成与不同接收器的配对
6. 通过Lshift+Rshift+Q/W/E切换到不同的无线通道，可在多个配对的无线接收器之间切换。
7. 如PAD等小键盘，无Lshift、Rshift，可通过配置工具配置切换 无线模式/无线 广播配对/无线 解除配对/BT 1 / BT 2 / BT 3按键，或通过键盘控制面板，启动配对通讯、删除配对信息、切换无线接收器

Tip

配对前请确保键盘固件更新到支持三模固件的版本，并确保接收器和键盘的固件版本对应。

## 状态提示灯说明[¶](https://wiki.glab.online/manual/#_17)

GT系列无线键盘无灯版指示灯一般为3颗，每个键盘指示灯的位置稍有不同，但是指示逻辑基本一致：

- 蓝色灯-蓝牙连接成功、蓝牙输出
- 绿色灯-USB输出
- 红色灯-2.4G无线输出
- 蓝色指示灯闪烁-蓝牙通道1️⃣广播中
- 绿色指示灯闪烁-蓝牙通道2️⃣广播中
- 红色指示灯闪烁-蓝牙通道3️⃣广播中
- USB连接状态下，状态灯常亮
- 蓝牙连接状态下，指示灯5秒后自动熄灭（可自定义常亮时长）
- 蓝牙广播时闪烁30秒后未连接自动熄灭。

GT系列无线键盘单色RGB轴灯版默认采用轴灯作为指示，不同颜色代表不同状态：

- 蓝色-蓝牙连接成功、蓝牙输出
- 绿色-USB输出
- 青色-2.4G无线输出
- 粉色-蓝牙通道1️⃣广播中
- 黄色-蓝牙通道2️⃣广播中
- 红色-蓝牙通道3️⃣广播中
- USB连接状态下，状态灯常亮
- 蓝牙连接状态下，指示灯5秒后自动熄灭（可自定义常亮时长）
- 蓝牙广播30秒后未连接自动熄灭。

GT系列无线键盘炫彩RGB轴灯版默认采用某颗轴灯作为指示，不同颜色代表不同状态：

- 绿色-USB输出
- 蓝色-蓝牙通道1️⃣输出
- 红色-蓝牙通道2️⃣输出
- 橙色-蓝牙通道3️⃣输出
- 青色-2.4G无线1️⃣输出
- 紫色-2.4G无线2️⃣输出
- 粉色-2.4G无线3️⃣输出
- 黄色-2.4G无线接收器接收模式启用
- 指示灯可通过配置工具设置一颗`状态灯开关`按键进行开关
- 指示灯支持独立运行，建议在使用电池时，关闭RGB轴灯，开启指示灯，指示灯将可自动关闭节能

## 关于充电[¶](https://wiki.glab.online/manual/#_18)

- PCB自带锂电池充电功能，可通过USB连线进行充电。
- 可插入电脑USB口充电，也可插入5V电源适配器充电。
- 最常见的5V电源适配器就是手机充电头，但是不建议采用快充头进行充电，因为快充头支持9V/12V等较高电压输出，可能导致烧毁PCB。
- 锂电池充电电流最大支持400mah左右（老版200mah），1000mah电池大概需要3小时左右能充满。

## 关于续航[¶](https://wiki.glab.online/manual/#_19)

- 无轴灯版本，1000mah电池可使用约90天（每日8小时）
- 新版带编码器与RGB矩阵灯的版本，1000mah电池可使用约80天（每日8小时）[1](https://wiki.glab.online/manual/#fn:1)
- RGB轴灯耗电量非常大，开启轴灯后1000mah电池可使用仅约5天。[2](https://wiki.glab.online/manual/#fn:2)
- 具体续航时间因硬件及使用环境等差异而不同，如电池实际容量（排除虚标）大小及损耗情况，无线信号强度等均对续航有影响

## 硬件运作方式[¶](https://wiki.glab.online/manual/#_20)

硬件上采用nRF52系列芯片+CH55x芯片协同实现的USB、蓝牙、2.4G无线功能

- nRF52系列芯片当前支持nRF52810/nRF52811/nRF52832三款芯片
- CH55x芯片主要使用CH552T或CH554T，也支持同系列的CH552G等芯片
- nRF52主要负责 运行键盘主控程序、蓝牙通讯、2.4G通讯、灯光控制，是主功能芯片。
- CH55x主要负责 USB通讯，包括转发nRF52数据到USB主机，转发配置数据到nRF52，烧录nRF52芯片固件

本页面最后修改时间2025-06-09

本页面操作说明全系列键盘通用，所有内容基于最新版固件，如与当前键盘实际不符，建议升级固件 更多键盘专有说明可以访问键盘[产品信息](https://wiki.glab.online/keyboard/volta9/)页面