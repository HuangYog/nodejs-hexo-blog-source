---
title: android开发环境搭建
date: 2016-07-12 23:39:07
tags:
 - android
 - react-native
categories:
 - 日志
 - 笔记
---
要搭建 Android 的开发环境需要先搭建 java的开发环境，一下全是基于linux环境搭建基本步奏如下:
### 1.安装 git 版本管理工具
git是一个开源的分布式版本控制系统,{% link git 官网 https://git-scm.com/ %}，git的学习可以参考{% link 廖雪峰老师的git教程 http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 %}
{%codeblock%}
$ sudo add-apt-repository ppa:git-core/ppa //添加 git 的源
$ sudo apt-get update // 跟新系统软件
$ sudo apt-get install git 安装 git软件
{%endcodeblock%}
查看是否安装成功使用 `git --version`会吧git的版本打印出来表示安装成功。

### 2.安装java
java 我们选择 oracle发行的java(oracle 就 android 侵全案 败诉 google 所以不用担心使用android会换用openjdk)。先去下载个{%link jdk http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html %}在这里选择适合的`jdk`下载。我下载的是`.tar.gz`文件的jdk，然后解压到你喜欢的地方，在配置环境变量，这里只说一下jdk的环境变量的配置：
{%codeblock%}
$ sudo vim /etc/profile
// 添加jdk配置
export JAVA_HOME=/home/huangyong/develop/java/jdk1.8.0_91 //解压后的jdk所在目录
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
// 刷新下配置
$ source /etc/profile
{%endcodeblock%}
之后检查是否一配置成功 在终端输入`java -version`打印出与你配置的jdk相关信息表示配置成功。这里jdk的安装配置就搞定了。

### 3.安装Android SDK
安装Android 需要去google的Android网站去下载SDK，所以请自备梯子(目前的南海形式紧张，情有可原，话又说回来 `犯我中华者虽远必诛`中国 一寸也不能少)，{% link Android-SDK https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz%}下载后解压到喜欢的地方然后运行 `Android-SDK/tools/android` 文件，下载或是跟新需要的包和android版本。弄完后还是需要配置一下android的环境变量，一样的打开 `/etc/profile`文件 添加android 的配置
{%codeblock%}
export ANDROID_HOME=/home/huangyong/develop/android-sdk-linux
export PATH=$PATH:$ANDROID_HOME/platform-tools
{%endcodeblock%}
添加好后使之生效 `source /etc/profile`,然后验证下，需要有usb连接android手机和电脑,然后在终端输入 `adb devices` 查看是否打印连接的android手机。

### 4. 安装Gradle
android应用是用gradle构建的，所以还需要安装gradle，先去下载gradle 并解压到喜欢的目录，同样配置环境变量然后使之生效，配置环境变量如下:
{%codeblock%}
export GRADLE_HOME=/home/huangyong/develop/gradle-2.14
export PATH=$GRADLE_HOME/bin:$PATH
// 配置 gradle 默认的本地仓库
export GRADLE_USER_HOME=/home/huangyong/develop/Maven/.gradle
{%endcodeblock%}
立即生效`source /etc/profile` 再输入 `gradle -version` 查看。
### 5.安装 Android Studio
神马,你用的是eclipse? 有没有搞错啊，兄弟趁你还年轻赶紧换到 {%link Android Studio https://developer.android.com/studio/index.html %} 吧。
这个软件的安装很简单不用多介绍了吧，如果不清楚请自行`{% link bing  http://www.bing.com/ %}`，额你是不是有不知道神么是`bing`，那你还是用这个吧`baidu`。
