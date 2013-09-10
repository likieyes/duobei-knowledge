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
**20130902** 在grunt中，新增加了一个task，命名为findUselessImg，会将静态资源中没有被引用的图片从目录中删除。具体做法：遍历静态资源的每个图片，然后遍历所有less和jsp资源，如果图片没有被任何less或jsp引用，则视为无用的图片，将其删除。

结果：全站静态资源758个，删除无用的459个。以后压缩图片的时间节省一大半。

**20130902-20130906** grunt在多贝升级为0.4的版本，解决了之前grunt版本无法合并压缩并混淆js的尴尬局面
- 做了比较完善的[demo](https://github.com/kbisnotzombie/grunt-fedeploy)版本，可在dev和www上部署。具体配置项等涉及多贝的信息未提交到项目上。
- 查看gruntjs.com中很多关于grunt的task和api的文档，grunt0.3和grunt0.4的版本在写task上有很多不同的地方，以前的部署配置在升级后需要重写。
 
**20130909** grunt在本地和dev升级测试，完成上次delay的计划。
- 压缩图片一开始使用imagemin，后来还是改为使用原来的smushit。一方面smushit是无损压缩，另一方面imagemin需要安装其他一大堆的依赖库等东西。
- 使用了压缩css的task。
- 在做页面的js合并压缩和混淆的时候，遇到bug。使用uglify来做混淆，当子task过多的时候会报错，task执行会自动中断，当时测试250+的子task就跪了，这是grunt的一个bug，暂时无解，后来优化了下混淆的代码，具体优化是：不是所有的jsp都需要压缩和混淆js的，当需要的jsp才做这些工作，因此加了判断该jsp是否需要合并压缩混淆js。这样uglify的子task数量下降到100以内，就没有问题了。经过测试在dev上部署ok。——该问题还是值得进一步考虑，当我们需要合并压缩混淆js的jsp页面很多很多的时候（超过250+），怎么办？我初步想法是，设定一个阀值，比如200，按照每两百一个批次来执行uglify task混淆，前200个执行完了，重写uglify的200个子task，再次执行uglify，依次类推，按照200批次执行混淆工作。