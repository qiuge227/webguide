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

    可以精细到图片质量，格式，的压缩工具

[tinypng](http://tinypng.org/)

    可以批量压缩png和jpg格式图片

[美图秀秀(Window)](http://xiuxiu.web.meitu.com/)

[acdsee(Mac)](http://rj.baidu.com/soft/detail/29080.html?ald)

    可以批量修改图片的大小格式


一下所有安排都是在各位有任务的前提之下，并行去做的。
<table>
    <tr>
        <th>
            优化内容
        </th>
        <th>
            参与人
        </th>
        <th>
            时间
        </th>
    </tr>
    <tr>
        <td>
            图片压缩
        </td>
        <td>
            黄权，黄偲，张百鸽，侯月源
        </td>
        <td>
            本周六将CDN图片权哥压缩完
        </td>
    </tr>
    <tr>
        <td>
            去首页js大图轮播，去掉js复杂效果，采用CSS3实现。（优化首页js）
        </td>
        <td>
            胡志娟
        </td>
        <td>
            本周六完成
        </td>
    </tr>
    <tr>
        <td>
            修改页面不合理布局，使用最少的标签实现页面的快速渲染，首页，课程页，
            重新优化首页和课程页面的结构，优化代码
        </td>
        <td>
            梁少峰，单亚轩
        </td>
        <td>
            本周六完成
        </td>
    </tr>
    <tr>
        <td>
            使用事件委托，尽量少的使用js操作DOM元素
        </td>
        <td>
            席辉亮
        </td>
        <td>
            本周六完成
        </td>
    </tr>
    <tr>
        <td>
            缓存利用
        </td>
        <td>
            梁少峰，张百鸽
        </td>
        <td>
            本周六完成
        </td>
    </tr>
    <tr>
        <td>
            每天review代码
        </td>
        <td>
            梁少峰，张百鸽，胡志娟，席辉亮，单亚轩
        </td>
        <td>
            本周六完成
        </td>
    </tr>
</table>

总的优化原则

1. 在不影响页面的功能的前提下去做最好的优化。
2. 协同合作--主站bug，性能优化
3. 优化的代码从develop上面切分支， 共同维护optimizing的分支

