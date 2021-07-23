# 反爬虫之字体反爬虫
## 0x0 前言
   反爬虫和爬虫之间的较量已经争斗多年，不管是攻还是守，已经持续N年，这是一个没有硝烟的战场，大家都知道爬虫和反爬之家的道高一尺魔高一丈的关系。但这个方案可以很大程度上可以增加普通爬虫的采集成本，在不使用OCR的前提下，算是比较极致的方案了。当然方案有很多种，层出不穷的各种方法，这里介绍的时候反爬虫的中的一种比较实用的方案，字体反爬也就是自定义字体反爬通过调用自定义的ttf文件来渲染网页中的文字，而网页中的文字不再是文字，而是相应的字体编码，通过复制或者简单的采集是无法采集到编码后的文字内容！必须通过程序去处理才能达到采集成本。
![-w677](http://mweb.03sec.com/2019-02-15-15502104553537.jpg)
效果展示！

## 0x1 思路

  细心的人会问，为什么不把所有的内容都替换成编码呢？这个就涉及到加载和渲染速度的问题。还有如果启动字体反爬虫，基本上已经告别SEO了，请仔细考虑中间的厉害关系，你懂得!
 我们知道，单纯汉字就有好几千个，还有各种字符，有的还包含各种外国人的字符串！如果全部放到自定义字体库中的话，这个文件灰常大，几十兆是肯定有的了，那后果啥样就很清楚了，加载肯定很慢，更糟糕的是如此之多的字体需要浏览器去渲染，那效果，卡到爆！！！

  为了解决这个问题，我们可以选择只渲染少量的、部分的文字，假设50个字，那么字体库就会小到几十K了，相当于一个小图片而已，加上CDN加速之类的，解决了。具体网络上又N种方法参考方法我会贴在下面！

  如此简单？50个字儿呢可不是随便随便选择的，要选择那些爬虫采集不到就会很大改变整个语句的语义的词，直接点吧，也就是量词、否定词之类的。如原文“我有一头朱佩琪，我从来都不骑”，我们把其中的“一”、“不”放到我们的自定义字库中，这样一来，爬虫采集到的就是“我有头朱佩琪，我从来都骑”，嘿嘿，如果加上数字就更叼了，“漏洞版本 .. ???“ 是不是很猥琐。


  但是上述方法早已让网络各位大神破解一遍了，这可如何是好呢？办法总是有的！如果让“叼”字的编码随机变化，但字体信息不变，浏览器渲染出来还是“叼”字那不就完美了，
于是，每个网页加载的时候，都随机加载了一套字体库，字体库的内容还是50个字，但每个字的顺序编码都是变化的，虽然我们打乱了关键字的编码顺序，但是每个字对应的字体信息是不变的，例如，“是”字一共有9划，每一笔划都有相应的x、y坐标信息，浏览器正是根据这些笔划信息渲染
![-w518](http://mweb.03sec.com/2019-02-15-15502123184600.jpg)

如果吧，unicode编码和x，y坐标都骚做改动。他需要采集我的每一套字体库并且建立关系，这样增加的爬虫的成本，美滋滋。

## 0x3 实现
 基于微软雅黑字库信息，抽取其中的关键字的字体信息，生成ttf 后 使用下方代码 开始随机然后随机生成上千套字库，后文章显示时随机从文件或者裤中查询出一套字库，并把文章中的关键字替换成Unicode编码进行渲染！

![-w872](http://mweb.03sec.com/2019-02-15-15502140214430.jpg)


> 上述代码中 不带x，y混淆，请自己修改代码谢谢,如有需要请联系我！

## 0x4 参考
https://www.jianshu.com/p/4d28dd440cdd
https://github.com/FantasticLBP/Anti-WebSpider

