雅虎前端优化的34条原则
=======

>对于前端优化的方案，我们参照的是雅虎前端优化[Best Practices for Speeding Up Your Web Site](https://developer.yahoo.com/performance/rules.html),他们还推出了一款专门测试性能的工具[Yslow](Yslow http://developer.yahoo.com/yslow/),该工具Chrome和firefox都有对应的插件。

###我们先来看一下雅虎给出的34优化原则：

####1.Minimize HTTP Requests 减少HTTP请求
tag: content

* 资源文件数量减少
* 简化设计
* 合并脚本
* 合并css
* 合并图片

####2.Use a Content Delivery Network  内容分发网络
tag: server

* 分散静态文件在网络中
* 服务器具有最少跳数的网络或服务器选择以最快的响应速度
* 切换到CDN是一个相对简单的方式

####3.Add an Expires or a Cache-Control Header  添加一个Expires或Cache-Control头
tag: server

* 对于静态内容：实现‘永不过期’的政策去设置足够长的Expires头，如果需要更改，就添加版本号，强制修改文件名。如：zhike_2.0.1.js
* 对于动态内容：使用适当的Cache-Control头部帮助浏览器进行资源请求

####4.Gzip Components Gzip组件
tag: server
####5.Put Stylesheets at the Top  样式表放在顶部
tag: css
####6.Put Scripts at the Bottom  脚本放在底部
tag: javascript
####7.Avoid CSS Expressions  避免CSS表达式
tag: css
####8.Make JavaScript and CSS External  使用js和css外链方式
tag: javascript, css
####9.Reduce DNS Lookups  减少DNS查找
tag: content
####10.Minify JavaScript and CSS  缩小js和CSS
tag: javascript, css
####11.Avoid Redirects  避免重定向
tag: content
####12.Remove Duplicate Scripts  删除重复脚本
tag: javascript
####13.Configure ETags  配置ETag
tag: server
####14.Make Ajax Cacheable 使Ajax可缓存
tag: content
####15.Flush the Buffer Early 尽早刷新缓冲区
tag: server
####16.Use GET for AJAX Requests 使用Ajax的GET请求
tag: server

雅虎邮件研究小组发现，使用XMLHttpRequest的时候，POST将在浏览器中实现为两个步骤：首先发送标题，然后发送数据。所以最好使用GET，只需要一个TCP数据包发送（除非你有很多的饼干）。最大URL长度在IE浏览器是2K，所以如果你发送超过2K的数据，你可能无法使用GET。

当你只需要请求数据，而不是将数据发送到存储服务器端。就尽量使用GET请求

####17.Post-load Components  请求加载
tag: content

* 渐进增强
* 延迟加载

####18.Preload Components  预加载
tag: content
####19.Reduce the Number of DOM Elements 减少DOM元素的数量
tag: content

* YUI的CSS实战
* grids.css， fonts.css, reset.css

####20.Split Components Across Domains  拆分文件到不同域名下
tag: content

* 最大限度的提高并行下载。
* 确保使用的并行下载是不超过2-4个域名。（DNS查找限制）

####21.Minimize the Number of iframes 嵌入最少的iframes
tag: content
####22.No 404s 没有404
tag: content

* http请求是昂贵的，影响用户体验

####23.Reduce Cookie Size  减少cookie大小
tag: cookie
####24.Use Cookie-free Domains for Components
tag: cookie
####25.Minimize DOM Access
tag: javascript
####26.Develop Smart Event Handlers
tag: javascript
####27.Choose <link> over @import
tag: css
####28.Avoid Filters
tag: css

####29.Optimize Images
tag: images
####30.Optimize CSS Sprites
tag: images
####31.Don't Scale Images in HTML  不要定义你的图片大小
tag: images

设置一个元素宽度和高度不要用一个更大的图像。如果你需要
<IMG WIDTH =“100”HEIGHT =“100”SRC =“mycat.jpg”ALT =“我的猫”/>
那么你的图像（mycat.jpg）应100x100px，而不是一个按比例缩小500x500px的图像。

####32.Make favicon.ico Small and Cacheable 让favicon.ico更小，可缓存
tag: images

* 控制在1K
* 设置Expires会让人感到舒服（如果决定改变它，就不要重命名）。可以安全地设置了过期在未来的头几个月。您可以查看当前的favicon.ico的最后修改日期，以做出明智的决定。

####33.Keep Components under 25K 在移动端，内容保持小于25K
tag: mobile
####34.Pack Components into a Multipart Document
tag: mobile
####35.Avoid Empty Image src  避免空得图片路径
tag: server

* ```<IMG SRC =“”/>```
* ```VAR IMG =新的图像();img.src =“”;```


这两种形式产生同样的效果：浏览器发出另一个请求到服务器。


通过发送大量的突发流量，特别是对于获得数百万每天页面访问量页面削弱你的服务器。

这种现象的根本原因是在浏览器中执行URI解析的方式。统一资源标识符 - 这一行为在RFC 3986中定义。当空字符串遇到为URI，它被认为是一个相对URI，并根据在第5.2节中定义的算法得到解决。这个具体的例子，一个空字符串，列在第5.4节。火狐，Safari和Chrome都正确地按照规范解决一个空字符串，而Internet Explorer正在与规范的早期版本解决它不正确，显然是行，RFC 2396 - 统一资源标识符（这是过时的由RFC 3986） 。因此从技术上讲，浏览器都在做他们应该做的事情来解决相对URI。的问题是，在这种情况下，空字符串显然是无意的。

HTML5增加了标签的src属性指示浏览器不作出第4.8.2额外要求的描述：

src属性必须存在，而且必须包含一个有效的URL引用非交互式，可选动画，图像既不是分页，也没有脚本的资源。如果该单元的基座URI是相同文档的地址，则该src属性的值不能为空字符串。
但愿，浏览器不会在将来这个问题。不幸的是，对于<SCRIPT SRC =''/>和<link的href =''/>没有这样的条款。也许还有时间作出这样的调整，以保证浏览器不会意外地实现此行为。


[中文翻译版：加快您的网站的最佳实践（](http://www.cnblogs.com/jiahaohk/archive/2009/08/29/1556330.html)



1111 |  111 |

## 请求数量
=====

####合并脚本和样式表、压缩HTML文档
减少资源大小可以加快网页显示速度，所以要对HTML，CSS，Javascript等进行代码压缩，并在服务器端设置GZip. a) 压缩（例如，多余的空格，换行符和缩进） b) 启用GZip

* gzip压缩
* CSS Sprites

####页面的按需加载
  
* 划分主域
* 增加并发请求数

提供多个域名去分散加载页面页面的数据请求

## 请求带宽
=====

####开启Gzip

我们的主站已经开启
  
####精简JavaScript

删除代码中不必要的字符，减少文件尺寸。能压既压的做法。缩小JavaScript代码的两个流程的工具是[JSMin](),[YUI]()压缩机，YUI压缩机也可以压缩CSS
####图像优化

* 使用合理的图片格式
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
=====
缓存一切可以缓存的资源,使用长Cache(使用时间戳更新Cache)

####使用CDN

  目前的缓存路径：media8.smartstudy.com
  
####静态资源的版本控制和缓存更新
  
####减少DNS查找

缓存时间较长，有利于重复利用DNS缓存，提高速度。

缓存时间较短，有利于及时地检测到目标站点的IP地址更新，以进行正确的访问。
所以，两者都有其优点和考虑。

那么，讲了这么多，了解这个只是对于我们网站设计和优化有何启示呢？

由于DNS查找是需要时间的，而且它们通常都是只缓存一定的时间，所以应该尽可能地减少DNS查找的次数。
减少DNS查找次数，最理想的方法就是将所有的内容资源都放在同一个域(Domain)下面，这样访问整个网站就只需要进行一次DNS查找，这样可以提高性能。

但理想总归是理想，上面的理想做法会带来另外一个问题，就是由于这些资源都在同一个域，而HTTP /1.1 中推荐客户端针对每个域只有一定数量的并行度（它的建议是2），那么就会出现下载资源时的排队现象，这样就会降低性能。

所以，折衷的做法是：建议在一个网站里面使用至少2个域，但不多于4个域来提供资源。我认为这条建议是很合理的，也值得我们在项目实践中去应用。

下面是![博客园的DNS请求](http://images.cnitblog.com/blog/9072/201305/02084629-0c89c8e9611e4153ac5129ac26c6b4b6.png)

阅读参考：[优化网站设计(九):减少DNS查找的次数](http://www.cnblogs.com/chenxizhang/archive/2013/05/02/3053996.html)


## 页面结构
=====
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
=====

####避免CSS表达式

CSS表达式是一个功能强大的（和危险）的方式来动态设置CSS属性。他们支持在Internet Explorer开始用5版本，但被废弃的开始与IE8。作为一个例子，背景颜色可以被设置使用CSS表达式交替每隔一小时：

####避免重定向

## 代码性能
=====

####高质量HTML
####高质量CSS
####高质量JavaScript

## 工程化
=====

####项目目录
####项目分工
####前端框架的搭建

## 性能工具
=====

[Page Speed Online](https://developers.google.com/pagespeed/)

[Pingdom Tools](http://tools.pingdom.com/fpt/)

[Free Website Performance Test]()

[Which loads faster?]()

[WebPagetest](http://www.webpagetest.org/)

[Web Page Analyzer]()

[Show Slow](http://www.showslow.com/)

[Web Page Analyzer from Website Optimization] 一个很好的工具，它在分析完一个网页后，会为减少加载时间提出优化建议，着重优化物体的数目，图片和网站的总体大小。（强烈推荐）

[WebSitePulse Test Tools] 有一系列的工具来确定网站的加载速度和主机信息。

[Internet Supervision Url Check] 从世界各地不同的服务器来测试你的网站的加载时间，用于确定是不是各地的来访者都能顺利快速的打开你得网站。

[在线性能测试工具大全](http://ztc.iteye.com/blog/206856)

## 数据请求
=====


> Right angle brackets &gt; are used for block quotes.



