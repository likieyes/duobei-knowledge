# 前端测试研究探索 #

方案还在探索中，还没有一个清晰的计划：

1.google：jsTestDriver **有待探索**

http://oss.org.cn/?action-viewnews-itemid-62878 不支持老的一些浏览器如IE7 demo没跑起来

2.yahoo：YUI **未探索**

http://developer.yahoo.com/yui/yuitest/

http://yuilibrary.com/projects/yuitest/

http://www.oschina.net/p/yuitest

https://github.com/yui/yuitest

3.taobao：cloudRun和totoro

cloudRun已经不维护了

https://github.com/totorojs/totoro  有资源404，demo没跑起来

4.selenium **看的蛋碎了,还继续探索么，谁有兴趣我们一起？**

5.phantomJs **探索中**

http://phantomjs.org/

6.grunt

利用grunt-contrib-jshint，不过我们得升级grunt到0.4.0。前端写完自己的页面的js后，可以修改下grunt.js文件，然后用jshint检测js代码。可检测js代码细节层面的语法错误或书写错误

利用grunt-contrib-qunit，进行单元测试。

## 建议 ##

欢迎大家给建设性意见

需要去和其他公司的测试沟通取经

## 补充 ##

使用nodejs进行前端测试：https://github.com/assaf/zombie
