---
title: 初探 ESP32-CAM
url: https://5ime.cn/esp32-cam.html
source: Hi, I Am I
date: 2025-11-26
fetch_date: 2025-11-27T16:52:31.600971
---

# 初探 ESP32-CAM

[Home](/)  [Blog](/archive)  [Links](/links)  [Comment](/guestbook)  [About](/about)

[Home](/)  [Blog](/archive)  [Links](/links)  [Comment](/guestbook)  [About](/about)

# 初探 ESP32-CAM

Nov 26, 2025   | 4 min read |   [折腾](/categories/%E6%8A%98%E8%85%BE.html)   |   [硬件](/tags/%E7%A1%AC%E4%BB%B6.html)

## 文章目录

1. [1. 写在前面](#写在前面)
2. [2. IDE 配置](#ide-配置)
   1. [2.1. 配置管理器地址](#配置管理器地址)
   2. [2.2. 安装开发板支持](#安装开发板支持)
   3. [2.3. 选择开发板和端口](#选择开发板和端口)
3. [3. 示例代码配置](#示例代码配置)
   1. [3.1. 修改硬件配置](#修改硬件配置)
4. [4. 上传和运行](#上传和运行)
   1. [4.1. 查看运行信息](#查看运行信息)
5. [5. 写在后面](#写在后面)

近期博主需要在大屏展示系统中加入实时视频串流功能。由于并非硬件开发出身，上一次接触 Arduino 烧录还是在 [使用 Digispark 开发板制作 BadUSB](https://5ime.cn/digispark.html) 的时候，因此这次直接选择了集成摄像头的 ESP32-CAM 开发板来实现该需求，如下图所示：

![image-20251126184319135](https://img.5ime.cn/esp32-cam/image-20251126184319135.png)

## 写在前面

开始之前，需要先下载并安装 [CH340](https://www.wch.cn/products/CH340.html) 驱动，以确保电脑能够正常识别 ESP32-CAM。

ESP32-CAM 的工作模式主要分为 **下载模式** 和 **运行模式**：

* **烧录模式（下载模式）：**
  将 IO0 接地（接 GND），按下 RST 按钮；此时 Arduino IDE 可正常上传固件。
* **运行模式：**
  将 IO0 悬空/不接地，再按下 RST 按钮；ESP32-CAM 将运行已烧录的程序。

## IDE 配置

### 配置管理器地址

打开 Arduino IDE，进入 `文件` -> `首选项`，在 `其他开发板管理器地址` 中添加以下内容（此处采用乐鑫官方提供的 Jihulab 国内镜像）

```
https://jihulab.com/esp-mirror/espressif/arduino-esp32/-/raw/gh-pages/package_esp32_index_cn.json
```

![开发板管理器地址配置](https://img.5ime.cn/esp32-cam/image-20251125201633607.png)

### 安装开发板支持

在 `工具` -> `开发板` 中选择 `开发板管理器`，或者在左侧导航栏中选择 `开发板管理器`，搜索 `esp32` 并安装 **2.0.16** 版本。（推荐使用 2.0.x 系列，3.0.x 版本当前可能存在兼容性问题）

![ESP32 开发板安装](https://img.5ime.cn/esp32-cam/image-20251125195003947.png)

### 选择开发板和端口

选择电脑识别到的 `CH340` 串口（如 COM3/COM5），并将开发板型号设置为 `AI Thinker ESP32-CAM`。

![端口和开发板选择](https://img.5ime.cn/esp32-cam/image-20251125195145307.png)

## 示例代码配置

成功选择开发板后，在 `文件` -> `示例` 中即可看到对应示例，选择 `CameraWebServer`。

![CameraWebServer 示例](https://img.5ime.cn/esp32-cam/image-20251125195212463.png)

### 修改硬件配置

在示例代码中找到摄像头型号配置部分，将 `CAMERA_MODEL_ESP_EYE` 注释掉，并取消注释 `CAMERA_MODEL_AI_THINKER`，以匹配开发板的硬件型号。

随后，将 `ssid` 和 `password` 修改为你自己的 Wi-Fi 信息。

![硬件型号配置](https://img.5ime.cn/esp32-cam/image-20251125195816580.png)

## 上传和运行

配置完成后，点击 `验证` 确保代码无误，再点击 `上传` 进行烧录。

![验证和上传](https://img.5ime.cn/esp32-cam/image-20251125200335907.png)

### 查看运行信息

打开右上角的 `串口监视器`，选择 `115200` 波特率后，按下 ESP32-CAM 的 `RST` 按钮进入运行模式，即可看到串口输出信息。

![串口监视器](https://img.5ime.cn/esp32-cam/image-20251125200430038.png)

随后，在浏览器访问串口中输出的 IP 地址，点击 `Start Stream` 即可看到实时视频画面：

![摄像头画面](https://img.5ime.cn/esp32-cam/image-20251125200651877.png)

通过开发者工具可以看到串流地址如下所示，可以嵌入到其他想要展示画面的地方：

```
<img id="stream" src="http://192.168.137.141:81/stream" crossorigin="">
```

## 写在后面

最后附上一版博主基于官方 CameraWebServer 示例优化的项目：[5ime/ESP32CamPlus](https://github.com/5ime/ESP32CamPlus)，加入了断网自动重连、LED 闪烁提示、亮度调节等功能。

**本文作者：** iami233

**本文链接：** <https://5ime.cn/esp32-cam/index.html>

**版权声明：** 本文采用  [CC BY-NC-SA 4.0 CN](https://creativecommons.org/licenses/by-nc-sa/4.0/cn/)  协议进行许可

**1**/0

© 2025 iami233. All rights reserved.