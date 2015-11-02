目前面临的问题
=========

![Smartstudy loading icon](http://3861227.s21i-3.faiusr.com/3/ABUIABADGAAgwqHOsQUo8vWdNTBBOEQ.gif)

[前端工程与性能优化](https://github.com/fouber/blog/issues/3)

## 请求数量

![主站请求总数](http://3861227.s21i-3.faiusr.com/4/ABUIABAEGAAgrsjbsQUozrGZqQUw-Qk4lQU.png)

152个 requests
11.7MB transferred
2.6min finish time
1.61s DOMContentLoaded

总的加载时间26.74s


####合并脚本和样式表、压缩HTML文档
减少资源大小可以加快网页显示速度，所以要对HTML，CSS，Javascript等进行代码压缩，并在服务器端设置GZip. a) 压缩（例如，多余的空格，换行符和缩进） 

目前主站的代码压缩

* gzip压缩
* CSS Sprites

![雪碧图](http://media8.smartstudy.com/images/icons.png)

####页面的按需加载
  
* 划分主域
* 增加并发请求数

提供多个域名去分散加载页面页面的数据请求

## 请求带宽

####开启Gzip

我们的主站已经开启
  
####精简JavaScript

删除代码中不必要的字符，减少文件尺寸。能压既压的做法。缩小JavaScript代码的两个流程的工具是[JSMin](),[YUI]()压缩机，YUI压缩机也可以压缩CSS

可以审查代码，边审查边做
####图像优化

查看主站的图片
![](http://media8.smartstudy.com/media/pic/advertisement/c26a0ba268c111e59c5900163e0053c21443761710600752.jpg)
![](http://media8.smartstudy.com/media/pic/advertisement/47984ba668b611e5a05e00163e0053c21443756779469930.png)

banner图的设计就应该有章法可循

尺寸大小：1080px * 500px
给一个背景配色的选项

* 使用合理的图片格式


![](http://media8.smartstudy.com/media/pic/teacher/1439018156876.png)

本应该用jpg，竟然用成了png

* 能用css3实现就不要用图片
  
  图片是最占流量的资源，因此尽量避免使用他，使用时选择最合适的格式(实现需求的前提下，以大小判断)，合适的大小，然后使用智图压缩，同时在代码中用Srcset来按需显示

* 使用[智图](http://zhitu.isux.us/)

  最可取的方案是，我们后台技术能够实现，图片上传之后的自动压缩


* 使用其他方式代替图片(1. 使用CSS3 2.使用SVG 3.使用IconFont)

  对于字体图标的介绍，请移驾石墨文档[字体图标](https://shimo.im/doc/AO5dWuNB3Z1iobRu)


* 使用Srcset

主要是响应式的一些优化手段，现在也是基本的写法了

  <img class="image" src="mm-width-128px.jpg"
     srcset="mm-width-128px.jpg 128w, mm-width-256px.jpg 256w, mm-width-512px.jpg 512w"
     sizes="(max-width: 360px) 340px, 128px">
     
参考阅读请移步：[响应式图片srcset全新释义sizes属性w描述符](http://www.zhangxinxu.com/wordpress/2014/10/responsive-images-srcset-size-w-descriptor/)
     
* 选择合适的图片（1.webP优于Jpg 2.png8优于gif）

  webP是谷歌推出的一种新的图片格式，当然也不是很新了。

  ***图片对比：***
  

![图片](http://3861227.s21i-3.faiusr.com/2/ABUIABACGAAgsJ-QsQUoxrf45AYw9AM4qwI.jpg)

  webP:  33KB,   jpg: 72KB,   png: 228KB,   原图：754.3KB

  
* 选择合适的大小(1.首次加载不大于1024KB 2.宽度不宽于640)

由于我们的主站，是
  
## 缓存利用
缓存一切可以缓存的资源,使用长Cache(使用时间戳更新Cache)

####使用CDN

  目前的缓存路径：media8.smartstudy.com
  
####静态资源的版本控制和缓存更新

  之后偲哥会弄的吧
  
####减少DNS查找

缓存时间较长，有利于重复利用DNS缓存，提高速度。

缓存时间较短，有利于及时地检测到目标站点的IP地址更新，以进行正确的访问。
所以，两者都有其优点和考虑。

由于DNS查找是需要时间的，而且它们通常都是只缓存一定的时间，所以应该尽可能地减少DNS查找的次数。
减少DNS查找次数，最理想的方法就是将所有的内容资源都放在同一个域(Domain)下面，这样访问整个网站就只需要进行一次DNS查找，这样可以提高性能。

但理想总归是理想，上面的理想做法会带来另外一个问题，就是由于这些资源都在同一个域，而HTTP /1.1 中推荐客户端针对每个域只有一定数量的并行度（它的建议是2），那么就会出现下载资源时的排队现象，这样就会降低性能。

所以，折衷的做法是：建议在一个网站里面使用至少2个域，但不多于4个域来提供资源。我认为这条建议是很合理的，也值得我们在项目实践中去应用。

下面是![博客园的DNS请求](http://images.cnitblog.com/blog/9072/201305/02084629-0c89c8e9611e4153ac5129ac26c6b4b6.png)

阅读参考：[优化网站设计(九):减少DNS查找的次数](http://www.cnblogs.com/chenxizhang/archive/2013/05/02/3053996.html)


## 页面结构
写在HTML头部的javascript(无异步),和写在html标签中的style会阻塞页面的渲染，因此css放在页面头部并使用Link方式引入，避免在html标签中写style,javascript放在页面尾部或使用异步方式加载。

####将样式表放在顶部，脚本放在底部
####最先刷新出文档输出
####首屏加载

首屏的快速显示，可以大大提高用户对页面速度的感知，因此应尽量针对首屏的快速显示做优化。

####js异步加载
*同步加载方式

  <script src="http://XXX.com/script.js"></script>


*异步加载方式

它允许无阻塞资源加载，并且使 onload 启动更快，允许页面内容加载，而不需要刷新页面，也可以根据页面内容延迟加载依赖。

  (function() {  
     var s = document.createElement('script');  
     s.type = 'text/javascript';  
     s.async = true;  
     s.src = 'http://yourdomain.com/script.js';  
     var x = document.getElementsByTagName('script')[0];  
     x.parentNode.insertBefore(s, x);  
  })();

这种实现方式，可以用于视频加载，乐语加载，百度统计等

*延迟加载（lazy loading）

前面解决了异步加载（async loading）问题，再谈谈什么是延迟加载。

延迟加载：有些 js 代码并不是页面初始化的时候就立刻需要的，而稍后的某些情况才需要的。延迟加载就是一开始并不加载这些暂时不用的js，而是在需要的时候或稍后再通过js 的控制来异步加载。

也就是将 js 切分成许多模块，***页面初始化时只加载需要立即执行的 js ，然后其它 js 的加载延迟到第一次需要用到的时候再加载。***
特别是页面有大量不同的模块组成，很多可能暂时不用或根本就没用到。
就像图片的延迟加载，***在图片出现在可视区域内时（在滚动条下拉）才加载显示图片***

已经比较成熟的延迟加载插件：

####预加载

大型重资源页面(如游戏)可以增加Loading的方法，资源加载完成后再显示页面。但Loading时间过长，会造成用户流失。 对用户行为分析，可以在当前页加载下一页资源，提升速度。

*可感知Loading（如进入空间游戏的Loading）

*不可感知的Loading(如提前加载下一页)

## 代码校验

####避免CSS表达式

CSS表达式是一个功能强大的（和危险）的方式来动态设置CSS属性。他们支持在Internet Explorer开始用5版本，但被废弃的开始与IE8。作为一个例子，背景颜色可以被设置使用CSS表达式交替每隔一小时：

####避免重定向

## 代码性能

####高质量HTML
####高质量CSS
####高质量JavaScript

请移步到[高质量代码规范](http://qiuge.me/codeguide/)

## 工程化

####项目目录
####项目分工
####前端框架的搭建

## 性能工具
```

[Page Speed Online](https://developers.google.com/pagespeed/)

[Pingdom Tools](http://tools.pingdom.com/fpt/)

[Free Website Performance Test]()

[Which loads faster?]()

[WebPagetest](http://www.webpagetest.org/)

[Web Page Analyzer]()

[Show Slow](http://www.showslow.com/)

```

[Web Page Analyzer from Website Optimization] 一个很好的工具，它在分析完一个网页后，会为减少加载时间提出优化建议，着重优化物体的数目，图片和网站的总体大小。（强烈推荐）

[WebSitePulse Test Tools] 有一系列的工具来确定网站的加载速度和主机信息。

[Internet Supervision Url Check] 从世界各地不同的服务器来测试你的网站的加载时间，用于确定是不是各地的来访者都能顺利快速的打开你得网站。

[在线性能测试工具大全](http://ztc.iteye.com/blog/206856)

## 数据请求

目前主站的get请求接口11个。

* 随机获取教师：http://www.smartstudy.com/api/teachers?random=2&_=1445653254727
* 获取用户的课程详情：http://www.smartstudy.com/api/user/product/612977?token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取用户信息：http://www.smartstudy.com/login/userinfo?_=1445653194268
* 获取用户基本信息：http://www.smartstudy.com/api/user/?token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取我的练习数据：http://www.smartstudy.com/api/user/exerciselogs?page=1&token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取我的笔记数据：http://www.smartstudy.com/api/user/notes?page=1&token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取我的模考数据：http://practice.smartstudy.com/api/user/examinations?page=1&page_size=8&uid=4559200&token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取我的批改数据：http://www.smartstudy.com/api/user/pigailogs?page=1&token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8
* 获取课程的详细信息：http://www.smartstudy.com/api/user/product/586575?token=SPSUTJPDJMLDHGXHZ03GU4UW8EY2TXQ8








