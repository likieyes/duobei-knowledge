#Hibernate Validator
##Overview
  JSR303 BeanValidator是JavaEE后期少有的实用规范了。既可以做REST/SOAP接口的参数校验，Spring MVC也与之绑定做了Form校验。

  [Hibernate Validator](http://www.hibernate.org/subprojects/validator) 是唯一成熟的实现，在标准校验规则之外还添加了自己的一些规则，详见其[参考手册](http://docs.jboss.org/hibernate/validator/4.2/reference/en-US/html/validator-usingvalidator.html#validator-defineconstraints-builtin)。

  Hibernate Validator有[中英文版本的参考手册](http://www.hibernate.org/subprojects/validator/docs).

  除了标准的简单使用，高级点还可以自定义规则， 自定义Class级别的规则(比如如果属性A非空，则属性B的值必须怎样怎样)，还有玩玩Validating groups(在不同的情况下，校验不同的属性)

##出错信息的输出与国际化
validate（Object）的返回值是Set<ConstraintViolation<?>>，每一个ConstraintViolation包含了出错的message和propertyPath，InvalidValue， 不同场景有不同的显示需求，麻烦主要在propertyPath的i18N翻译上。

1.与SpringMVC结合的Form校验
  因为出错信息显示在输入框的右侧，不需要显示出错的属性名，是最简单的场景。

2.Rest/SOAP等接口参数校验。
  出错返回信息的观众一般是开发人员而不是终端用户，它们不介意看英文，直接把propertyPath 和 默认的message拼接起来即可。如"loginName may not be empty."

3.需要显示给终端用户但只需显示一种语言。
  这时候，建议在用annotation定义规则的时候，同时定义中文的，包含propertyPath的出错信息。如
```java
   @NotBlank(message="Some Chinese message")
   private String loginName;
```

4.显示给终端用户且支持多种语言
  这时候有两种做法，一是Hibernate Validator支持的，在ValidationMessge.properties定义带propertyPath的key，如
```
  loginName.org.hibernate.validator.constraints.NotEmpty.message=User's Login Name can not be empty.
```
  定义得更细的时候,连class名也加上： 
```
 User.loginName.org.hibernate.validator.constraints.NotEmpty.message=User's Login Name can not be empty.
```

  另外一种是自行翻译propertyPath的i18N结果，然后将i18后的propertyPath与Message进行拼接。

###SpringSide Core BeanValidatorｓ
BeanValidatorｓ对上面的第２，３种出错信息的输出做了支持，从ConstraintViolation中抽取出相应的信息，然后一般再配合Collections的
ｃｏｎｖｅｒｔToString(collection, seperator) 或 ConvertToString(collection, prefix, postfix)输出成
"用户名不能为空,邮箱格式不正确" 或"< li >用户名不能为空</ li >< li >邮箱格式不正确</ li >"

提取函数包括了如下三种：
 * 1. List<String>, String内容为message
 * 2. List<String>, String内容为propertyPath + separator + message
 * 3. Map<propertyPath, message>

###全面国际化
Hibernate默认会载入/org/hibernate/validator/ValidationMesages和/ValidationMesages中的信息，而Locale则是使用系统的默认Locale。  
Spring的LocalValidatorFactoryBean能注入更多的Bundle位置，并从HttpRequest中获取Locale.  

自行实现的话，可以AggregateResourceBundleLocator绑定多个Bundel,用自定义的MessageInterpolator设定Locale.

##Tips
像@Size这类规则，如果数据本身为Null，是不会校验的，必须加上@NotNull

[返回参考手册](back-end.md)