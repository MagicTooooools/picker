---
title: 逆向飞书思维导图 Protobuf，获取含层级关系的文本（借助 Claude Code）
url: https://blog.skyju.cc/post/feishu-mindmap-protobuf-reverse-engineering-w-claude-code/
source: Terrarum::异世界丨居正博客
date: 2025-12-17
fetch_date: 2025-12-18T03:24:49.687314
---

# 逆向飞书思维导图 Protobuf，获取含层级关系的文本（借助 Claude Code）

[![Avatar](/img/avatar_hu_de1ebe2aa3f5e67d.jpg)](/)

# [Terrarum::异世界丨居正博客](/)

## 大道至简

1. [主页](/)
2. [分类](/categories)
3. [归档](/archives)
4. [朋友](/friends)
5. [搜索](/search)
6. [关于](/about)

- 中文
- 暗色模式

[![Featured image of post 逆向飞书思维导图 Protobuf，获取含层级关系的文本（借助 Claude Code）](https://public.ptree.top/picgo/2025/12/1765961056/PixPin_2025-12-17_16-44-07.jpg)](/post/feishu-mindmap-protobuf-reverse-engineering-w-claude-code/)

[网安](/categories/security/)

## [逆向飞书思维导图 Protobuf，获取含层级关系的文本（借助 Claude Code）](/post/feishu-mindmap-protobuf-reverse-engineering-w-claude-code/)

### 介绍在 Claude Code 的帮助下，逆向飞书的「思维导图」块的过程。包括最开始使用 Python 手写二进制 decode 失败、尝试手写 proto 后用 protobuf decode 失败、最终使用 blackboxprotobuf 成功逆向的过程。

2025 年 12 月 17 日
By  居正

阅读时长: 8 分钟

最近有一个项目重度依赖飞书的思维导图，大概是这样的一个东西：

![PixPin_2025-12-17_16-08-52](https://public.ptree.top/picgo/2025/12/1765958935/PixPin_2025-12-17_16-08-52.jpg)

好处是协作方便，自定义 style 直观快捷；坏处是与纯文本八字不合。

虽然能导出 pdf，但层级关系完全丧失了。

能理解为什么不使用 mermaid 之类的标准格式…但在这个 AI 时代，这样的东西真的很难发给 AI。

用截图的话，小一点的图还好，大图就完全乏力了。而该项目几个月来产生的思维导图恰恰是那种超大型的。

于是想着是否可以通过逆向的方式，提取出纯文本。目标是包含层级关系，最好输出为 markdown、yaml 或者 json 等易读的格式。

## 获取 block 文件

因为网页端是直接通过 canvas 渲染的，所以从 DOM 里什么也看不出来。

问了问 AI 才恍然大悟，原来还可以抓包呀！

![PixPin_2025-12-17_19-40-25](https://public.ptree.top/picgo/2025/12/1765971628/PixPin_2025-12-17_19-40-25.jpg)

原谅自己最近 AI 用得太多，思考能力减退了…

于是 F12 抓包：

![PixPin_2025-12-17_16-12-33](https://public.ptree.top/picgo/2025/12/1765959156/PixPin_2025-12-17_16-12-33.jpg)

这一步其实很简单，通过浏览器控制台抓一下包就能发现是个二进制。

二进制逆向确实有点困难。

不过好在现在有 AI…之前一直在用 Cursor，现在感觉 Claude Code 更加聪明。

应该说 GPT 在 Codex 上最聪明，Claude 在 Claude Code 中使用最聪明，而 Cursor…就两个模型局限都比较大。

## 让 Claude Code 直接分析二进制

最开始的话，其实没有什么想法。

甚至都不知道这个二进制是 protobuf，而且还是直接把整个项目的巨大的思维导图 block 丢给它。

单纯把二进制文件保存下来，然后随便截了一张截图丢给 Claude，让它自己去分析。

不过让 AI 拟定计划后再执行，每一步都形成文档，光是这一点还是知道的。

详见：<https://github.com/tukuaiai/vibe-coding-cn>

prompt：

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` whiteboard/whiteboard 这是飞书思维导图的二进制文件 [Image #1] 你尝试去逆向一下这个文件还原原始的思维导图，最好以JSON或markdown形式提供。可以使用任何语言写任何脚本、做测试之 类的。反复尝试和迭代。先拟定plan，形成plan的文档以及todolist。每进行一步都要更新文档和目前已知的信息和做的尝试 等等，然后确定下一步计划，不断往前推进。工作区限定在 whiteboard/ 文件夹下 ``` |

![PixPin_2025-12-17_16-17-35](https://public.ptree.top/picgo/2025/12/1765959458/PixPin_2025-12-17_16-17-35.jpg)

结果是 Claude 调用工具一通分析，勉强糊出来一个 parser 的 Python 程序，采用的是手写解析二进制的方法。

输出的结果很难看，各种 padding 都不对，文字和乱码掺杂着。

这样的结果肯定不能用。

不过至少知道了文件格式是 protobuf（由 Claude 通过二进制特征发现）。

## 逆向 proto 失败，转为借助 blackboxprotobuf

稍微谷歌一下，发现有一个叫做 blackboxprotobuf 的 Python 库，就是用来干这个的。

<https://github.com/nccgroup/blackboxprotobuf/blob/master/lib/CLI.md>

其中提供一个 bbpb 的二进制程序，可以从二进制中读取 protobuf 的结构。

尝试着对二进制运行了一下：

![PixPin_2025-12-17_16-25-27](https://public.ptree.top/picgo/2025/12/1765959930/PixPin_2025-12-17_16-25-27.jpg)

输出的文件巨大，如上画风的有几千行。

感觉对 AI 太不友好了，让 Claude 直接去读取这个文件分析特征，得爆上下文了吧。

归根到底还是使用的二进制文件太大。既然是逆向，就应该做一个最小的实现的 example 文件，对分析友好一点。

于是就有了开头那张图，先自己做一个小一点的思维导图，把二进制下载下来。

只是这种程度的话，大概还是可以接收的：

![PixPin_2025-12-17_16-51-37](https://public.ptree.top/picgo/2025/12/1765961515/PixPin_2025-12-17_16-51-37.jpg)

这次运行的时候也写一个 CLAUDE.md 文件吧：

|  |  |
| --- | --- |
| ```  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 ``` | ``` # 飞书whiteboard二进制格式逆向  目标：思维导图，需要逆向出来纯文本，要保留内容和层次关系。字体、边框样式这些都可以舍弃。  思维导图里面的文本有中文和英文，大概率是utf8  必须读一下看看思维导图在飞书上显示的是啥样：PixPin_2025-12-17_11-47-29.jpg  源文件：block  工作区限定为：whiteboard目录（当前README文件的目录）  目前已知是用protobuf编码的  注意我们的pip环境安装的软件要带前缀才能执行：  ```bash cat block | /Users/anon/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/bin/bbpb -ot ./types.json ```  保存在 types.json  你在需要时候可以去网上搜索、git clone、pip install、npm install -g、brew install等方式安装使用任何工具  要求：看看能不能逆向出proto文件，用protobuf的方式去decode它，不要手写二进制读取。不断编程和测试，小步迭代。每一步都要有todolist，每一步进行完成后要写文档 ``` |

因为之前被朋友提醒说，Claude 有时候不提醒它的话，它就不知道可以用某些工具（比如 git clone 等等）。所以也明确写一下。

以及因为思维导图变小了，可以把整张图的截图也顺便发给 Claude，让它对导图的结构更加胸有成竹一点。

然后让 Claude 直接开始任务：

![PixPin_2025-12-17_16-29-08](https://public.ptree.top/picgo/2025/12/1765960151/PixPin_2025-12-17_16-29-08.jpg)

其实最开始想的是让 Claude 从 bbpb 生成的 types 中直接洞察出原始的 proto 文件，然后我们也直接用 protobuf 正统的那一套进行 decode。

但实际上这样还是有点天马行空，毕竟原始的 proto 文件可能很复杂。而 protobuf 这种格式稍微错一点整个就解析失败了。

实际上，Claude 在执行过程中自己会发现问题所在，自动转为使用 blackboxprotobuf 这个库：

![PixPin_2025-12-17_16-30-55](https://public.ptree.top/picgo/2025/12/1765960294/PixPin_2025-12-17_16-30-55.jpg)

确实好聪明。

经过几次改错后把 example 的思维导图完美提取出来了：

![PixPin_2025-12-17_16-32-55](https://public.ptree.top/picgo/2025/12/1765960377/PixPin_2025-12-17_16-32-55.jpg)

## 优化脚本

最后把我们项目原本的思维导图 protobuf 二进制文件，用这个脚本运行。

顺便让 Claude 加上输出 markdown、yaml 等能力。

遇到了一些问题，诸如 yaml 格式只能提取出一个父节点而忽略其他的、markdown 格式有换行问题、可能存在节点循环引用导致卡死等等。

都一一让 Claude 解决了。

输出的 yaml 是这种感觉：

![PixPin_2025-12-17_16-48-37](https://public.ptree.top/picgo/2025/12/1765961336/PixPin_2025-12-17_16-48-37.jpg)

最后的成品姑且贴上，留一个备份：

|  |  |
| --- | --- |
| ```   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 200 201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 267 268 269 270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 ``` | ``` #!/usr/bin/env python3 """ Feishu Whiteboard Mindmap Extractor Extract text content and hierarchy from Feishu whiteboard binary format """ import json import blackboxprotobuf import sys import yaml import argparse  def decode_whiteboard(filename):     """Decode whiteboard binary file using blackboxprotobuf"""     with open(filename, 'rb') as f:         data = f.read()      message, typedef = blackboxprotobuf.protobuf_to_json(data)     return json.loads(message)  def get_text(node_data):     """Extract text from node"""     try:         return node_data['2']['2']['33']['1']     except (KeyError, TypeError):         return None  def get_parent(node_data):     """Extract parent ID from node"""     try:         return node_data['2']['2']['61']['1']     except (KeyError, TypeError):         return None  def build_tree(data):     """Build tree structure from decoded data"""     nodes_dict = {}      # Add simple nodes (field 1)     if '1' in data['3']:         for node in data['3']['1']:             node_id = node['1']             nodes_dict[node_id] = {                 'id': node_id,                 'text': None,                 'parent': None,                 'children': []             }      # Add full nodes (field 2)     if '2' in data['3']:         for node in data['3']['2']:             node_id = node['1']             nodes_dict[node_id] = {                 'id': node_id,                 'text': get_text(node),                 'parent': get_parent(node),                 'children': []             }      # Build parent-child relationships     for node_id, node_info in nodes_dict.items():         if node_info['parent'] and node_info['parent'] in nodes_dict:             parent = nodes_dict[node_info['parent']]             parent['children'].append(node_id)      # Find root nodes (nodes without parent and with text content)     root_nodes = [nid for nid, info in nodes_dict.items()                   if not info['parent'] and info['text']]      return nodes_dict, root_nodes  def print_tree(node_id, nodes_dic...