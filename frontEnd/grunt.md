# grunt研究计划 #

**主要是把对我们有用的插件能很好地运用到项目中，玩转插件**

1. 首先grunt升级到0.4.0
2. grunt-contrib-connect 开启web server链接
3. grunt-contrib-qunit qunit单元测试
4. grunt-concat-min.js 有初步demo 合并压缩js 需要`grunt-contrib-concat` `grunt-contrib-uglify`
5. grunt-contrib-watch 监测文件增加、改动、删除，可能要结合grunt-contrib-livereload 做浏览器reload
6. 其他的还在想。。。欢迎补充

##  目标 ##
- 能比之前前端部署更方便些，更快速
- 能为前端开发时提供一些便利，比如监测文件改动并适时刷新，减少开发人员不停手动刷新的繁琐工作
- 对前端性能有一定优化，比如js测试，保证代码正确和高效、合并压缩混淆js减少http请求

## 具体研究记录 ##
**20130902-20130906** grunt在多贝升级为0.4的版本，解决了之前grunt版本无法合并压缩并混淆js的尴尬局面