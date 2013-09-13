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

**20130911-20130912** grunt升级后部署增加sourcemap
- 使用uglify压缩混淆js，但是需要解决线上js代码调试的问题，总不至于让人来看混淆后的代码吧。于是增加了sourcemap，会生成一个map文件，里面会存储压缩混淆前的源文件和目标文件之间混淆的一一对应关系。同时有了map文件，就可以查看js的源文件了。
- 在https://github.com/gruntjs/grunt-contrib-uglify 项目中有解释，我们需要用到sourceMap和sourceMapRoot两个参数，但是部署后在浏览器无法获得.map文件。原因是，执行uglify的子task的时候，压缩混淆的源地址是dev的前端项目的js路径，目标地址是dev下的前端项目js路径，但是压缩混淆完，所有的静态资源全部要copy到dev部署运行环境的目录下，这样压缩混淆完的js文件中.map文件的位置存的还是dev的前端项目的js路径（虽然.js和.map都被同时拷贝了过来，但是浏览器获得.map文件是根据.js中写的.map文件的路径来获取的），而不是真正部署环境的路径，最后当然找不到.map文件了。解决办法：在静态资源拷贝到部署运行环境的目录下后，再读取.js文件，修改文件中.map路径为生产环境的路径，ok。
- .map文件解决了，在浏览器无法获得压缩前的源文件，后来发现是已因为.map文件中的两个参数，sources和sourceRoot，从浏览器获取到压缩的源文件是根据这两个参数拼接的，即sourceRoot+sources，但是sourceRoot存储的是真正部署运行环境的路径，但是sources中存储的是执行uglify子task中压缩混淆的源地址（dev的前端项目的js路径），造成源文件的地址拼接出错。解决方案：在静态资源拷贝到部署运行环境的目录下后，再读取.map文件，修改文件中sources属性为生产环境的x相对路径（相对于sourceRoot），ok解决。
- 解决以上若干问题，基本算完成了升级，之后就在dev上开始各种乱测。