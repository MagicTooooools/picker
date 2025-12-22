---
title: gmt特殊字符
url: http://www.seis-jun.xyz/gmt-special-character
source: SEISAMUSE
date: 2025-12-21
fetch_date: 2025-12-22T03:32:07.960828
---

# gmt特殊字符

[SEISAMUSE](/)

Jun Xie的博客

* [首页](/)
* [标签](/tags/)
* [分类](/categories/)
* [归档](/archives/)
* [关于](/about/)
* 搜索

* 文章目录
* 站点概览

![Jun Xie](/images/seisamuse.png)

Jun Xie

好记性不如烂笔头

[182
日志](/archives/)

[14
分类](/categories/)

[78
标签](/tags/)

[GitHub](https://github.com/junxie01 "GitHub → https://github.com/junxie01")

### 标签云

* [FreshRSS](/tags/FreshRSS/)1
* [Fun](/tags/Fun/)1
* [LaTeX](/tags/LaTeX/)2
* [Linux](/tags/Linux/)9
* [NCF](/tags/NCF/)1
* [Seismology](/tags/Seismology/)1
* [ai](/tags/ai/)1
* [axes](/tags/axes/)1
* [blog](/tags/blog/)1
* [blogs](/tags/blogs/)1
* [code](/tags/code/)2
* [filezilla](/tags/filezilla/)1
* [git](/tags/git/)1
* [glacial seismology](/tags/glacial-seismology/)1
* [gmt](/tags/gmt/)3
* [grub](/tags/grub/)1
* [hexo](/tags/hexo/)12
* [linux](/tags/linux/)5
* [model](/tags/model/)1
* [next](/tags/next/)6
* [obspy](/tags/obspy/)1
* [paper](/tags/paper/)44
* [project](/tags/project/)2
* [python](/tags/python/)18
* [review](/tags/review/)1
* [science](/tags/science/)1
* [script](/tags/script/)1
* [sed](/tags/sed/)1
* [seismic](/tags/seismic/)1
* [seismology](/tags/seismology/)4
* [sem](/tags/sem/)1
* [special character](/tags/special-character/)1
* [video](/tags/video/)1
* [web](/tags/web/)14
* [wiki](/tags/wiki/)1
* [work](/tags/work/)1
* [中文](/tags/%E4%B8%AD%E6%96%87/)1
* [乱](/tags/%E4%B9%B1/)1
* [乱笔](/tags/%E4%B9%B1%E7%AC%94/)1
* [儿子](/tags/%E5%84%BF%E5%AD%90/)1
* [博客](/tags/%E5%8D%9A%E5%AE%A2/)2
* [历史](/tags/%E5%8E%86%E5%8F%B2/)1
* [反演](/tags/%E5%8F%8D%E6%BC%94/)1
* [命题作文](/tags/%E5%91%BD%E9%A2%98%E4%BD%9C%E6%96%87/)1
* [外公](/tags/%E5%A4%96%E5%85%AC/)1
* [外婆](/tags/%E5%A4%96%E5%A9%86/)1
* [大脑](/tags/%E5%A4%A7%E8%84%91/)1
* [好友](/tags/%E5%A5%BD%E5%8F%8B/)1
* [妖怪](/tags/%E5%A6%96%E6%80%AA/)1
* [学习](/tags/%E5%AD%A6%E4%B9%A0/)4
* [宁波](/tags/%E5%AE%81%E6%B3%A2/)1
* [安息](/tags/%E5%AE%89%E6%81%AF/)1
* [憎恨](/tags/%E6%86%8E%E6%81%A8/)1
* [成都](/tags/%E6%88%90%E9%83%BD/)1
* [打牌](/tags/%E6%89%93%E7%89%8C/)1
* [无题](/tags/%E6%97%A0%E9%A2%98/)1
* [日记](/tags/%E6%97%A5%E8%AE%B0/)4
* [杂](/tags/%E6%9D%82/)8
* [某日记](/tags/%E6%9F%90%E6%97%A5%E8%AE%B0/)1
* [活着](/tags/%E6%B4%BB%E7%9D%80/)1
* [猫](/tags/%E7%8C%AB/)1
* [电影](/tags/%E7%94%B5%E5%BD%B1/)3
* [电视剧](/tags/%E7%94%B5%E8%A7%86%E5%89%A7/)1
* [瘾](/tags/%E7%98%BE/)1
* [盆地](/tags/%E7%9B%86%E5%9C%B0/)1
* [网易](/tags/%E7%BD%91%E6%98%93/)1
* [茶铺](/tags/%E8%8C%B6%E9%93%BA/)1
* [诡异](/tags/%E8%AF%A1%E5%BC%82/)1
* [读后感](/tags/%E8%AF%BB%E5%90%8E%E6%84%9F/)1
* [躯壳](/tags/%E8%BA%AF%E5%A3%B3/)1
* [过渡带](/tags/%E8%BF%87%E6%B8%A1%E5%B8%A6/)1
* [这是啥](/tags/%E8%BF%99%E6%98%AF%E5%95%A5/)1
* [迷雾](/tags/%E8%BF%B7%E9%9B%BE/)1
* [逝者](/tags/%E9%80%9D%E8%80%85/)1
* [阿甘](/tags/%E9%98%BF%E7%94%98/)1
* [雾霾](/tags/%E9%9B%BE%E9%9C%BE/)1
* [霸王别姬](/tags/%E9%9C%B8%E7%8E%8B%E5%88%AB%E5%A7%AC/)1
* [饭后感](/tags/%E9%A5%AD%E5%90%8E%E6%84%9F/)1

# gmt特殊字符

发表于
2025-12-21

分类于

[gmt](/categories/gmt/)

阅读次数：

本文字数：
189

阅读时长 ≈
1 分钟

最重要的是以下两个表：

![](/gmt-special-character/a.png)

至于是参考左边的Standard+还是右边的ISOLation1+，就要用命令

|  |  |
| --- | --- |
| ``` 1 ``` | ``` gmt get PS_CHAR_ENCODING ``` |

查看你的编码方式了。
另外如果了12号字体(Symbol)或34号字体(ZapfDingbats)则查下面这个表：

![](/gmt-special-character/b.png)

好了，搞定。自求多福哈，嘿嘿。

请我一杯咖啡吧！

赞赏

![Jun Xie 微信](/images/wechatpay.png)
微信

![Jun Xie 支付宝](/images/alipay.png)
支付宝

[# special character](/tags/special-character/)

[文献阅读(41)](/paper-reading-41 "文献阅读(41)")

© 2020 –
2025

Jun Xie

319k

4:50

0%

Theme NexT works best with JavaScript enabled