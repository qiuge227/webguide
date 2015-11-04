前端规范开发文档
========

本文档由智课前端团队整理发布

![团队成员](http://3861227.s21i-3.faiusr.com/2/ABUIABACGAAg9LfgsQUogavEKTD0Azhk.jpg)

![优化方案设计图](http://3861227.s21i-3.faiusr.com/4/ABUIABAEGAAgpvTQsQUo6rOujwQw4gs4tgU.png)

图片优化参数：

| 图片位置        | 图片规格           | 图片压缩大小  |  原图大小  |
| ------------- |:-------------:| -----:|
| 首页banner图      | 1080*500 | <100KB |
| 教师头像     | 350*350      |  <50KB |
| 课程详情页商品图 | 666*375     |  <70KB |
| 课程详情页介绍图 | 1040*375     |  <70KB |

首页：
![请求测试](http://3861227.s21i-3.faiusr.com/4/ABUIABAEGAAg5a7msQUogKrT-wEw4AU4nAY.png)
![请求测试](http://3861227.s21i-3.faiusr.com/4/ABUIABAEGAAg5q7msQUom9aXrgMw_wM4wwM.png)
![请求测试](http://3861227.s21i-3.faiusr.com/4/ABUIABAEGAAg6a7msQUo7YKLswcw8gY4kQM.png)

影响页面的因素有很多，图片确实是最紧迫的。

配合的方案：

1. 后台上传的图片格式的控制，未来实现自动化在线压缩平台
2. 权哥和偲哥这边实现自动化压缩脚本
3. 后台上传图片之前除了按照标准的格式大小去上传外，提前做好压缩处理
4. 前端切图的时候就做好压缩处理，再上传到CDN
5. 如果需要改变图片的格式，png->jpg.可能会影响到数据库的存储


压缩图片的在线工具：

[智图](http://zhitu.isux.us/)

[tinypng](http://tinypng.org/)



1. 压缩所有可以压缩的图片
2. 去掉首页大图轮播，改成CSS3实现
3. 修改页面不合理的布局，使用最少标签实现页面的快速渲染
4. 使用事件委托，尽量少的是使用js操作DOM元素
5. 重新优化首页和课程页面的结构，优化代码
6. 缓存利用
7. 协同合作--主站bug，性能优化
8. 每天都要review代码
9. 优化的代码从develop上面切分支， 共同维护optimizing的分支

总的优化原则，在不影响页面的功能的前提下去做最好的优化。
