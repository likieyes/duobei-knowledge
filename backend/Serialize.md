#Serialize(XML,JSON)

##Overview
  想当年XML是标准的持久化格式，而Java的JAXB标准就很好，不鼓励再使用别的如XStream的类库了。  

  后来全部都换成JSON了，好的类库有Jackson和Google的Gson。Gson的API简单而优雅，但是功能与性能都没有jackson强，所以SpringSide里选了Jackson。

  但在苛刻要求性能的地方，JSON还有两个明显浪费空间的地方：  
  一个是以字符串的形式来表达数字，所以有了bson之类的格式来改进这一点。  
  一个是内容本身是自表达的，属性旁边总跟着一个属性名，如果这个属性名很长的话就更要命了。  
  这既是JSON的优点(灵活)，也是缺点(浪费空间)，解决方式就是预定义Schema了，与JSON的优缺点调了个个，如Google的Protocol Buffers， FaceBook的Thirft， Hadoop的Avor, 将在下个迭代演示。

##Details
* [Jackson for JSON](JSON)
* [JAXB for XML](JAXB)

[返回参考手册](back-end.md)