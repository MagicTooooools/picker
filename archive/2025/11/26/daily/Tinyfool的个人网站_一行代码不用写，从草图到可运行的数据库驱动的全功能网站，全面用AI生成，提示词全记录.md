---
title: 一行代码不用写，从草图到可运行的数据库驱动的全功能网站，全面用AI生成，提示词全记录
url: https://codechina.org/2025/11/31263/
source: Tinyfool的个人网站
date: 2025-11-26
fetch_date: 2025-11-27T16:52:26.939827
---

# 一行代码不用写，从草图到可运行的数据库驱动的全功能网站，全面用AI生成，提示词全记录

[跳至内容](#site-content)

搜索

[Tinyfool的个人网站](https://codechina.org/)

菜单

* [首页](https://codechina.org)
* [Blog](https://codechina.org/blog/)
* [Tiny推荐](https://codechina.org/tiny-recommendation/)

搜索

搜索：

关闭搜索

关闭菜单

* [首页](https://codechina.org)
* [Blog](https://codechina.org/blog/)
* [Tiny推荐](https://codechina.org/tiny-recommendation/)

分类

[AI](https://codechina.org/category/ai/)

# 一行代码不用写，从草图到可运行的数据库驱动的全功能网站，全面用AI生成，提示词全记录

* 文章作者

  [tinyfool](https://codechina.org/author/tinyfool/)
* 发布日期

  [26 11 月, 2025](https://codechina.org/2025/11/31263/)
* [一行代码不用写，从草图到可运行的数据库驱动的全功能网站，全面用AI生成，提示词全记录无评论](https://codechina.org/2025/11/31263/#respond)

![](https://codechina.org/wp-content/uploads/2025/11/Gemini-Generated-Image-23-1200x896.jpeg)

最近我打算做一个讲机械原理的英文网站，正好Gemini 3出来，前端能力很强，于是我就用手绘了草图，让Gemini帮我生成页面，然后用这个页面去做模版，用 google 的 AI编程工具 antigravity 生成了网站，然后上线了。我给大家记录我用的全部提示词（不包括 Gemini 和 antigravity 的回复），让大家感受一下，完全不懂代码，纯靠嘴的编程的快乐。一开始是Gemini：

![](https://codechina.org/wp-content/uploads/2025/11/截屏2025-11-26-21.24.01-1.png)

它给我生成了一张图片，而不是代码可能是我提示词不够精准

![](https://codechina.org/wp-content/uploads/2025/11/Gemini-Generated-Image-23-1-1024x765.jpeg)

我说：

* 风格很好，帮我生成html页面
* 能不能把页面的图片根据刚才的效果图配好，再给整个页面丰富一些内容，让它至少有几个屏幕那么长
* 1、改成全英文2、我们不是只关心蒸汽机而是整个科技史，比如蒸汽机、珍妮纺纱机。我们关注历史的进程，发展的讨论和各种机械的原理。文章和视频都有，历史图片和机械原理的动画等等。3、先帮助填充些内容，文章页页设计一下
* 有个报错处理一下
* 网站标题是Mechanica Planet，logo如附件，我在youtube给这个网站做了一期节目，链接上来吧 https://www.youtube.com/watch?v=BNHRlbjwrsA
* 让视频放在更显眼的地方
* （点击加入AI交互能力）Gemini自动加了个The Mechanical Oracle (机械神谕)，可以对话聊机械常识。Artifact Analyzer (文物鉴定仪)把一个机器的图片传给它，它会识别出来给你介绍。
* 这些Gemini增强功能，Oracle等，我如何部署
* 丰富首页，让我看看，内容很多的时候，网站的样子
* lab改成机械博物馆，展示各种科技史的发明
* Museum的模式我不喜欢，不能容纳更多的展品
* 去掉Oracle和 ARTIFACT ANALYZER（用这些部署有点麻烦现在懒得弄）
* 把博物馆放在第三排

至此，网站的视觉设计完成。得到了一个纯html页面，好几屏。截图如下（本地执行缺一些图片）：

![](https://codechina.org/wp-content/uploads/2025/11/Mechanica-Planet-Museum-of-Industrial-Evolution-455x1024.jpeg)

我把这个html文件保存在工程目录，然后，开了一个 antigravity 的项目，开始跟 antigravity 对话：

* 帮我做一个PHP，网站用Laravel框架来做，网站风格用原型页面po.html
* 实现一个登陆界面，让网站管理员可以登陆后台
* 密码啥啥（笔误，AI看懂了，告诉我密码在哪里，这里没做更复杂的数据库管理密码，这个需求暂时不需要那么复杂）
* 后台页面在哪里
* 生成完整的cms后台，包括发布文章，修改文章，等等功能
* 我在http://127.0.0.1:8000/dashboard没看到这些功能啊
* 完善导航栏之类的，现在点左上角logo都不能回到首页
* 发了一篇文章，首页怎么看不到
* 以后不要我review了，直接接受一切答案，除非你要大规模修改数据库（没听我的）
* cms就是要管理整个网站的全部内容，不能只有简单的一种文章，理解么？
* 继续
* 你看看后台，这都乱了（附了一张截图）
* 我加了一个exhibit，但是首页没有显示，原来的也没有在后台可以管理
* logo.jpg文件是网站的logo，放进代码里面吧
* 网站logo还不对，首页的Digital Archive Catalog似乎也不是从数据库读出来的
* cms进行大改进，输入都是可视化的编辑器，支持图片上传.每个内容分类都有自己页面，在首页点击更多的时候进去自己的页面。每个文章都可以单独打开，而不是仅在页面加载
* 继续
* logo的显示还是不对
* Digital Archive Catalog这个区域在首页是混乱的显示
* 去掉Gallery这个栏目
* 去掉timeline
* 文章增加类型，科技史和机械原理这两个类型
* 帮我生成几个假文章，然后在把这两个分类都在首页上加入他们的展现和导航
* 后台增加一个功能，把一个文章放到Featured的位置上。首页增加最新更新的板块
* Hero Section先去掉
* FEATURED ARTICLE放在左上，右边是Latest Updates
* 继续完善网站，在页面加上seo增强的代码，在文章页加上分享到社交网络的能力。改善文章页的显示效果
* 我的编辑器如何支持嵌入youtube视频，在文章里面
* 如果被FEATURED的文章包含youtube视频，则把youtueb视频放在被FEATURED显示的部分
* 配色有问题，文章标题看不清楚（附图）
* 文章页面问题类似，也改了吧
* 如果是登陆状态，管理员的话，那么在文章页面上提供直接编辑的按钮
* Exhibits的页面也是
* 目前的代码有git管理么
* 你建议我在主目录建立git，还是在子目录mechanica建立
* 做吧
* 我已经把代码推到云端，web服务器我也设置好了，请指导我部署这个网站
* php artisan db:seed –forceINFO  Seeding database.In UserFactory.php line 27:Call to undefined function Database\Factories\fake()（遇到错误，直接交给AI，我啥也没管）
* 提交代码到git
* deploy文档是nginx，我是apache2，更新文档
* admin密码现在是写死的？（问了下，但是懒得改了，没让它改）
* 后台加入功能，在全局页面header加入js代码的能力，在页面某些位置加入html代码的能力，比如我用来加入google分析和google广告代码的需求，比如一些定制的html code。
* 提交代码
* 在服务器更新了代码，还需要什么操作
* exhibits详情页面字的颜色不对，看着很不舒服（附图）
* 提交代码
* 文章内容滚动的时候，文章标题不滚动，很干扰阅读
* 提交代码，不需要我确认直接执行（还是让我确认）
* 奇怪本地没问题，部署到服务器后，还是不滚动，为啥？难道css有缓存？
* 提交代码

然后发布成功，我在后台发了几个文章，加了几个博物馆收藏，就正式公开这个网站了。

网站现在这个样子：

![](https://codechina.org/wp-content/uploads/2025/11/Mechanica-Planet-Museum-of-Industrial-Evolution-·-21.58-·-11-26-scaled.jpeg)

有登录功能：

![](https://codechina.org/wp-content/uploads/2025/11/截屏2025-11-26-22.01.11.png)

管理后台：

![](https://codechina.org/wp-content/uploads/2025/11/截屏2025-11-26-22.01.51-1024x549.png)

后台编辑文章有可视化编辑器：

![](https://codechina.org/wp-content/uploads/2025/11/截屏2025-11-26-22.02.42-1024x969.png)

这就是今天这个新时代。

回头我会在我的 Youtube 视频，重新按照这个流程做一个网站，全程录制下来。不过你看文字版是不是已经看到了，AI编程的快乐？

点击下面的链接可以直接访问：<https://mechanicaplanet.com/>

### 相关文章:

[## 开源音频驱动人像动画(对口型)项目列表](https://codechina.org/2024/06/30852/)[## ChatGPT即将到来的AI新时代以及对我们的改变](https://codechina.org/2023/03/28406/)[## 从智能手机到人工智能：创新为什么总会被质疑，以及我们应该如何聪明的对待创新](https://codechina.org/2023/11/30562/)[## 万能的ChatGPT真有智能了么？一篇文章让你彻底搞懂ChatGPT-人类是怎么训练出了一只聪明的莎士比亚的猴子](https://codechina.org/2023/01/25998/)[## 美国最顶尖的人工智能专家1/3是中国人，如果他们回国，立即抹平中美之间的技术差距](https://codechina.org/2024/03/30686/)[## ChatGPT又发布了一堆新功能，普通人在AI时代应该如何应对](https://codechina.org/2023/11/30548/)

打赏

![微信扫一扫支付](https://codechina.org/wp-content/uploads/2022/06/2.pic_.jpg)

![微信logo](https://codechina.org/wp-content/plugins/wechat-reward/assets/ico-wechat.jpg)微信扫一扫，打赏作者吧～

---

[←
如何在 YouTube 上找到有流量但竞争低的关键词与选题](https://codechina.org/2025/11/31250/)

---

## 发表回复 [取消回复](/2025/11/31263/#respond)

您的邮箱地址不会被公开。 必填项已用 \* 标注

评论 \*

显示名称 \*

邮箱 \*

网站

[ ]  在此浏览器中保存我的显示名称、邮箱地址和网站地址，以便下次评论时使用。

Δ

这个站点使用 Akismet 来减少垃圾评论。[了解你的评论数据如何被处理](https://akismet.com/privacy/)。

搜索：

## 近期文章

* [一行代码不用写，从草图到可运行的数据库驱动的全功能网站，全面用AI生成，提示词全记录](https://codechina.org/2025/11/31263/)
  26 11 月, 2025
* [如何在 YouTube 上找到有流量但竞争低的关键词与选题](https://codechina.org/2025/11/31250/)
  18 11 月, 2025
* [免费剪辑工具：零成本开始你的创作（2025年版）](https://codechina.org/2025/11/31229/)
  1 11 月, 2025
* [YouTube SEO入门：轻松掌握标题、标签与描述](https://codechina.org/2025/11/31221/)
  1 11 月, 2025
* [循序渐进工作流：从拍摄到上传你的第一支 YouTube 视频（新手指南 + 可打印清单）](https://codechina.org/2025/10/31217/)
  31 10 月, 2025

## 最新评论

* tinyfool 发表在《[不到百元低成本启动 YouTube 指南：2025 年新手低成本设备配置](https://codechina.org/2025/10/31181/#comment-35063)》
* tinyfool 发表在《[YouTube新手最值得入手的五款视频剪辑软件对比（2025年指南）](https://codechina.org/2025/10/31205/#comment-35062)》
* 土豆泥 发表在《[不到百元低成本启动 YouTube 指南：2025 年新手低成本设备配置](https://codechina.org/2025/10/31181/#comment-35016)》
* 土豆泥 发表在《[YouTube新手最值得入手的五款视频剪辑软件对比（2025年指南）](https://codechina.org/2025/10/31205/#comment-35015)》
* [liuf](https://blog.liuf.net) 发表在《[我们都需要感谢郑智化的撒娇和矫情](https://codechina.org/2025/10/31171/#comment-34973)》

## 标签

[CapCut剪辑](https://codechina.org/tag/capcut%E5%89%AA%E8%BE%91/)
[ChatGPT](https://codechina.org/tag/chatgpt/)
[cpu](https://codechina.org/tag/cpu/)
[ghe](https://codechina.org/tag/ghe/)
[github](https://codechina.org/tag/github/)
[GPT3](https://codechina.org/tag/gpt3/)
[mac](https://codechina.org/tag/mac/)
[Mac OS](https://codechina.org/tag/mac-os/)
[nlp](https://codechina.org/tag/nlp/)
[swift](https://codechina.org/tag/swift/)
[swiftui](https://codechina.org/tag/swiftui/)
[youtube](https://codechina.org/tag/youtube/)
[YouTube SEO](https://codechina.org/tag/youtube-seo/)
[YouTube优化](https://codechina.org/tag/youtube%E4%BC%98%E5%8C%96/)
[YouTube入门](https://codechina.org/tag/youtube%E5%85%A5%E9%97%A8/)
[YouTube内容创作](https://codechina.org/tag/youtube%E5%86%85%E5%AE%B9%E5%88%9B%E4%BD%9C/)
[YouTube内容策略](https://codechina.org/tag/youtube%E5%86%85%E5%AE%B9%E7%AD%96%E7%95%A5/)
[YouTube内容规划](https://codechina.org/tag/youtube%E5%86%85%E5%AE%B9%E8%A7%84%E5%88%92/)
[YouTube增长](https://codechina.org/tag/youtube%E5%A2%9E%E9%95%BF/)
[YouTube实用建议](https://codechina.org/tag/youtube%E5%AE%9E%E7%94%A8%E5%BB%BA%E8%AE%AE/)
[YouTube成功秘诀](https://codechina.org/tag/youtube%E6%88%90%E5%8A%9F%E7%A7%98%E8%AF%80/)
[YouTube成长](https://codechina.org/tag/youtube%E6%88%90%E9%95%BF/)
[YouTube成长指南](https://codechina.org/tag/youtube%E6%88%90%E9%95%BF%E6%8C%87%E5%8D%97/)
[YouTube数据分析](https://codechina.org/tag/youtube%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90/)
[YouTube新手指南](https://codechina.org/tag/youtube%E6%96%B0%E6%89%8B%E6%8C%87%E5%8D%97/)
[YouTube标题优化](https://codechina.org/tag/youtube%E6%A0%87%E9%A2%98%E4%BC%98%E5%8C%96/)
[YouTube流量](https://codechina.org/tag/youtube%E6%B5%81%E9%87%8F/)
[YouTube点击率](https://codechina.org/tag/youtube%E7%82%B9%E5%87%BB%E7%8E%87/)
[YouTube算法](https://codechina.org/tag/youtube%E7%AE%97%E6%B3%95/)
[YouTube营销](https://codechina.o...