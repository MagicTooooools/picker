---
title: 使用 uv 管理 Python 依赖
url: https://oldj.net/article/2025/11/29/python-with-uv
source: oldj's blog
date: 2025-11-29
fetch_date: 2025-11-30T03:29:02.582342
---

# 使用 uv 管理 Python 依赖

[# oldj's blog](/)

编程，写作，以及涂鸦

[首页](/)[文章](/article)[留言](/contact)[关于](/about)

# 使用 uv 管理 Python 依赖

2025-11-29

目录

[什么是 uv](#1-什么是 uv)

[uv 的安装和初始化](#2-uv 的安装和初始化)

[虚拟环境](#3-虚拟环境)

[安装依赖](#4-安装依赖)

[在 docker 中使用 uv](#5-在 docker 中使用 uv)

[小结](#6-小结)

我有一个运行了好几年的 Django 项目，之前一直在使用默认的 pip 管理和安装依赖，最近切换到了 [uv](https://github.com/astral-sh/uv)，感觉还不错，在这儿记录一下。

## 什么是 uv

根据官网的介绍，uv 是一个 Python 包以及项目管理器，非常快，使用 Rust 开发。

安装和管理依赖只是它的功能之一，除此之外，它还可以创建虚拟环境，即可以取代 pip + virtualenv 的功能。

我测试了一下，uv 确实比 pip 快了很多。在使用相同的镜像源，且都是纯净的 docker 环境下，使用 pip 安装项目的依赖花了约 88 秒，但使用 uv 只用了 13 秒。

不过，安装依赖并不是一个高频操作，多花一点时间一般不是什么痛点，uv 更吸引人的是它简化了很多工作，比如内置了 Python 多版本安装以及虚拟环境管理，且能保证环境的可复现性，这就让 Python 项目的开发和发布工作简单了很多。

## uv 的安装和初始化

在 macOS 或 Linux 上，可以使用以下命令安装 uv：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows 上的命令如下：

```
# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

安装完成之后，可以使用以下命令初始化一个新项目：

```
uv init my-project
```

如果你的项目已经存在，也可以直接进入项目根目录，执行以下命令：

```
uv init
```

如果项目中已经有 `requirements.txt`，想改用 uv 进行管理，可以在项目根目录执行以下命令：

```
uv pip install -r requirements.txt
```

也可以直接执行：

```
uv sync
```

初始化后，uv 会在项目根目录下生成一个 `pyproject.toml` 文件，其中包含了项目的基本信息以及依赖项。

可以运行以下命令，锁定依赖项：

```
uv lock
```

这个命令将会在项目根目录下生成 `uv.lock` 文件，类似 Node.js 的 `package-lock.json`，其中包含了项目依赖的各个第三方库以及版本号，确保下次安装时安装的包相同。

## 虚拟环境

Python 默认是安装在系统中的，使用 pip 安装依赖时，默认也会全局安装，在很多情况下，尤其是需要维护多个项目时，这显然不是我们期望的，此时可以使用虚拟环境。

在 uv 中使用虚拟环境很简单。

首先，可以使用 uv 安装多个不同版本的 Python：

```
uv python install 3.10 3.11 3.12
```

然后，可以通过类似下面的命令安装虚拟环境：

```
uv venv --python 3.12.0
```

可以直接在项目根目录下执行这个命令，执行成功之后，项目根目录下会生成一个 `.venv` 文件夹，包含这个环境的所有信息，之后安装的包也会保存在这个文件夹下，记得将这个文件夹添加到 `.gitignore` 中。

如果你熟悉 Node.js，会发现这个 `.venv` 文件夹和 Node.js 的 `node_modules` 文件夹功能类似，且它更进一步，不仅包含依赖，还能包含当前项目所需的 Python 本身。

使用以下命令可以用虚拟环境中的 Python 执行指定脚本：

```
uv run example.py
```

如果你在终端中访问项目，可以使用以下命令激活当前 Python 虚拟环境：

```
source .venv/bin/activate
```

激活虚拟环境之后，可以直接用类似 `python example.py` 的方式来运行项目中的脚本。

使用现代 IDE（比如 PyCharm、VSCode 等）打开这个项目时，IDE 一般都能自动识别项目中的 `.venv` 虚拟环境。

## 安装依赖

配置好环境后，就可以使用类似下面的命令安装依赖了：

```
uv add django
```

这信命令会下载对应的包并安装在 `.venv`中，安装成功之后，会修改 `pyproject.toml` 和 `uv.lock` 文件。

如果你刚将代码从仓库中拉到本地，项目中已经有了 `pyproject.toml` 和 `uv.lock`，那么只需执行以下命令即可安装所有依赖：

```
uv sync
```

注意，这个命令会根据 `pyproject.toml` 解析和下载依赖，有可能会改进 `uv.lock`。如果是在生产环境，你希望**严格**按照 `uv.lock` 中的版本安装依赖，可以使用以下命令：

```
uv sync --frozen
```

## 在 docker 中使用 uv

如果你的项目需要使用 docker 发布，还有一些额外需要注意的事项。

如果你的服务器在国内，那么可能需要使用国内 pypi 镜像，uv 中要指定镜像很简单，设置相应的环境变量即可，例如下面设置使用了阿里云的镜像：

```
ENV UV_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/
ENV UV_TRUSTED_HOST=mirrors.aliyun.com
```

在 docker 中安装依赖时，大体上有两种方式，一种是使用 `uv sync` 命令，如下所示：

```
WORKDIR /code/
COPY pyproject.toml uv.lock /code/

# 安装依赖
RUN uv sync --frozen --no-dev

ENV PATH="/code/.venv/bin:$PATH"
```

这种方式会在当前目录下创建虚拟环境，所有依赖都将安装到 `.venv` 目录下，因此需要将对应的目录加入 `PATH`。这种方式安装的依赖将严格遵守 `uv.lock` 中的版本限制，最为可靠。

或者，也可以选择将依赖直接安装到系统环境中：

```
RUN uv pip install --no-cache-dir --system .
```

注意那个 `--system` 参数，这种方式会将各依赖包安装到全局目录，如果你的 docker 中只有这一个 Python 项目，且不想使用虚拟环境，也可以使用这种方式安装。不过，这种方式安装时虽然也会参考 `uv.lock`，但并不保证各依赖的版本和 `uv.lock` 中严格相同。

## 小结

Python 发布迄今已有三十余年，一开始并没有第三方包的安装和管理工具，这和 Node.js 一发布就自带 npm 不同。如果你使用 Python 的时间较早，可能还会记得曾经有一个叫 easy\_install 的工具用于安装 Python 的第三方包。

约 2008 年，pip 发布，随后在 2014 年被 Python 官方集成到 3.4 版中（以及 2.7.9 中），Python 这才有了一个官方的依赖管理工具。不过 pip 并不完美，主要是依赖解析能力较弱，无法保证每次安装后的环境完全相同，同时安装速度也有一些慢，因此后续又出现了一些新的依赖管理工具，比如 poetry、uv 等。

目前，开发 Python 项目的最佳实践是为每个项目创建独立的虚拟环境，将项目所需的依赖安装在该环境中，并通过依赖管理文件记录依赖，以确保隔离、可复现和可移植性。这些工作都可以使用 uv 完成，如果你正在开发或维护一个 Python 项目，不妨试试 uv。

**分类：**[编程](/article/list?category=%E7%BC%96%E7%A8%8B)**标签：**[Python](/article/list?tag=Python)

前一篇[富文本框架体验](/article/2025/06/21/rich-text-editor-experience)

后一篇无

## 相关文章：

* [使用 Python 在内存中生成 zip 文件](/article/2011/04/14/in-memory-zip-in-python)
* [Python 循环中的 else 语句](/article/2010/06/20/python-loop-else)
* [黑客的统计学](/article/2016/06/14/statistics-for-hackers)

## 评论：

暂无评论

## 发表评论：

电子邮件地址不会被公开。必填项已用 \* 标注。

评论 \*你的称呼 \*电子邮件 \*站点（选填）验证码 \*（不区分大小写）提交评论

### 分类

* [涂鸦 (23)](/article/list?category=%E6%B6%82%E9%B8%A6)
* [小故事 (2)](/article/list?category=%E5%B0%8F%E6%95%85%E4%BA%8B)
* [阅读 (16)](/article/list?category=%E9%98%85%E8%AF%BB)
* [文章 (112)](/article/list?category=%E6%96%87%E7%AB%A0)
* [编程 (94)](/article/list?category=%E7%BC%96%E7%A8%8B)

搜索

[可通过 **RSS** 订阅本站](/feed)

### 标签

[JavaScript (46)](/article/list?tag=JavaScript)[随笔 (34)](/article/list?tag=%E9%9A%8F%E7%AC%94)[Python (33)](/article/list?tag=Python)[我读 (24)](/article/list?tag=%E6%88%91%E8%AF%BB)[涂鸦 (23)](/article/list?tag=%E6%B6%82%E9%B8%A6)[数学 (19)](/article/list?tag=%E6%95%B0%E5%AD%A6)[纯属娱乐 (17)](/article/list?tag=%E7%BA%AF%E5%B1%9E%E5%A8%B1%E4%B9%90)[生活 (15)](/article/list?tag=%E7%94%9F%E6%B4%BB)[HTML5 (12)](/article/list?tag=HTML5)[科幻 (10)](/article/list?tag=%E7%A7%91%E5%B9%BB)[Electron (9)](/article/list?tag=Electron)[算法 (8)](/article/list?tag=%E7%AE%97%E6%B3%95)[趣题 (8)](/article/list?tag=%E8%B6%A3%E9%A2%98)[产品 (8)](/article/list?tag=%E4%BA%A7%E5%93%81)[工具 (7)](/article/list?tag=%E5%B7%A5%E5%85%B7)[网页游戏 (6)](/article/list?tag=%E7%BD%91%E9%A1%B5%E6%B8%B8%E6%88%8F)[数据分析 (6)](/article/list?tag=%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90)[概率 (6)](/article/list?tag=%E6%A6%82%E7%8E%87)[Django (6)](/article/list?tag=Django)[博客系统 (6)](/article/list?tag=%E5%8D%9A%E5%AE%A2%E7%B3%BB%E7%BB%9F)[统计 (6)](/article/list?tag=%E7%BB%9F%E8%AE%A1)[SwitchHosts (6)](/article/list?tag=SwitchHosts)[NodeJS (6)](/article/list?tag=NodeJS)[迷宫 (5)](/article/list?tag=%E8%BF%B7%E5%AE%AB)[热力图 (5)](/article/list?tag=%E7%83%AD%E5%8A%9B%E5%9B%BE)[网站分析 (5)](/article/list?tag=%E7%BD%91%E7%AB%99%E5%88%86%E6%9E%90)[宇宙 (5)](/article/list?tag=%E5%AE%87%E5%AE%99)[我看 (5)](/article/list?tag=%E6%88%91%E7%9C%8B)[漫画 (4)](/article/list?tag=%E6%BC%AB%E7%94%BB)[转摘 (4)](/article/list?tag=%E8%BD%AC%E6%91%98)[博弈 (4)](/article/list?tag=%E5%8D%9A%E5%BC%88)[方法 (4)](/article/list?tag=%E6%96%B9%E6%B3%95)[幂律 (4)](/article/list?tag=%E5%B9%82%E5%BE%8B)[A-B测试 (4)](/article/list?tag=A-B%E6%B5%8B%E8%AF%95)[遗传算法 (3)](/article/list?tag=%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95)[混沌 (3)](/article/list?tag=%E6%B7%B7%E6%B2%8C)[分形 (3)](/article/list?tag=%E5%88%86%E5%BD%A2)[matplotlib (3)](/article/list?tag=matplotlib)[VIM (3)](/article/list?tag=VIM)[用户体验 (3)](/article/list?tag=%E7%94%A8%E6%88%B7%E4%BD%93%E9%AA%8C)[DevOps (3)](/article/list?tag=DevOps)[AI (3)](/article/list?tag=AI)[效率 (2)](/article/list?tag=%E6%95%88%E7%8E%87)[质数 (2)](/article/list?tag=%E8%B4%A8%E6%95%B0)[物理 (2)](/article/list?tag=%E7%89%A9%E7%90%86)[Zipf (2)](/article/list?tag=Zipf)[NoSQL (2)](/article/list?tag=NoSQL)[wxPython (2)](/article/list?tag=wxPython)[智力蛮力 (2)](/article/list?tag=%E6%99%BA%E5%8A%9B%E8%9B%AE%E5%8A%9B)[科普 (2)](/article/list?tag=%E7%A7%91%E6%99%AE)[WordPress (2)](/article/list?tag=WordPress)[诗经 (2)](/article/list?tag=%E8%AF%97%E7%BB%8F)[Mandelbrot (2)](/article/list?tag=Mandelbrot)[悖论 (2)](/article/list?tag=%E6%82%96%E8%AE%BA)[MongoDB (1)](/article/list?tag=MongoDB)[newLISP (1)](/article/list?tag=newLISP)[Ubuntu (1)](/article/list?tag=Ubuntu)[历史 (1)](/article/list?tag=%E5%8E%86%E5%8F%B2)[C (1)](/article/list?tag=C)[Bresenham (1)](/article/list?tag=Bresenham)[电影 (1)](/article/list?tag=%E7%94%B5%E5%BD%B1)[pywin32 (1)](/article/list?tag=pywin32)[pyHook (1)](/article/list?tag=pyHook)[Next.js (1)](/article/list?tag=Next.js)[数字 (1)](/article/list?tag=%E6%95%B0%E5%AD%97)[Alfred (1)](/article/list?tag=Alfred)[设计 (1)](/article/list?tag=%E8%AE%BE%E8%AE%A1)[有限状态机 (1)](/article/list?tag=%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E6%9C%BA)[MacGap (1)](/article/list?tag=MacGap)[PICO-8 (1)](/article/list?tag=PICO-8)[jQuery Mobile (1)](/article/list?tag=jQuery%20Mobile)[无尺度网络 (1)](/article/list?tag=%E6%97%A0%E5%B0%BA%E5%BA%A6%E7%BD%91%E7%BB%9C)[CodeMirror (1)](/article/list?tag=CodeMirror)[Swift (1)](/article/list?tag=Swift)[管理 (1)](/article/list?tag=%E7%AE%A1%E7%90%86)[CoffeeScript (1)](/article/list?tag=CoffeeScript)[Selenium (1)](/article/list?tag=Selenium)[GTD (1)](/article/list...