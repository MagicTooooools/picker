---
title: 树莓派桌面环境日常使用的若干问题记录
url: https://azhuge233.com/%e6%a0%91%e8%8e%93%e6%b4%be%e6%a1%8c%e9%9d%a2%e7%8e%af%e5%a2%83%e6%97%a5%e5%b8%b8%e4%bd%bf%e7%94%a8%e7%9a%84%e8%8b%a5%e5%b9%b2%e9%97%ae%e9%a2%98%e8%ae%b0%e5%bd%95/
source: azhuge233's
date: 2026-01-14
fetch_date: 2026-01-15T03:34:54.849642
---

# 树莓派桌面环境日常使用的若干问题记录

[Skip to content](#content)

# Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search for:

Search Submit

Search for:

Search Submit

[![](https://azhuge233.com/wp-content/uploads/2020/11/cropped-IMG_2518-150x150.jpg)](https://azhuge233.com/)

[azhuge233's](https://azhuge233.com/)

Hi

* [Github](https://github.com/azhuge233)
* [Steam](https://steamcommunity.com/id/azhuge233/)

Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search...

![](https://azhuge233.com/wp-content/uploads/2026/01/Raspberry_Pi_OS_Logo-720x240.png)

# 树莓派桌面环境日常使用的若干问题记录

[2026-01-142026-01-14](https://azhuge233.com/%E6%A0%91%E8%8E%93%E6%B4%BE%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83%E6%97%A5%E5%B8%B8%E4%BD%BF%E7%94%A8%E7%9A%84%E8%8B%A5%E5%B9%B2%E9%97%AE%E9%A2%98%E8%AE%B0%E5%BD%95/) [azhuge233](https://azhuge233.com/author/azhuge233/)

文章目录

Toggle

* [4 与 5 的部分对比](#4-%E4%B8%8E-5-%E7%9A%84%E9%83%A8%E5%88%86%E5%AF%B9%E6%AF%94)
  + [浏览器视频播放](#%E6%B5%8F%E8%A7%88%E5%99%A8%E8%A7%86%E9%A2%91%E6%92%AD%E6%94%BE)
  + [LED 状态差异](#LED-%E7%8A%B6%E6%80%81%E5%B7%AE%E5%BC%82)
  + [供电问题](#%E4%BE%9B%E7%94%B5%E9%97%AE%E9%A2%98)
* [超频](#%E8%B6%85%E9%A2%91)
* [无线鼠标问题](#%E6%97%A0%E7%BA%BF%E9%BC%A0%E6%A0%87%E9%97%AE%E9%A2%98)
* [软件](#%E8%BD%AF%E4%BB%B6)
  + [输入法](#%E8%BE%93%E5%85%A5%E6%B3%95)
  + [RDP 客户端](#RDP-%E5%AE%A2%E6%88%B7%E7%AB%AF)
  + [截图](#%E6%88%AA%E5%9B%BE)
  + [conky](#conky)

最近把吃灰派（4b、5）拿出来作为日常电脑使用，遇到了不少小问题，记录一下

# 4 与 5 的部分对比

我的 4b 和 5 都是 8GB 版本，但使用体验上 5 比 4b 流畅得多

在相同系统环境下（通过 Raspbian 自带的 SD 克隆），5 冷启动 Chromium 没有任何卡顿延迟，4 则会等待很久

## 浏览器视频播放

4 使用 Chromium 播放 B 站 1080p 视频非常卡（FireFox 比 Chromium 还糟糕），只有将分辨率调整为 720p 才能基本流畅播放全站的视频（1080p 会比较挑视频，有些会卡有些不会）

Youtube 视频比 B 站好一点，但也仅仅是一点

4 在观看 B 站直播时如果使用最高档画质（1080p 60）会非常卡顿，调整到次档会缓解，当然这也取决于主播和平台对直播间的设置

5 则没有遇到任何播放问题，而且网页加载速度也有显著提升

## LED 状态差异

4b 以及之前版本的树莓派，LED 的指示行为都是一致的，通电后如果正常启动，红色 LED 会常量，伴随绿色 LED 不规律闪烁（随 IO 状态变化）

但是树莓派 5 的默认 LED 指示行为有变化，通电后如果正常启动，红色 LED 先亮后灭，然后绿色 LED 不规则闪烁

也就是说，树莓派 5 正常启动后红色 LED 是熄灭的

之所以会注意这点，是因为发生了下文的供电问题

## 供电问题

我一开始给树莓派4、5（均已超频）供电时使用的是绿联的一款多口电源适配器，使用 C to C 线，支持 PD，这种环境下树莓派4、5 都能正常启动

但是当我切换成一个标称 5V 3A 的电源适配器（黑色塑壳看起来就很老的那种），换了一根带按钮开关的电源线（这两个都是当时商家附赠的），树莓派 5 就无法启动了

表现为：通电后红色 LED 亮一阵后熄灭，随后绿色 LED 亮一阵后也熄灭，最后红色 LED 常亮

排查后发现是线的问题（换线后 5V 3A 的电源适配器也可以启动），这根带按钮开关的线可能内阻过大，压降增加后树莓派 5 获取不到足够的电压，导致无法启动

# 超频

树莓派超频只需要编辑 /boot/firmware/config.txt，添加内容

```
# Overclocking RPi 4
over_voltage=6
arm_freq=2000
gpu_freq=750

# Overclocking RPi 5
over_voltage=6
arm_freq=3000
gpu_freq=1200
```

其中，over\_voltage 控制 CPU 和 GPU 电压（此处设置为 6 ，即加电压），arm\_freq 是 CPU 频率，gpu\_freq 是 GPU 频率

相关设置和详细说明可以查看 [Overclocking options](https://www.raspberrypi.com/documentation/computers/config_txt.html#overclocking-options)

编辑后重启，如果成功进入系统，可以通过以下指令查看频率

```
vcgencmd measure_clock arm # CPU 频率
vcgencmd measure_clock core # GPU 频率

vcgencmd measure_temp # CPU/GPU 温度
```

如果超频后无法进入系统，则需要将存储设备连接到另外一台设备，编辑 config.txt 恢复原来的内容

# 无线鼠标问题

树莓派连接部分 USB 2.4G 无线鼠标后，发现无法正常使用：

* 有些无法长按拖动，拖动时会有连续点击的效果，类似双击，但更换微动无法修复
* 有些根本无法正常移动光标，DPI 似乎变得非常小，而且移动时有严重卡顿

之所以说“部分 USB 2.4G 无线鼠标”，是因为试了一圈只有罗技 G304 能用，G304 回报率有 1000 Hz，其他鼠标的回报率没有这么高

排查一圈发现鼠标没有硬件故障，问题在于树莓派系统的鼠标回报率

解决方法是编辑 /boot/firmware/cmdline.txt，在行尾添加以下内容，然后重启

```
# 手动指定 USB 回报率
# 值可以是 0/1/2/4/8，按需调整
# 设置为 0 即使用鼠标自己的回报率
usbhid.mousepoll=2
```

Raspbian 默认的 mousepoll 是 16（来源：[Mouse Input Latency](https://github.com/dekuNukem/USB4VC/issues/6)），即 62.5 Hz （根据 [Mouse polling rate](https://wiki.archlinux.org/title/Mouse_polling_rate) 推算），手动提高 USB 设备回报率后问题消失

# 软件

## 输入法

安装 fcitx5 中文输入法

```
sudo apt install fcitx5 fcitx5-pinyin fcitx5-chinese-addons
```

安装完毕后可以启动 菜单 - 首选项 - 输入法 将默认输入法设置为 fcitx5

也可以 shell 执行 im-config 命令，唤起 GUI 界面来设置默认输入法

## RDP 客户端

在 Windows 下常使用 mstsc 远程连接 Windows 电脑，Raspbian 中可以使用 Remmina 作为平替

```
sudo apt install remmina
```

## 截图

由于在 Windows 下习惯了 Snipaste 快捷键 F1 截图，所以想在 Raspbian 下实现同样操作

系统默认只有 PrintScreen 键全屏截图（调用 grim），保存位置也固定在用户根目录

最后我使用 flameshot + labwc 的快捷键实现了 F1 区域截图（如果在官方社区搜索快捷键设置，会发现很多回复说需要编辑 .config/wayfire.ini，但实际上新版 Raspbian 默认没有使用 wayfire，换成了 labwc，所以相关设置需要符合 labwc 的标准）

安装 flameshot

```
sudo apt install flameshot
```

此时如果直接运行 flameshot，会提示 flameshot 无法检测桌面环境，建议设置 XDG\_CURRENT\_DESKTOP 环境变量

如果将该环境变量设置在 .bashrc，在 labwc 快捷键调用时会无法启动 flameshot，flameshot 还是无法读取 XDG\_CURRENT\_DESKTOP 环境变量

所以需要将该环境变量设置在 labwc 中，对应文件是 ~/.config/labwc/environment，在该文件中添加一行

```
XDG_CURRENT_DESKTOP=sway
```

这样通过快捷键启动 flameshot 时就不会报错，同时 CLI 和 GUI 也能顺利启动 flameshot

之后设置 labwc 快捷键，编辑 ~/.config/labwc/rc.xml，在 openbox\_config 标签中，添加键盘快捷键设置

```
<?xml version="1.0"?>
<openbox_config xmlns="http://openbox.org/3.4/rc">
    <!-- 其他设置 -->

    <keyboard>
        <!-- 可选：禁用原生截图快捷键 -->
        <keybind key="Print"></keybind>
        <keybind key="F1">
            <action name="Execute">
                <command>flameshot gui</command>
            </action>
        </keybind>
    </keyboard>
</openbox_config>
```

重新登录后生效

## conky

conky  可以用作系统资源实时监控程序，显示一个包含 CPU、GPU、内存相关信息的窗口

安装 conky

```
sudo apt install conky-all
```

conky 的界面配置灵活，可自定义的程度很高，相关文档：[conky.cc](https://conky.cc/)

这里贴一份我基于 [NickTheNeko/conky](https://gitlab.com/NickTheNeko/conky/-/raw/main/Main/config/conky.conf?ref_type=heads) 修改的 conky 配置文件

```
--[[
#===============================================================================================#
# Date    : package-date                                                                        #
# Author  : Nicola Bicocchi                                                                     #
# Version : package-version                                                                     #
# License : Distributed under the terms of GNU GPL version 2 or later                           #
#===============================================================================================#
# CONKY                                                                                         #
# For commands in conky.config section:                                                         #
# http://conky.sourceforge.net/config_settings.html                                                #
#                                                                                              #
# For commands in conky.text section:                                                           #
# http://conky.sourceforge.net/variables.html                    ...