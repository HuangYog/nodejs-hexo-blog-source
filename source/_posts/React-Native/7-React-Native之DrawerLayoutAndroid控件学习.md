---
title: 7.React-Native之DrawerLayoutAndroid控件学习
date: 2016-07-18 10:43:00
tags:
- React-Native
- DrawerLayoutAndroid
- React
categories:
- 日志
- 笔记
---
本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1.`DrawerLayoutAndroid`组件是对Android 平台的`DrawerLayout`控件的封装
`DrawerLayoutAndroid`组件是对Android 平台的`DrawerLayout`控件的封装，这个抽屉页面经常用于导航页面，通过renderNavigationView进行渲染，`DrawerLayoutAndroid`中的子视图会变成主视图.导航栏的视图在屏幕中一开始是隐藏的，但我们可以通过drawerPostition指定位置进行把导航视图拖拽出来，最终拖拽出来的距离大小可以使用drawWidth属性进行指定。
该控件试用起来比较简单，只需要熟悉一下其中的属性和方法即可，下面是一个官方的实例:
```
render() {
    var navigatiobView = (
        <View style = {{flex:1, backgroundColor:'#fff'}}>
          <Text style={{margin:10, fontSize: 15, textAlign:'left'}}>
            I'm in the Drawer !
          </Text>
        </View>
    );
    return (
        <DrawerLayoutAndroid drawerWidth = {300}
          drawerPosition = {DrawerLayoutAndroid.positions.Left}
          renderNavigationView = {() => navigatiobView}>
          <View style={{flex:1,alignItems:'center'}}>
            <Text style={{margin:10, fontSize:15,textAlign: 'right'}}>Hello</Text>
            <Text style={{margin:10, fontSize:15,textAlign:'right'}}>World !</Text>
          </View>
         </DrawerLayoutAndroid>
    );
  }
```
运行后看到的效果如下图所示:
{% img /images/2016-07-1811-44-17.png %}
在屏幕上用手指从屏幕边缘从左到右滑动之后看到的效果如下:
{% img /images/2016-07-1811-45-54.png %}

### 2.基本属性方法
1.继承View控件的属性(宽和高,背景颜色,边距等相关属性样式)
2.`drawerPosition` 参数为枚举类型(`DrawerConsts. DrawerPosition.Left`,`DrawerConsts.DrawerPosition.Right`),进行指定导航菜单用哪一侧滑行出来，更具官方实例，最终传入的两个枚举值分别为:`DrawerLayoutAndroid.positioons.Left`和`DrawerLayoutAndroid.positioons.Right`
3.`drawerWidth` 进行指定导航菜单视图的宽度，也就是说该侧面导航视图可以从屏幕边缘拖拽到屏幕的宽度距离
4.`keyboardDismissMode` 参数为枚举类型('none','on-drag')进行指定在导航视图拖拽的过程中是否要隐藏键盘:
  - `none` 不隐藏键盘(默认)
  - `on-drag` 当拖拽开始的时候进行隐藏键盘
5.`onDrawerClose` function 方法当导航视图被关闭后进行回调方法
6.``onDrawerOpen`` function 方法 当导航视图被打开后进行回调该方法
7.`onDrawerSlide` function 方法 当导航视图和用户进行交互时候调用该方法
8.`onDrawerStateChanged` function 方法，当导航视图状态发生变化的时候调用该方法，该方法会有三种状态:
  - `idle(空闲)` 表示导航视图上面没有任何交互状态
  - `dragging(正在拖拽中)` 表示该用户正在和导航视图发生交互动作
  - `setting(暂停-刚刚结束)`表示用户 刚刚结束和导航视图的交互动作，当前导航视图正在打开或者正常关闭拖拽滑动动画效果
9.`renderNavigationView` function 方法，该方法进行渲染一个导航抽屉视图(用于用户从屏幕边缘拖拽出来)

### 3.DrawerLayoutAndroid 使用实例
代码如下:
```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  DrawerLayoutAndroid
} from 'react-native';

class RNAppDrawerLayout extends Component {
  render() {
    var navigatiobView = (
        <View style = {{flex:3, backgroundColor:'green'}}>
          <Text style={{margin:10, fontSize: 15,color:'red', textAlign:'center'}}>
            导航功能标题栏
          </Text>
          <Text style={{margin:10, marginLeft: 20, fontSize: 15, color:'#fff',textAlign:'left'}}>
            1.功能一
          </Text>
          <Text style={{margin:10,marginLeft: 20, fontSize: 15, color:'#fff', textAlign:'left'}}>
            2.功能二
          </Text>
        </View>
    );
    return (
        <DrawerLayoutAndroid drawerWidth = {150}
          drawerPosition = {DrawerLayoutAndroid.positions.Left}
          renderNavigationView = {() => navigatiobView}>
          <View style={{flex:1,alignItems:'center'}}>
            <Text style={{margin:10, fontSize:15,textAlign: 'right'}}>主布局界面内容</Text>
          </View>
         </DrawerLayoutAndroid>
    );
  }
}
AppRegistry.registerComponent('RNAppDrawerLayout', () => RNAppDrawerLayout);
```
运行后能看到的效果如下:
1.主页面
{% img /images/2016-07-1815-10-32.png %}
2.导航栏
{% img /images/2016-07-1815-10-15.png %}
