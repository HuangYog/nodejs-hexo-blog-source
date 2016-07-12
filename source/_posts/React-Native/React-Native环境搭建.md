---
title: React-Native环境搭建
date: 2016-07-12 14:51:56
tags:
 - React-Native
 - React
categories:
 - 日志
 - 笔记
---
## 一 React Native配置安装
React-Native 是什么这里就不多说了(是伟大的facebook 开源项目),这里就直接开始，直入主题。
{% link React-Native 项目的官网 http://facebook.github.io/react-native/ %}
{% link React-Native 项目源码 https://github.com/facebook/react-native %}
React-Native 需要依赖 Nodejs，所以事先我们需要安装Nodejs，这里推荐大家先安装 NVM (Nodejs Version Manage) 通过nvm来安装管理Nodejs具体步奏如下:
  1.下载安装nvm
  {%codeblock%}
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash
  {%endcodeblock%}
  安装完成后需要刷一下环境变量，我的环境是Linux(ubunut)所以只需如下操作:
  {%codeblock%}
  $ source ~/.bashrc
  {%endcodeblock%}
  之后在检查一下nvm是否已安装成功:
  {%codeblock%}
  $ nvm --version
  {%endcodeblock%}

  2.安装完成nvm后就可以用nvm命令来按装nodejs了，命令如下:
  {%codeblock%}
  $ nvm list //检查已安装的版本，->指向哪个版本就是当前使用的版本，
  $ nvm install 6.3.0 // 按装版本号为V6.3.0的nodejs
  $ nvm use 4.4.7 // 修改默认使用的nodejs为4.4.7版本的(前提是已经安装了4.4.7版本的nodejs)
  {%endcodeblock%}
  完成后再是一下是否已成功安装nodejs:
  {%codeblock%}
  $ node --version
  $ npm --version
  {%endcodeblock%}

  3.接下来我们需要安装facebook 的Watchman,只是用于监控bug 和出发指定操作的。{% link watchman 官方地址 https://facebook.github.io/watchman %}
  {%codeblock%}
  $ sudo git clone https://github.com/facebook/watchman.git
  $ cd watchman
  $ sudo ./autogen.sh
  $ sudo ./configure
  $ sudo make
  $ sudo make install
  {%endcodeblock%}
  如果在这步安装watchman报错处理如下:
    错误一{%codeblock%}
      ./autogen.sh: 9: ./autogen.sh: aclocal: not found
      ./autogen.sh: 10: ./autogen.sh: autoheader: not found
      ./autogen.sh: 11: ./autogen.sh: automake: not found
    {%endcodeblock%}
    处理方式是 {%codeblock%}
      $ sudo apt-get install autoconf
    {%endcodeblock%}
    错误二: 如果报错新增里有关python的错误那处理方式是{%codeblock%}
      $ sudo apt-get install python-dev
    {%endcodeblock%}

  4.安装folw，这是一个 JavaScript 的静态类型检查器，建议安装它{% link flow官方地址 https://www.flowtype.org %}
  {%codeblock%}
  $ npm install folw-bin -g
  {%endcodeblock%}

  5.安装 React-Native
  {%codeblock%}
$ npm install -g react-native-cli
  {%endcodeblock%}
  这里完成后就可以验证一下是否成功了用下面的命令创建一个React-Native工程{%codeblock%}$ react-native init RNAppProject{%endcodeblock%}
  这一步需要漫长的时间，慢慢等吧。
  项目创建完成后生成的目录如下
  {% img /images/2016-07-1216-07-18RNApp.png react-native项目目录结构 %}
  - android和ios文件夹是为android studio 和 Xcode创建的项目
  - index.android.js和index.ios.js文件为Android和IOS的空壳应用文件
  - node_modules文件夹为Node.js包，含React Native框架包。
  6.测试 Android应用
    - 检查手机是已经通过adb连接到PC上(命令行输入 adb devices看看)
    - adb reverse tcp:8081 tcp:8081 配置Android设备8081端口，访问PC的8081端口的React服务
    - cd RNAppProject,路径切换到项目主目录
    - sudo react-native start 启动PC端的React服务
    - react-native run-android进行加载运行android 应用
