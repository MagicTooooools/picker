---
title: Mac中安装FreshRSS
url: http://www.seis-jun.xyz/install-freshrss-on-mac
source: SEISAMUSE
date: 2025-11-26
fetch_date: 2025-11-27T16:52:26.355744
---

# Mac中安装FreshRSS

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

1. [1. 依赖安装（PHP + Composer）](#%E4%BE%9D%E8%B5%96%E5%AE%89%E8%A3%85%EF%BC%88PHP-Composer%EF%BC%89)
2. [2. 安装FreshRSS](#%E5%AE%89%E8%A3%85FreshRSS)
3. [3. 迁移和更新](#%E8%BF%81%E7%A7%BB%E5%92%8C%E6%9B%B4%E6%96%B0)
4. [4. 下次登录](#%E4%B8%8B%E6%AC%A1%E7%99%BB%E5%BD%95)

![Jun Xie](/images/seisamuse.png)

Jun Xie

好记性不如烂笔头

[169
日志](/archives/)

[14
分类](/categories/)

[77
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
* [paper](/tags/paper/)32
* [project](/tags/project/)2
* [python](/tags/python/)18
* [review](/tags/review/)1
* [science](/tags/science/)1
* [script](/tags/script/)1
* [sed](/tags/sed/)1
* [seismic](/tags/seismic/)1
* [seismology](/tags/seismology/)4
* [sem](/tags/sem/)1
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

# Mac中安装FreshRSS

发表于
2025-11-26

分类于

[mac](/categories/mac/)

阅读次数：

本文字数：
546

阅读时长 ≈
1 分钟

参考FreshRSS的github[主页](https://github.com/FreshRSS/FreshRSS/tree/edge)。不过还是麻烦，请[千问](https://www.qianwen.com/)帮忙，简直不要太简单。步骤如下：

## 依赖安装（PHP + Composer）

用以下命令安装:

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` brew install php brew install composer ``` |

## 安装FreshRSS

命令如下：

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` git clone https://github.com/FreshRSS/FreshRSS.git cd FreshRSS # 如果想参与开发可切入edge分支：git checkout edge php -S localhost:8080 -t . ``` |

接下来访问 [http://localhost:8080](http://localhost:8080/) 进行安装。

## 迁移和更新

数据都在data下面，拷贝他就可以进行迁移。更新的话运行如下脚本。

|  |  |
| --- | --- |
| ``` 1 ``` | ``` git pull origin main ``` |

## 下次登录

运行以下脚本

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` echo 'alias freshrss="cd ~/FreshRSS && php -S localhost:8080 -t ."' >> ~/.zshrc source ~/.zshrc ``` |

然后每次运行freshrss，然后在浏览器访问 [http://localhost:8080](http://localhost:8080/) 就好。
大功告成。

请我一杯咖啡吧！

赞赏

![Jun Xie 微信](/images/wechatpay.png)
微信

![Jun Xie 支付宝](/images/alipay.png)
支付宝

[# FreshRSS](/tags/FreshRSS/)

[hexo插入图片](/hexo-picture "hexo插入图片")

© 2020 –
2025

Jun Xie

287k

4:21

0%

Theme NexT works best with JavaScript enabled