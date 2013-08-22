## 性能优化
<div style="margin-top:15px;"></div>
* HTTP优化
 - 减少HTTP请求
 - 重定向优化
 - 避免死链、空链、404错误 
 - 尽早Flush Buffer Chucked
 - 指定http中的character编码
 
* 缓存优化
 - 设置过期时间（静态资源，时间长一点）
     - Last-modify cache - control 304 
 - 对缓存资源加URL唯一标识（后端）
 - 反向服务器代理缓存

* DNS优化
 - 减少DNS解析
 - 增加静态资源域名，实现并行下载
     - 域解析 html5（FF11/webkit...支持） 

* 服务器负载
 - CDN（资源分发网络，然用户网络更快）
 - cookie优化
     - 静态资源不妨cookie
 - Gzip (90%浏览器支持)
     - XML和JSON可以做，图片和pdf不用 
 - 压缩文件
     - 让代码更精致

* CSS
 - 样式表合并放在头部
 - CSS Sprite
     -  同事下载的放一起
     - gif 和 png优先
     - icon 放一起
     - 减少空白空间
     - 色彩想进的放一起，利于压缩
 - 使用外联CSS
 - CSS编码要点

* javascript
 - js合并放底部（行内）
 - js代码去重
 - 无阻塞加载脚本（外联、异步）
     - 使用XMLHttpRequest加载，用eval响应
     - 使用iframe加载（不提供，不推荐）
     - 使用script加载（创建标签,变src）
 - 外联js和行内js（位置）
 - js编码要点
* 图片相关优化
 - 图片压缩
      - 一般压缩成png8，但会失真，超过256不压缩
 - 图片合并
 - 图片缩放

* 其他优化

