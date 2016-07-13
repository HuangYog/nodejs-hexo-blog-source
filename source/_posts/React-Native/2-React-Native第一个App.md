---
title: 2.React-Native第一个App
date: 2016-07-13 09:47:54
tags:
- React-Native
- React
categories:
- 日志
- 笔记
---
在上一篇文章中介绍了React-Native Android 开发环境的搭建，接下来就应该开始尝试Run一下了{% link React-Native 环境搭建 http://ty52u.cn/2016/07/12/React-Native/React-Native%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/ %}，
因为 Android项目是用 Gradle 构建打包，所以依赖Gradle，在我们用`react-native run-android`命令运行 RNAppProject 项目时 程序会自动去下载 Gradle， 默认会在`$User/.gradle/wrapper/dists` 里面，这个过程很漫长，所以我们得找找捷径，打开Adroid项目的目录可以发现有个在gradle/wrapper/有个叫`gradle-wrapper.properties`的文件如下图:{% img /images/2016-07-13 9-56-18.png %}
打开这个文件看以看到这里面有
{%codeblock%}
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.4-all.zip
{%endcodeblock%}
这不难看出 `distributionBase=GRADLE_USER_HOME`还记得之前我们配置过`GRADLE_USER_HOME`环境变量吗？没错就是表示gradle的本地仓储；`distributionUrl`表示需要从神么地方下载神么版本的gradle，其他的就不多解释了。
1.运行 `react-native run-android` 之前选运行`adb reverse tcp:8081 tcp:8081` ，如果这步报错出现 -error: device unauthorized.- 表示没有勾选手机的上的授权允许。勾选后在运行。
2.运行 `react-native run-android` 这步会去下很多的包，所以需要一段时间。
3.出现下面的界面中带有`BUILD SUCCESSFUL`说明android项目部署成功
{% img /images/2016-07-1313-59-1.png %}
4.注意这途中可能会出现各种奇葩的问题，当服务器显示部署成功后查看手机又傻b了，怎么是红屏的?这个问题一般需要两步就搞定了(android5.0以上的手机)
  (1) 先运行 adb reverse tcp:8081 tcp:8081
  (2) 运行 react-native start // 启动包服务
  (3) react-native run-android //搞定
5.开一下完整的页面如下:
{% img /images/1194432545.jpg %}
