---
title: 14.React-Native之WebView组件学习
date: 2016-07-21 09:36:37
tags:
- React-Native
- WebView
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}


### 1. WebView组件介绍
WebView组件是对 创建渲染一个原生的WebView进行加载一个网页。

### 2. WebView 属性方法
  1. 继承可以使用View组件的所有属性和样式`{% link 官方介绍 http://facebook.github.io/react-native/docs/view.html#content %}`
  2. `automaticallyAdjustContentInsets` boolean   设置是否自动调整内容
  3. `contentInset`  {top:number,left:number,bottom:number,right:number}  设置内容所占的尺寸大小
  4. `html`  string  WebView加载的HTML文本字符串(已过期,现在使用`source`)
  5. `injectJavaScript`  string 当网页加载之前进行注入一段js代码
  6. `onError` function  方法 当网页加载失败的时候调用
  7. `onLoad`  function 方法  当网页加载结束的时候调用
  8. `onLoadEnd` fucntion 当网页加载结束调用，不管是成功还是失败
  9. `onLoadStart`  function  当网页开始加载的时候调用
  10. `onNavigationStateChange` function方法  当导航状态发生变化的时候调用
  11. `renderError`  function  该方法用于渲染一个View视图用来显示错误信息
  12. `renderLoagin` function  该方法用于渲染一个View视图用来显示一个加载进度指示器
  13. `startInLoadingState`  boolean 控制加载指示器是否可以显示
  14. `url`  string  设置加载的网页地址(已过期,现在使用`source`)
  15. `allowsInlineMediaPlayback`  boolean   适合iOS平台，设置决定当使用HTML5播放视频的时候在当前页面位置还是使用原生的全屏播放器播放，默认值false。【注意】.为了让视频在原网页位置进行播放，不光要设置该属性为true，还必须要设置HTML页面中video节点的包含webkit-playsinline属性
  16. `bounces` boolean  适合iOS平台 设置是否有界面反弹特性
  17. `domStorageEnabled` boolean  适合Android平台 该只适合于Android平台，用于控制是否开启DOM Storage（存储）
  18. `javaScriptEnabled`  boolean  该适合于Android平台，是否开启javascript，在iOS中的WebView是默认开启的
  19. `onShouldStartLoadWithRequest`  function  该适合iOS平台，该允许拦截WebView加载的URL地址，进行自定义处理。该方法通过返回true或者falase来决定是否继续加载该拦截到请求
  20. `scalesPageToFit`  boolean  该适合iOS平台  用于设置网页是否缩放自适应到整个屏幕视图以及用户是否可以改变缩放页面
  21. `scrollEnabled`  boolean    该适合iOS平台 用于设置是否开启页面滚动

### 3. WebView 实例
  1. WebView组件最基本的使用方法，直接加载一个网页，具体代码如下:
  ```
  'use strict';
  import React, {Component} from 'react';
  import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
      WebView
  } from 'react-native';

  var DEFAULT_URL = 'http://www.oschina.net/';

  class RNAppWebView extends Component {
      render() {
          return (
              <View style={{flex: 1}}>
                  <Text style={{height: 40, backgroundColor: '#cc4125',alignSelf:'center'}}>
                      简单的网页显示
                  </Text>
                  <WebView
                      style={styles.webview_style}
                      source = {{uri:DEFAULT_URL}}
                      startInLoadingState = {false}
                      domStorageEnabled = {true}
                      javaScriptEnabled = {true}
                  />
              </View>
          );
      }
  }

  const styles = StyleSheet.create({
      webview_style: {
          backgroundColor: '#00ff00'
      }
  });

  AppRegistry.registerComponent('RNAppWebView', () => RNAppWebView);
  ```
{% img /images/2016-07-2110-44-51.png %}

2. WebView加载本地的HTML静态字符串，具体代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    WebView
} from 'react-native';
const HTML = `<!DOCTYPE html>\n
    <html>
    <head>
    <title>HTML字符串</title>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=320, user-scalable=no">
    <style type="text/css">
    body {
    margin: 0;
    padding: 0;
    font: 62.5% arial, sans-serif;
    background: #ccc;
}
h1 {padding: 45px;margin: 0;text-align: center;color: #33f;}
</style>
</head>
<body>
<h1>加载静态的HTML文本信息</h1>
</body>
</html>`;

class RNAppWebView extends Component {
    render() {
        return (
            <View style={{flex: 1}}>
                <Text style={{height: 30, backgroundColor: '#cc4125',alignSelf:'center'}}>
                    简单的网页显示
                </Text>
                <WebView
                    style={styles.webview_style}
                    source = {{html:HTML}}
                    startInLoadingState = {false}
                    domStorageEnabled = {true}
                    javaScriptEnabled = {true}
                />
            </View>
        );
    }
}
const styles = StyleSheet.create({
    webview_style: {
        backgroundColor: '#00ff00'
    }
});
AppRegistry.registerComponent('RNAppWebView', () => RNAppWebView);
```
运行效果如下:
{% img /images/2016-07-2111-05-23.png%}
