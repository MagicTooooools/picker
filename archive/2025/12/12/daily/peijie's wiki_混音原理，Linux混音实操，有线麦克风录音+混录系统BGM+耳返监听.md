---
title: 混音原理，Linux混音实操，有线麦克风录音+混录系统BGM+耳返监听
url: https://liupj.top/2025/12/12/mix-audio/
source: peijie's wiki
date: 2025-12-12
fetch_date: 2025-12-13T03:20:55.453043
---

# 混音原理，Linux混音实操，有线麦克风录音+混录系统BGM+耳返监听

[peijie's wiki](/)

* [Home](/)
* [Contact](/contact)
* [GitHub](https://github.com/brannua)
* [Travelling](https://travellings.link)
* [WormHole](https://foreverblog.cn/go.html)
* [RSS](/atom.xml)

# 混音原理，Linux混音实操，有线麦克风录音+混录系统BGM+耳返监听

2025-12-12

背景知识：
在Linux音频系统中，“输出设备” = 这个设备可以接收声音，然后播放出来，并非声音的生产者，只是声音的播放者
而“输入设备”负责采集声音，是声音的起点，声音从输入设备开始传播

如此一来，就不难理解TUI工具pulsemixer的output标签下的内容，output即输出设备，下含
1.有线麦克风（因为我的这个麦克风有3.5mm耳机孔，用于播放声音，所以是输出设备）
2.电脑内置声卡（能接收声音然后播放<无论是接耳机还是扬声器>，当然是输出设备）
3.虚拟混音池（接收音乐播放器的声音、接收麦克风的声音；播放给录音软件和你的耳朵<耳返监听>；当然是输出设备）

也不难理解TUI工具pulsemixer的input标签下的内容，input即输入设备，下含
1.有线麦克风
2.电脑内置声卡
～～
每个输出设备都有一个对应的监听口，这些监听口都是输入设备，即Monitor of …
简言之：每个“喇叭”都有个“窃听器”，录音软件不用接喇叭，只需要接窃听器，就能录到喇叭正在播放的声音

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 ``` | ``` #!/bin/bash  # ======================================= # 音乐（电脑播放的）-> 流入水池，即：采集BGM #  # 你的人声 -> 流入水池，即：采集人声 #  # 水池有俩出水口： #  # 出水口A -> SimpleScreenRecorder 录音，即：同时录到BGM和人声 #  # 出水口B -> 耳机，即：耳机里能同时听到BGM和人声 # =======================================  echo "=== 查找音频设备 ==="  UGREEN_IN=$(pactl list short sources | rg input | head -1 | awk '{print $2}') echo "你的输入设备：$UGREEN_IN"  PC_OUT=$(pactl list short sinks | rg "analog-stereo" | tail -1 | awk '{print $2}') echo "你的输出设备：$PC_OUT"  echo "=== 创建虚拟混音池 ==="  pactl unload-module module-null-sink 2>/dev/null pactl load-module module-null-sink sink_name=record_pool  echo "=== 正在配置音频流 ==="  pactl set-default-sink record_pool  pactl unload-module module-loopback 2>/dev/null pactl load-module module-loopback source="$UGREEN_IN" sink=record_pool latency_msec=5  pactl load-module module-loopback source=record_pool.monitor sink="$PC_OUT" latency_msec=5  echo "✅配置完成！" echo "在 Simple Screen Recorder 中：" echo "1. 勾选 'Record audio'" echo "2. Source 选择: 'Monitor of record_pool......'" echo "3. 开始录制" ``` |

注意：latency\_msec=5 是指明的低延迟参数，因为是软件实现的监听

[Back to Top](#top)

© 2025
lpj