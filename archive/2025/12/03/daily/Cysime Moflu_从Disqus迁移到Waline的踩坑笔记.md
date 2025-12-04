---
title: 从Disqus迁移到Waline的踩坑笔记
url: https://blog.cysi.me/2025/12/migrate-from-disqus-to-waline/
source: Cysime Moflu
date: 2025-12-03
fetch_date: 2025-12-04T03:23:07.837734
---

# 从Disqus迁移到Waline的踩坑笔记

[![Avatar](/img/yukiakari_hu_f3078ec4886472d2.jpg)](/)🦊

# [Cysime Moflu](/)

## 再不会遇见第二个时光

1. [Home](/)
2. [Archives](/archives/)
3. [Links](/link/)
4. [Gallery](/gallery/)
5. [About](/about/)
6. [Search](/search/)
7. 1. 暗色模式

## 目录

1. [评论系统的选择](#评论系统的选择)
2. [搭建和配置Waline](#搭建和配置waline)
   1. [使用MongoDB+Vercel搭建](#使用mongodbvercel搭建)
   2. [评论迁移](#评论迁移)
   3. [整理文章](#整理文章)
3. [完成](#完成)

[![Featured image of post 从Disqus迁移到Waline的踩坑笔记](/2025/12/migrate-from-disqus-to-waline/comments_hu_72ea27c544947af0.jpg)](/2025/12/migrate-from-disqus-to-waline/)

[技术](/categories/%E6%8A%80%E6%9C%AF/)

## [从Disqus迁移到Waline的踩坑笔记](/2025/12/migrate-from-disqus-to-waline/)

### 自从本站将框架从Wordpress改成Hugo后我一直使用Disqus作为评论系统，但即便是在使用了DisqusJS这样的项目并配合Vercel的反代，Disqus在大陆直连体验仍然不甚理想，甚至还可能完全无法加载，为了能有个正常点的评论体验，只能换个别的，Waline就是一个优秀的替代品。

Dec 03, 2025

阅读时长: 9 分钟

我的博客是2022年从Wordpress换成Hugo的，受制于Hugo这种纯静态框架的限制，必须要额外再配备评论系统。当时Waline等产品只能使用Leancloud作为数据库，所以[为了省事直接用的Disqus](https://blog.cysi.me/2022/05/migrate-to-hugo/#%E8%AF%84%E8%AE%BA%E8%BF%81%E7%A7%BB%E5%92%8C%E4%BD%BF%E7%94%A8)，后者是可以直接接入Wordpress的，因此可以非常简单地转移到Hugo，并且使用DisqusJS这样的项目并配合反向代理的话体验还不错。但到了现在，即便使用了Vercel的反代，Disqus的体验也很难说好，且我还多次收到反馈说有用户根本没法加载Disqus评论区。为了能有个阳间一点的体验，是时候换个新的了。

## 评论系统的选择

我的Hugo主题是Stack，它内置支持了不少评论系统，所以直接在[兼容列表](https://stack.jimmycai.com/config/comments)里面选择是最方便的。

其实当前评论系统无非两类：

一是Giscus、Gitalk这类利用Github Discuss或者Issues的，很方便搭建且无需再配备数据库，使用GitHub登录也一定程度上避免了Spam，但缺点也是GitHub：它必须也只能使用GitHub（或类似平台）登录；另一类比如Twikoo、Waline就支持匿名评论，和WordPress的体验完全一致，但缺点是需要自行配备数据库存储评论。

我个人来讲还是更希望使用类似WordPress那样的匿名评论系统，无需登录，仅需留下邮箱和昵称即可直接评论；同时访客也并不一定有GitHub账号。当时使用Disqus其实也有这个原因，因为Disqus其实也是可以不用注册账号就能评论的。

基于上面的需求且比较适合国人的就只有Twikoo和Waline了。

Twikoo是个新一点的评论系统，使用MongoDB，我个人感觉相较于Waline来讲要更简约简单一点。但有一个问题：太简单了。它没有单独的后台，只能自己找一个使用了Twikoo的页面进入管理面板，且这个面板不能在新页面中打开而是就评论区里面原地启动，显而易见地这个面板的展开面积就很小了。虽然GUI可修改的各种自定义项目很多（但主题都已经适配好了，所以一般来讲是不需要动的），管理起来是真的有点麻烦。如果有个独立面板就更好了。

另一个大问题就是，我们都知道这些评论系统是通过文章URL来判断并输出对应文章的评论的，Twikoo配置的文章URL是使用的绝对路径而非相对路径，但我这个博客现在绑定了2个域名，且后续我想把`cysi.me`这个顶级域名也改成博客本体，如果评论对应的文章URL全是绝对路径（如`https://example.com/post/1`）而非相对路径（如`/post/1`），一旦域名更换就必然无法识别。

至于Waline，已经存在挺久了，且现在还在活跃更新，用户群也不少。早年它只能用Leancloud数据库，当时因为这个也走了点弯路去用了Disqus，但现在它还支持MongoDB等多种数据库了。我最终也选择使用Waline。

## 搭建和配置Waline

### 使用MongoDB+Vercel搭建

我使用的是Vercel+MongoDB的组合，其中Vercel的部分可以参考[官方文档](https://waline.js.org/guide/get-started/#vercel-%E9%83%A8%E7%BD%B2-%E6%9C%8D%E5%8A%A1%E7%AB%AF)。需要注意的是，官方文档的“快速上手”是基于Leancloud数据库的，如果不想使用Leancloud（比如我这种使用MongoDB的）则需要修改Vercel那边对应的环境变量，具体可以查阅[这里](https://waline.js.org/guide/database.html)。

总体来讲搭建并不算难，但MongoDB这边会有个坑，MongoDB默认给的URI是`mongodb+srv://`格式的，可以让客户端自动从DNS获取Seedlist服务器列表，但Waline的MongoDB实现比较老，并不支持`+srv`自动获取完整服务器列表（如果直接使用新版URI，Waline会出现500错误），必须获取旧版的`mongodb://`格式链接从而提取出服务器地址。

![MongoDB](/2025/12/migrate-from-disqus-to-waline/Mongodb.png)

因此，我们需要在MongoDB面板中，点击Connect，并选择Compass，如上图一样选择“I have MongoDB Compass installed”，版本选择“1.11 or earlier”，你会得到类似这样的URI。

|  |  |
| --- | --- |
| ``` 1 ``` | ``` mongodb://<username>:<db_password>@ac-vm0ftwk-shard-00-00.9cdzdlb.mongodb.net:27017,ac-vm0ftwk-shard-00-01.9cdzdlb.mongodb.net:27017,ac-vm0ftwk-shard-00-02.9cdzdlb.mongodb.net:27017/?replicaSet=atlas-opg9fs-shard-0&ssl=true&authSource=admin ``` |

可以看到这上面有3个服务器地址，我们提取出来。然后缝合进Waline官方已经整理好的环境变量里面，并将其设置进Vercel项目内即可。Waline部署完成后，访问`https://<your-site-url>/ui`即可进入后台并创建用户即可。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 ``` | ``` // 服务器列表，记得换上上面提取出来的地址 MONGO_HOST=["cluster0-shard-00-00.p4edw.mongodb.net","cluster0-shard-00-01.p4edw.mongodb.net","cluster0-shard-00-02.p4edw.mongodb.net"] MONGO_PORT=[27017,27017,27017,27017] MONGO_DB=waline MONGO_USER=admin //修改为你设置的username MONGO_PASSWORD=xxxx MONGO_REPLICASET=atlas-12cebf-shard-0 MONGO_AUTHSOURCE=admin MONGO_OPT_SSL=true ``` |

### 评论迁移

迁移也并不困难，先去Disqus的[导出数据页面](https://disqus.com/admin/discussions/export/)导出并下载数据，再在Waline官方提供的[迁移助手](https://waline.js.org/migration/tool.html)转换即可，记得要选对Waline的数据库格式。如果是MongoDB，导出的数据是CSV格式。

正常情况下，直接在MongoDB那边导入这个CSV数据到Comment数据表里面就行了，MongoDB官方有一个GUI管理工具Compass，可以直接导入CSV文件， 官方也同样提供了[相应教程](https://www.mongodb.com/zh-cn/docs/compass/import-export/)。如果不需要修改数据，导入CSV到指定数据库里面就算完成了，这时候评论系统也可以正常使用且旧评论都会正常迁移过去。

### 整理文章

但我这里情况比较特殊，我的博客已经跑了10多年了而且期间变过很多次永久链接格式（Permalink），而且还变过网站架构（Wordpress变成Hugo），这一系列操作下来，结果就是我的博客的链接格式是很混乱的，比如有的链接末尾带了`/`而有的没有。另一个问题是，我之前启用了Hugo的`uglyURL`功能，它会在博客内所有页面添加`.html`的后缀，致敬早年SEO的伪装静态文件的做法（实际上是因为我当时WordPress链接就是这样子的格式，为了保留后缀所以启用了这个功能），但这个`uglyURL`功能**仅对**Hugo设置中的`baseURL`对应域名才有效，且`baseURL`仅能设置一个——但我的博客有多个域名，也就是说，我现在**只有**`https://blog.cysi.me`这一个域名底下的URL后缀会带有`.html`而其他任何域名都不会有，除非我在每个Markdown文件的`slug`或者`url`字段都添加该后缀。

而Waline这些评论系统是靠文章URL来输出对应URL下的评论内容的，即便它使用的是相对路径，理论上不同域名只要URL结构一样也不会出问题，评论都可以正常在各个域名中正常共享，但基于我上面的情况，还是引发了两个坑：

1. Waline会把同URL但末尾带或者不带`/`的链接认为是2个不同文章，同理，带或者不带`.html`后缀也是会被识别成不同的文章的。Waline实际上提供了一个[解决方案](https://waline.js.org/reference/client/props.html#path)，就是在客户端处增加一个`path`属性，但我添加了这个字段没有起作用，所以我就开始考虑直接修改数据库的内容，此举顺便也是为了整理和梳理我博客混乱的永久链接的格式，将所有文章URL的格式统一。
2. 要完全统一URL格式，就必须关闭`uglyURL`功能，也就说所有的`.html`后缀都将会被移除，但此时访问带有`.html`后缀的URL都不会正常跳转并直接报错404。

好在这两个问题解决起来不难，第1个问题，上一步里面使用迁移助手转换得到的CSV文件，可以直接使用编辑器进行编辑，具体步骤就不必赘述了，毕竟现在让AI帮忙也很简单，然后再导入进MongoDB，会比直接在MongoDB上面使用数据库语句要方便点。Hugo默认情况下，文章URL末尾均带有`/`后缀，因此，我就要使用编辑器将CSV里面的评论URL字段，去除所有不必要的后缀（比如`.html`）并再在末尾都统一添加上`/`。

接下来处理问题2，关闭了`uglyURL`之后，实质上我现在的URL格式已经得到统一，但还是得做一个跳转（至少是旧的文章需要做跳转），避免出现404问题。其实Vercel等平台是可以直接跳转的，只需要在`vercel.json`内设置跳转即可，但我碰巧使用了多个平台（不同域名使用了不同的部署平台），经过考量我决定直接在Markdown的Frontmatter元数据里面设定`aliases`字段（也就是别名），我们可以在别名中添加`.html`后缀，这样就可以自动跳转了。这种办法的好处是完全不挑平台支持跳转与否，完全通用。直接让Gemini生成了个Python脚本，这个脚本会递归扫描指定目录，处理所有 .md 文件，并按照此逻辑优先级执行：

1. 有 aliases -> 跳过。
2. 无 aliases 但有 url -> 新增 aliases 为 url内容.html。
3. 无 aliases 且无 url，但有 slug -> 新增 aliases 为 slug内容.html。

运行该脚本后，我所有文章都会添加一个带有`.html`后缀的别名，访问后会自动跳转Hugo的标准无后缀的URL上，避免出现404错误。

|  |  |
| --- | --- |
| ```  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 ``` | ``` import os import re  def process_markdown_files(root_dir):     # 遍历目录及其子目录     for root, dirs, files in os.walk(root_dir):         for file in files:             if file.endswith(".md"):                 file_path = os.path.join(root, file)                 process_single_file(file_path)  def process_single_file(file_path):     try:         with open(file_path, 'r', encoding='utf-8') as f:             content = f.read()     except Exception as e:         print(f"❌ 读取错误: {file_path} - {e}")         return      # 正则表达式：匹配 Frontmatter (位于文件开头的两个 --- 之间)     # re.DOTALL 让 . 也能匹配换行符     fm_pattern = re.compile(r'^---\s*\n(.*?)\n---\s*\n', re.DOTALL)     match = fm_pattern.match(content)      if not match:         # print(f"⚠️ 跳过 (无 Frontmatter): {file_path}")         return      frontmatter_content = match.group(1)          # 1. 检查是否已经存在 aliases     if re.search(r'^aliases:', frontmatter_content, re.MULTILINE):         # print(f"⏭️ 跳过 (已有 aliases): {file_path}")         return      # 寻找 url 或 slug     # 匹配 key: value 格式，并捕获 value 部分（去除首尾空格和引号）     url_match = re.search(r'^url:\s*(.+)$', frontmatter_content, re.MULTILINE)     slug_match = re.search(r'^slug:\s*(.+)$', frontmatter_content, re.MULTILINE)      target_value = None     source_field = ""      # 2. 逻辑优先级：先找 url，没有再找 slug     if url_match:         raw_value = url_match.group(1).strip()         # 去除可能存在的引号         target_value = raw_value.strip('"\'')         source_field = "url"     elif slug_match:         raw_value = slug_match.group(1).strip()         target_value = raw_value.strip('"\'')         source_field = "slug"          if target_value:         # 3. 构造新的 aliases 字段         # 注意：通常 aliases 是一个数组，为了兼容性，这里写成 aliases: ["value.html"]         # 如果你的内容里已经有.html后缀，这里会重复添加吗？         # 根据你的要求：“内容并在其末尾添加.html”，这里不做去重检查，直接添加。                  new_alias_line = f'aliases: ["{target_value}.html"]'                  # 将新字段插入到 frontmatter 的末尾（但在第二...