---
title: Hexo 版本更新与技术债务
url: https://blog.ponder.work/2026/01/02/hexo-upgrade-and-tech-debt/
source: 逸思杂陈
date: 2026-01-02
fetch_date: 2026-01-03T03:25:31.387111
---

# Hexo 版本更新与技术债务

[茅聙赂忙聙聺忙聺聜茅聶聢](/)

盲潞潞莽卤禄盲赂聙忙聙聺猫聙聝茂录聦盲赂聤氓赂聺氓掳卤氓聫聭莽卢聭茫聙聜

* [茅娄聳茅隆碌](/)
* [莽录聳莽篓聥](/categories/programming/)
* [氓聢聠莽卤禄](/categories/)
* [忙聽聡莽颅戮](/tags/)
* [氓陆聮忙隆拢](/archives/)
* [氓聟鲁盲潞聨](/about/)
* 忙聬聹莽麓垄

* 忙聳聡莽芦聽莽聸庐氓陆聲
* 莽芦聶莽聜鹿忙娄聜猫搂聢

1. [1. 盲戮聺猫碌聳忙聸麓忙聳掳](#%E4%BE%9D%E8%B5%96%E6%9B%B4%E6%96%B0)
2. [2. 茅垄聺氓陇聳猫庐戮莽陆庐](#%E9%A2%9D%E5%A4%96%E8%AE%BE%E7%BD%AE)
   1. [2.1. hexo](#hexo)
   2. [2.2. next](#next)
      1. [2.2.1. 猫聡陋氓庐職盲鹿聣忙聽路氓录聫](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%A0%B7%E5%BC%8F)
      2. [2.2.2. 氓聫陋氓聹篓忙聳聡莽芦聽茅隆碌忙聵戮莽陇潞盲戮搂猫戮鹿忙聽聫](#%E5%8F%AA%E5%9C%A8%E6%96%87%E7%AB%A0%E9%A1%B5%E6%98%BE%E7%A4%BA%E4%BE%A7%E8%BE%B9%E6%A0%8F)
      3. [2.2.3. 猫聡陋氓庐職盲鹿聣茅聡聧氓庐職氓聬聭](#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E5%AE%9A%E5%90%91)
      4. [2.2.4. 忙聸麓忙聧垄 gitalk 盲赂潞 utterances](#%E6%9B%B4%E6%8D%A2-gitalk-%E4%B8%BA-utterances)
3. [3. 忙聸麓忙聧垄莽录聳猫戮聭氓聶篓](#%E6%9B%B4%E6%8D%A2%E7%BC%96%E8%BE%91%E5%99%A8)
4. [4. 忙聞聼氓聫聴](#%E6%84%9F%E5%8F%97)

Jay.Run

氓颅娄盲鹿聽 忙聙聺猫聙聝 猫隆聦氓聤篓 Python

[170
忙聴楼氓驴聴](/archives/)

[4
氓聢聠莽卤禄](/categories/)

[88
忙聽聡莽颅戮](/tags/)

[GitHub](https://github.com/ruanimal "GitHub 芒聠聮 https://github.com/ruanimal")

[Leetcode](https://leetcode-cn.com/u/iponder "Leetcode 芒聠聮 https://leetcode-cn.com/u/iponder")

茅聯戮忙聨楼

* [氓禄聳茅聸陋氓鲁掳莽職聞氓庐聵忙聳鹿莽陆聭莽芦聶](https://www.liaoxuefeng.com/ "https://www.liaoxuefeng.com/")
* [茅聵庐盲赂聙氓鲁掳莽職聞盲赂陋盲潞潞莽陆聭莽芦聶](http://www.ruanyifeng.com/ "http://www.ruanyifeng.com/")
* [mac猫陆炉盲禄露忙聬聹莽麓垄氓录聲忙聯聨](https://cse.google.com/cse?cx=006975008861986536211:lpx3qasaiom "https://cse.google.com/cse?cx=006975008861986536211:lpx3qasaiom")
* [Kaciras' Blog](https://blog.kaciras.com/ "https://blog.kaciras.com/")

# Hexo 莽聣聢忙聹卢忙聸麓忙聳掳盲赂聨忙聤聙忙聹炉氓聙潞氓聤隆

氓聫聭猫隆篓盲潞聨
2026-01-02

氓聢聠莽卤禄盲潞聨

[莽录聳莽篓聥](/categories/programming/)

忙聹卢忙聳聡氓颅聴忙聲掳茂录職
1.6k

茅聵聟猫炉禄忙聴露茅聲驴 ≈
6 氓聢聠茅聮聼

忙聵篓氓陇漏猫聤卤盲潞聠盲潞聸忙聴露茅聴麓氓掳聠 Hexo + next 莽職聞氓聧職氓庐垄莽聣聢忙聹卢忙聸麓忙聳掳盲潞聠盲赂聥茂录聦氓聫聭莽聨掳盲赂聧氓掳聭茅聴庐茅垄聵茫聙聜
氓聧職氓庐垄氓路虏莽禄聫氓戮聢氓陇職氓鹿麓盲潞聠茂录聦盲赂聙莽聸麓莽聰篓莽職聞忙聵炉忙聹聙氓录聙氓搂聥莽職聞Hexo莽聣聢忙聹卢茂录聦nodejs 盲鹿聼忙聵炉 v12茂录聦next 盲赂禄茅垄聵盲鹿聼忙聵炉忙聴漏氓鹿麓忙潞聬莽聽聛氓庐聣猫拢聟莽職聞茂录聦猫驴聵氓聹篓盲赂聤茅聺垄氓聛職盲潞聠盲驴庐忙聰鹿茫聙聜
盲鹿聼忙聵炉忙聴漏氓掳卤忙聵聨莽聶陆茅聹聙猫娄聛忙聸麓忙聳掳盲潞聠茂录聦盲陆聠忙聵炉猫搂聣氓戮聴茅潞禄莽聝娄茂录聦氓掳卤盲赂聙莽聸麓忙虏隆氓聤篓茂录聦忙聹聙氓聬聨氓聫聵氓戮聴莽聸赂氓陆聯茅潞禄莽聝娄茫聙聜

## 盲戮聺猫碌聳忙聸麓忙聳掳

茅娄聳氓聟聢忙聵炉莽聣聢忙聹卢氓聧聡莽潞搂茂录聦氓聟聢氓掳聠 nodejs 氓聢聡忙聧垄氓聢掳忙聹聙忙聳掳 lts茂录聢v22茂录聣茫聙聜
氓聠聧氓掳聺猫炉聲盲陆驴莽聰篓npm氓掳聠 package.json 盲赂颅莽職聞氓潞聯猫聡陋氓聤篓忙聸麓忙聳掳茂录聦盲陆聠莽聰卤盲潞聨莽聣聢忙聹卢氓路庐猫路聺氓陇陋氓陇搂盲禄楼氓聫聤氓戮聢氓陇職氓潞聯盲赂聧莽禄麓忙聤陇盲潞聠茂录聦忙聸麓忙聳掳盲赂聧盲潞聠茫聙聜

忙聹聙氓聬聨氓聫陋猫聝陆盲陆驴莽聰篓 hexo init 忙聳掳氓禄潞盲赂陋氓聧職氓庐垄茅隆鹿莽聸庐茂录聦氓掳聠茅聡聦茅聺垄莽職聞盲戮聺猫碌聳茅隆鹿氓陇聧氓聢露氓聢掳猫聙聛氓聧職氓庐垄茅聡聦茂录聦氓聢聽茅聶陇氓炉鹿氓潞聰莽職聞猫聙聛盲戮聺猫碌聳茫聙聜
氓聣漏盲赂聥莽職聞盲戮聺猫碌聳茂录聦氓陇搂茅聝篓氓聢聠忙聵炉氓庐聣猫拢聟莽職聞忙聫聮盲禄露茂录聦氓聫陋猫聝陆盲赂聙盲赂陋盲赂陋忙聬聹莽麓垄莽聰篓茅聙聰茂录聦莽聹聥莽聹聥忙聹聣忙虏隆忙聹聣忙聸驴盲禄拢忙聢聳猫聙聟猫驴聵猫娄聛盲赂聧猫娄聛莽聰篓茫聙聜
package.json 盲赂颅莽職聞氓聟露盲禄聳氓聫聜忙聲掳茂录聦氓聫聜猫聙聝忙聹聙忙聳掳莽職聞茅聟聧莽陆庐忙聳聡盲禄露茂录聦氓掳聠忙聴聽莽聰篓茂录聢猫驴聡忙聴露茂录聣莽職聞猫庐戮莽陆庐茅隆鹿氓聢聽茅聶陇茫聙聜

盲陆驴莽聰篓 npm 氓庐聣猫拢聟 next 盲赂禄茅垄聵茂录聦氓掳聠氓聨聼 themes/next 盲赂颅莽職聞茅聟聧莽陆庐忙聳聡盲禄露氓陇聧氓聢露氓聢掳 \_config.next.yml茫聙聜
氓炉鹿忙炉聰 hexo 氓聮聦 next 氓庐聵忙聳鹿 \_config 忙聳聡盲禄露茂录聦莽聸赂氓潞聰盲驴庐忙聰鹿 \_config.yml 氓聮聦 \_config.next.yml茫聙聜
氓聢聽茅聶陇 themes 盲赂颅茅聡聧氓陇聧莽職聞盲赂禄茅垄聵莽聸庐氓陆聲茫聙聜

盲陆驴莽聰篓 npm install 氓庐聣猫拢聟盲戮聺猫碌聳茂录聦盲驴聺猫炉聛猫聝陆氓陇聼忙颅拢氓赂赂莽聰聼忙聢聬氓聧職氓庐垄茫聙聜

氓聢聽茅聶陇 node\_modules茂录聦忙聰鹿莽聰篓 pnpm 莽庐隆莽聬聠盲戮聺猫碌聳茂录聦忙聸麓猫陆禄茅聡聫氓驴芦茅聙聼茫聙聜
盲陆驴莽聰篓 pnpm 忙聴露氓聫炉猫聝陆盲录職忙聤楼茅聰聶茂录聦氓娄聜忙聼聬盲赂陋氓潞聯莽職聞 `index.node` 忙聣戮盲赂聧氓聢掳茂录聦忙聵炉氓聸聽盲赂潞 pnpm 茅禄聵猫庐陇盲赂聧忙聣搂猫隆聦氓潞聯莽職聞 post install 猫聞職忙聹卢茫聙聜
茅聹聙猫娄聛氓掳聠猫驴聶盲潞聸氓潞聯忙路禄氓聤聽氓聢掳 pnpm.onlyBuiltDependencies 盲赂颅茫聙聜

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` "pnpm": {   "onlyBuiltDependencies": [     "hexo-word-counter",     "hexo-util"   ]  } ``` |

ps:

* 忙聙禄盲陆聯忙聺楼猫炉麓 pnpm 猫驴聵忙聵炉氓颅聵氓聹篓盲赂聙盲潞聸氓聟录氓庐鹿忙聙搂茅聴庐茅垄聵茂录聦盲陆聠氓陇搂茅聝篓氓聢聠茅聝陆盲赂聧忙聵炉 pnpm 忙聹卢猫潞芦莽職聞茅聴庐茅垄聵茂录聦氓聼潞忙聹卢盲赂聤茅聝陆忙聵炉氓潞聯莽職聞氓陆卤氓颅聬盲戮聺猫碌聳茫聙聜
* 氓陇搂忙篓隆氓聻聥氓聹篓猫驴聶莽搂聧莽聳聭茅職戮忙聺聜莽聴聡盲赂聤氓赂庐氓聤漏氓戮聢氓陇搂

## 茅垄聺氓陇聳猫庐戮莽陆庐

### hexo

盲驴庐忙聰鹿 scaffolds 忙聳聡莽芦聽忙篓隆忙聺驴忙聳聡盲禄露
猫聙聛莽聣聢忙聹卢 hexo 莽職聞 scaffolds/page.md 莽颅聣忙聳聡盲禄露忙聽录氓录聫盲赂聨忙聳掳莽聣聢盲赂聧氓聬聦茂录聦盲录職氓炉录猫聡麓盲赂聙盲潞聸氓路楼氓聟路盲赂聧猫聝陆猫炉聠氓聢芦 front-matter, 茅聹聙猫娄聛忙聸麓忙聳掳茫聙聜

猫聙聛莽聣聢忙聹卢 front-matter 氓聣聧茅聺垄氓掳聭盲潞聠氓录聙氓陇麓莽職聞 `---`茂录聦氓炉录猫聡麓 vscode 莽職聞 hexo-util 忙聫聮盲禄露盲赂聧猫聝陆猫炉聠氓聢芦 tag 莽颅聣盲驴隆忙聛炉茂录聦猫掳聝猫炉聲盲潞聠氓戮聢盲鹿聟茫聙聜

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 ``` | ``` # 猫聙聛莽聣聢忙聹卢 title: {{ title }} date: {{ date }} ---  # 忙聳掳莽聣聢忙聹卢 --- title: {{ title }} date: {{ date }} --- ``` |

### next

猫聡陋氓庐職盲鹿聣忙聽路氓录聫氓聮聦猫聞職忙聹卢茂录聦盲鹿聥氓聣聧忙聵炉茅聙職猫驴聡盲鹿聥茅聴麓盲驴庐忙聰鹿盲赂禄茅垄聵忙潞聬莽聽聛氓庐聻莽聨掳茂录聦莽聨掳氓聹篓盲陆驴莽聰篓 custom\_file\_path 忙鲁篓氓聟楼猫聡陋氓庐職盲鹿聣氓聠聟氓庐鹿茫聙聜
茅聹聙猫娄聛盲驴庐忙聰鹿 \_config.next.yml 忙聳聡盲禄露

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` custom_file_path:   head: source/_data/head.njk   bodyEnd: source/_data/body-end.njk   style: source/_data/styles.styl ``` |

#### 猫聡陋氓庐職盲鹿聣忙聽路氓录聫

next 盲赂禄茅垄聵莽職聞 Muse scheme 忙聲麓盲陆聯忙炉聰猫戮聝莽卢娄氓聬聢忙聢聭莽職聞氓聫拢氓聭鲁茂录聦盲陆聠茅聝篓氓聢聠忙聽路氓录聫猫驴聵茅聹聙猫娄聛氓戮庐猫掳聝茫聙聜

source/\_data/styles.styl

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 ``` | ```  // 猫聡陋氓聤篓莽禄聶忙聳聡莽芦聽莽職聞 toc 氓聮聦忙颅拢忙聳聡忙聽聡茅垄聵莽录聳氓聫路, 盲赂聧氓炉鹿茅娄聳茅隆碌莽录聳氓聫路 body {counter-reset: h1} .post-block .post-body h2 {counter-reset: h2} .post-block .post-body h3 {counter-reset: h3} .post-block .post-body h4 {counter-reset: h4} .post-block .post-body h5 {counter-reset: h5} .post-block .post-body h6 {counter-reset: h6}  .post-block .post-body h2:before {   counter-increment: h1;   content: counter(h1) ". " } .post-block .post-body h3:before {   counter-increment: h2;   content: counter(h1) "." counter(h2) " " } .post-block .post-body h4:before {   counter-increment: h3;   content: counter(h1) "." counter(h2) "." counter(h3) " " } .post-block .post-body h5:before {   counter-increment: h4;   content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) " " } .post-block .post-body h6:before {   counter-increment: h5;   content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) " " } .post-block .post-body h1.nocount:before, .post-block.post .post-body h2.nocount:before, .post-block.post .post-body h3.nocount:before, .post-block.post .post-body h4.nocount:before, .post-block.post .post-body h5.nocount:before, .post-block.post .post-body h6.nocount:before {   content: "";   counter-increment: none }  // 茅職聬猫聴聫忙聳聡莽芦聽 toc 盲赂颅忙聳聡莽芦聽莽聸庐氓陆聲 tab 盲赂颅莽職聞氓聫聥茅聯戮盲驴隆忙聛炉茂录聦莽芦聶莽聜鹿忙娄聜猫搂聢盲赂颅莽職聞盲驴聺莽聲聶 .sidebar .sidebar-toc-active + .sidebar-blogroll {     display: none } ``` |

#### 氓聫陋氓聹篓忙聳聡莽芦聽茅隆碌忙聵戮莽陇潞盲戮搂猫戮鹿忙聽聫

next 盲赂禄茅垄聵莽職聞盲戮搂猫戮鹿忙聽聫茅聙禄猫戮聭忙聹聣莽聜鹿茅聴庐茅垄聵茂录聦 氓掳卤莽庐聴猫庐戮莽陆庐 sidebar.display 盲赂潞 post茂录聦氓陆聯盲禄聨忙聳聡莽芦聽猫路鲁氓聢掳氓聟露盲禄聳茅隆碌茅聺垄忙聴露茂录聦盲戮搂猫戮鹿忙聽聫氓鹿露盲赂聧盲录職猫聡陋氓聤篓氓聟鲁茅聴颅茂录聦忙聣聙盲禄楼氓聫陋猫聝陆茅聙職猫驴聡猫聞職忙聹卢忙聺楼莽禄聲猫驴聡茫聙聜

source/\_data/body-end.njk

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 ``` | ``` <script>   document.addEventListener('pjax:success', () => {     if (CONFIG.sidebar.display !== 'remove') {       const hasTOC = document.querySelector('.post-toc:not(.placeholder-toc)');        // 氓陆聯忙虏隆忙聹聣 TOC 盲赂聰盲戮搂猫戮鹿忙聽聫忙颅拢氓聹篓忙聵戮莽陇潞忙聴露茂录聦茅職聬猫聴聫盲戮搂猫戮鹿忙聽聫       if (!hasTOC && document.body.classList.contains('sidebar-active')) {         window.dispatchEvent(new Event('sidebar:hide'));       }     }   }); </script> ``` |

#### 猫聡陋氓庐職盲鹿聣茅聡聧氓庐職氓聬聭

hexo 莽職聞忙聳聡莽芦聽盲赂聧忙聰炉忙聦聛氓陇職盲赂陋氓聟楼氓聫拢茂录聦忙炉聰氓娄聜忙聢聭忙聝鲁猫娄聛氓聢聠莽卤禄盲赂颅猫聥卤忙聳聡茅聝陆氓聫炉盲禄楼猫庐驴茅聴庐茂录聦茅禄聵猫庐陇忙聝聟氓聠碌忙聵炉氓聛職盲赂聧氓聢掳莽職聞茫聙聜

氓聹篓猫庐戮莽陆庐盲潞聠 category\_map 莽職聞忙聝聟氓聠碌盲赂聥茂录聦hexo 氓聫陋盲录職莽聰聼忙聢聬 programming 莽颅聣猫聥卤忙聳聡茅聯戮忙聨楼茂录聦忙聴聽忙鲁聲氓聬聦忙聴露盲驴聺莽聲聶盲赂颅忙聳聡url盲陆聹盲赂潞氓聟楼氓聫拢茫聙聜

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` category_map:   莽录聳莽篓聥: programming   茅職聫莽卢聰: writing   茅聵聟猫炉禄: reading   氓路楼盲陆聹莽聰聼忙麓禄: life ``` |

忙聣聙盲禄楼忙聹聙莽禄聢茅聙職猫驴聡氓聹篓 404 茅隆碌茅聺垄忙路禄氓聤聽猫聡陋氓庐職盲鹿聣莽職聞 js 氓庐聻莽聨掳茅聡聧氓庐職氓聬聭茅聙禄猫戮聭茫聙聜

source/\_data/head.njk

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 ``` | ``` {% if page.title === '404 Page Not Found' or page.type === '404' or page.layout === '404' %} <script> (function() {   // 盲禄聨 Hexo 茅聟聧莽陆庐盲赂颅猫聨路氓聫聳 category_map   var categoryMap = {{ config.category_map | dump | safe }};   var currentPath = decodeURIComponent(window.location.pathname);    // 忙拢聙忙聼楼猫路炉氓戮聞盲赂颅忙聵炉氓聬娄氓聦聟氓聬芦氓聢聠莽卤禄忙聵聽氓掳聞莽職聞key   for (var key in categoryMap) {     if (currentPath.includes(key)) {      ...