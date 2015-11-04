智课前端优化指南
=======

>对于前端优化的方案，我们参照的是雅虎前端优化[Best Practices for Speeding Up Your Web Site](https://developer.yahoo.com/performance/rules.html),他们还推出了一款专门测试性能的工具[Yslow](Yslow http://developer.yahoo.com/yslow/),该工具Chrome和firefox都有对应的插件。

[中文翻译，分析](http://www.cnblogs.com/chenxizhang/archive/2013/05/16/3082402.html)

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

[Webkit技术笔记(2):详解 webkit 网页渲染过程]http://www.w3ctech.com/topic/281
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/01/3053216.html)

####6.Put Scripts at the Bottom  脚本放在底部
tag: javascript
[js和css的 加载顺序](http://hikejun.com/blog/2012/02/02/js和css的顺序关系/)
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/01/3053299.html)

####7.Avoid CSS Expressions  避免CSS表达式
tag: css
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/01/3053439.html)

####8.Make JavaScript and CSS External  使用js和css外链方式
tag: javascript, css
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/01/3053562.html)

####9.Reduce DNS Lookups  减少DNS查找
tag: content

* 由于DNS查找是需要时间的，而且它们通常都是只缓存一定的时间，所以应该尽可能地减少DNS查找的次数。
* 减少DNS查找次数，最理想的方法就是将所有的内容资源都放在同一个域(Domain)下面，这样访问整个网站就只需要进行一次DNS查找，这样可以提高性能。
* 但理想总归是理想，上面的理想做法会带来另外一个问题，就是由于这些资源都在同一个域，而HTTP /1.1 中推荐客户端针对每个域只有一定数量的并行度（它的建议是2），那么就会出现下载资源时的排队现象，这样就会降低性能。
* 所以，折衷的做法是：建议在一个网站里面使用至少2个域，但不多于4个域来提供资源。我认为这条建议是很合理的，也值得我们在项目实践中去应用。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/02/3053996.html)

####10.Minify JavaScript and CSS  缩小js和CSS
tag: javascript, css

工具推荐:
[JSMin](http://crockford.com/javascript/jsmin)
[YUI Compressor](http://refresh-sf.com/yui/)

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/02/3054141.html)

####11.Avoid Redirects  避免重定向
tag: content

重定向是经常发生的。有两种主要的情况下会发生重定向

* 服务器本身的一些行为（针对某类请求）
* 程序中明确地做了重定向

#####如何尽可能避免重定向

1.在定义链接地址的href属性的时候，尽量使用最完整的、直接的地址。例如

* 使用www.cnblogs.com 而不是cnblogs.com
* 使用cn.bing.com 而不是bing.com
* 使用www.google.com.hk 而不是google.com
* 使用www.mysite.com/products/ 而不是 www.mysite.com/products

2.在使用Response.Redirect的时候，设置第二个参数为false

* 考虑是否可用Server.Execute代替
* 考虑Respone.RedirectPermanent

3.如果涉及到从测试环境到生产环境的迁移，建议通过DNS中的CNAME的机制来定义别名，而不是强制地重定向来实现

[优化网站设计（十一）：避免重定向](http://www.cnblogs.com/chenxizhang/archive/2013/05/05/3060804.html)
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/05/3060804.html)

####12.Remove Duplicate Scripts  删除重复脚本
tag: javascript

* 避免重复加载相同的文件

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/05/3061004.html)

####13.Configure ETags  配置ETag
tag: server
[阅读资料](http://div.io/topic/854)

对于Expires,Cache-Control,ETag, 这几个技术其实很多时候是会结合起来用的，而且优先级也有所不同。通常，ETag是优先于Cache-Control的，而Cache-Control又是优先于Expires的.

![缓存对比](http://images.cnblogs.com/cnblogs_com/skynet/201211/201211281402442505.png)

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/11/3072898.html)

####14.Make Ajax Cacheable 使Ajax可缓存
tag: content

* POST的请求，是不可以在客户端缓存的，每次请求都需要发送给服务器进行处理，每次都会返回状态码200。（这里可以优化的是，服务器端对数据进行缓存，以便提高处理速度）

* GET的请求，是可以（而且默认）在客户端进行缓存的，除非指定了不同的地址，否则同一个地址的AJAX请求，不会重复在服务器执行，而是返回304。
post请求不可缓存，get请求自动缓存

cache: false

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/12/3073690.html)

####15.Flush the Buffer Early 尽早刷新缓冲区
tag: server

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/12/3073836.html)

####16.Use GET for AJAX Requests 使用Ajax的GET请求
tag: server

当你只需要请求数据，而不是将数据发送到存储服务器端。就尽量使用GET请求

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/12/3073891.html)

####17.Post-load Components  延迟或按需加载
tag: content

* 渐进增强
* 延迟加载
* 按需加载


[实现参考-YUI:GET](http://yuilibrary.com/yui/docs/get/)

引入Yui

```
<script src="http://yui.yahooapis.com/3.18.1/build/yui/yui-min.js"></script>
```
动态地，按需加载脚本和样式表

```
<script>
// Create a new YUI instance and populate it with the required modules.
YUI().use('get', function (Y) {
    // Get is available and ready for use. Add implementation
    // code here.
});
</script>
```

加载js()

```
// Load a single JavaScript resource.
Y.Get.js('http://example.com/file.js', function (err) {
    if (err) {
        Y.log('Error loading JS: ' + err[0].error, 'error');
        return;
    }

    Y.log('file.js loaded successfully!');
});
```

加载css()

```
// Load a single CSS resource.
Y.Get.css('file.css', function (err) {
    if (err) {
        Y.log('Error loading CSS: ' + err[0].error, 'error');
        return;
    }

    Y.log('file.css loaded successfully!');
});
```

load()

```
// Load multiple JS resources and execute the callback after all have finished
// loading.
Y.Get.js(['one.js', 'two.js', 'http://example.com/three.js'], function (err) {
    if (err) {
        Y.Array.each(err, function (error) {
            Y.log('Error loading JS: ' + error.error, 'error');
        });
        return;
    }

    Y.log('All JS files loaded successfully!');
});
```


[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/16/3081941.html)
[按需加载图片实例](http://yuilibrary.com/yui/docs/imageloader/basic-features.html)

####18.Preload Components  预加载
tag: content

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/16/3082402.html)

####19.Reduce the Number of DOM Elements 减少DOM元素的数量
tag: content

* YUI的CSS实战
* grids.css， fonts.css, reset.css

![DOM树](http://www.w3school.com.cn/i/ct_htmltree.gif)

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/17/3083162.html)

####20.Split Components Across Domains  拆分文件到不同域名下-使用多鸽主机来平衡负载
tag: content

* 最大限度的提高并行下载。
* 确保使用的并行下载是不超过2-4个域名。（DNS查找限制）

如果我们仅仅是希望按照资源的类型，使用不同的服务器来处理（多服务器可以提高整体性能，也便于管理），但又不想增加多个主机。例如

* A服务器专门存放图片
* B服务器专门存放脚本
* C服务器存放网页

那么，按照我们之前提到的做法，可能会下面这样规划

* 创建images主机，指向A服务器
* 创建scripts主机，指向B服务器
* 创建www主机，指向C服务器

但实际上，还有一种解决方案，就是采用微软提供的Application Request Routing（ARR)模块，它里面包含了负载均衡，反向代理等功能，可以较为方便地实现这个需求

* 创建www主机，指向一台ARR Server Farm，这个Farm里面包含三个服务器(A,B,C)
* 配置路由规则，使得所有的图片请求都路由到A服务器，脚本请求都路由到B服务器，网页请求都路由到C服务器。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/17/3083509.html)

####21.Minimize the Number of iframes 嵌入最少的iframes
tag: content

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/17/3083788.html)

####22.No 404s 没有404
tag: content

* 找不到资源文件
* 原本的网页已经不存在了
* 已经定义的页面写错了
* 自定义404页面

* http请求是昂贵的，影响用户体验

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/17/3084052.html)

####23.Reduce Cookie Size  减少cookie大小
tag: cookie

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/18/3085712.html)

####24.Use Cookie-free Domains for Components
tag: cookie

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086514.html)

####25.Minimize DOM Access
tag: javascript

* 永远使用最新的JQuery版本。
* 合并并最小化脚本。
* 尽量使用for，而不是each。
* 尽量使用ID去访问，而不是class。
* 如果有可能，通过提供上下文，缩小查找范围。例如 $(expression, context)。
* 对一个元素的多次访问（尤其在循环里面），可以考虑先用一个变量将其缓存起来，而不是每次都重新查找它。应该尽可能使用到jQuery的链式选择方法。
在可能的情况下，尽量减少动态插入、添加、删除元素。（我知道你很多时候做不到这一条）
* 对于要拼接大量字符串的情况，可以考虑使用join方法，而不是concat函数，或者+= 这样的运算符。
* 为所有事件的处理函数都返回false。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086546.html)

####26.Develop Smart Event Handlers
tag: javascript

* 冒泡和事件代理
* 使用委托
* 更合理的处理我们页面中的事件

```
<body>
    <form id="form1" runat="server">
        <div id="testdiv">
            <input type="button" value="button 1" />
            <input type="button" value="button 2" />
            <input type="button" value="button 3" />
            <input type="button" value="button 4" />
            <input type="button" value="button 5" />
            <input type="button" value="button 6" />
            <input type="button" value="button 7" />
        </div>
    </form>

    <script src="Scripts/jquery-2.0.0.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $("input[type=button]").click(function (event) {
                alert("Button Clicked : " + $(this).val());
            });
        });
    </script>

</body>
```
```
<body>
    <form id="form1" runat="server">
        <div id="testdiv">
            <input type="button" value="button 1" />
            <input type="button" value="button 2" />
            <input type="button" value="button 3" />
            <input type="button" value="button 4" />
            <input type="button" value="button 5" />
            <input type="button" value="button 6" />
            <input type="button" value="button 7" />
        </div>
    </form>

    <script src="Scripts/jquery-2.0.0.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $("#testdiv").click(function (event) {
                var bt = $(event.target);
                alert("Div Clicked : " + bt.val());

            });
        });
    </script>

</body>
```

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086643.html)

####27.Choose <link> over @import
tag: css

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086725.html)

####28.Avoid Filters
tag: css

针对ie的属性，现在可以不用考虑

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086817.html)

####29.Optimize Images
tag: images

* PNG取代GIF
* 图片颜色比较单一的用png
* 图片颜色比较丰富的用jpg

[在线批量压缩图片工具-TinyPng](https://tinypng.com/)

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086865.html)

####30.Optimize CSS Sprites
tag: images

* 尽可能地使用横向组合图片，这比纵向组合图片通常体积更小一些。
* 尽量使图片具有接近的色系，这样最终组合出来的图片也会小一些。
* 尽量使用小一些的图片，并且图片之间的间隙尽量也小一些，目的还是为了最终组合出来的图片体积更小。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086895.html)

####31.Don't Scale Images in HTML  不要在页面中缩放图片
tag: images

设置一个元素宽度和高度不要用一个更大的图像。如果你需要
<IMG WIDTH =“100”HEIGHT =“100”SRC =“mycat.jpg”ALT =“我的猫”/>
那么你的图像（mycat.jpg）应100x100px，而不是一个按比例缩小500x500px的图像。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/19/3086912.html)

####32.Make favicon.ico Small and Cacheable 让favicon.ico更小，可缓存
tag: images

![](https://dn-shimo-image.qbox.me/ddKs0ioWy5JKKyeT.png!thumbnail)

* 控制在1K
* 设置Expires会让人感到舒服（如果决定改变它，就不要重命名）。可以安全地设置了过期在未来的头几个月。您可以查看当前的favicon.ico的最后修改日期，以做出明智的决定。

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/20/3087997.html)

####33.Keep Components under 25K 在移动端，内容保持小于25K
tag: mobile

[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/20/3087983.html)

####34.Pack Components into a Multipart Document
tag: mobile

* 可以在Html的Img对象中使用，例如  

```
<img src="data:image/x-icon;base64,AAABAAEAEBAAAAAAAABoBQAAF..." />
```

* 可以在Css的background-image属性中使用，例如

```
div.image {
  width:100px;
  height:100px;
  background-image:url(data:image/x-icon;base64,AAABAAEAEBAAAAAAAABoBQAAF...);
}
```

* 可以在Html的Css链接处使用，例如

```
<link rel="stylesheet" type="text/css"
  href="data:text/css;base64,LyogKioqKiogVGVtcGxhdGUgKioq..." />
```

* 可以在Html的Javascript链接处使用，例如

```
<script type="text/javascript"
  href="data:text/javascript;base64,dmFyIHNjT2JqMSA9IG5ldyBzY3Jv..."></script>
```
这里的一个小的难点，就是如何将图片、样式表、脚本生成为base64的字符串。好在有一些不错的工具，例如：

[在线转base64](http://www.greywyvern.com/code/php/binary2base64)
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/20/3087997.html)

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
 
[阅读资料](http://www.cnblogs.com/chenxizhang/archive/2013/05/20/3088007.html)
