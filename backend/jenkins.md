@authors
----
+ [louiseliu](https://github.com/louiseliu)    
+ [likieyes](https://github.com/likieyes)

###简介
----
Jenkins，之前叫做Hudson，是基于Java开发的一种持续集成工具，用于监控秩序重复的工作，主要包括
  + 项目的自动部署
  + 项目自动化测试
  

###安装
----



###配置
----


###插件使用
----

测试：


### 注意
----
1. 需要启动后台脚本，发现启动无效

    https://wiki.jenkins-ci.org/display/JENKINS/ProcessTreeKiller

    官方解释
    
    ```
    If your build wants to leave a daemon running behind...
    A convenient way to achieve that is to change the environment variable BUILD_ID which Jenkins's ProcessTreeKiller is looking for. This will cause Jenkins to assume that your daemon is not spawned by the Jenkins build. For example:
    
    BUILD_ID=dontKillMe /usr/apache/bin/httpd
    
    ```
    

