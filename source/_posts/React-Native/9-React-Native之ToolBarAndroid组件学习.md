---
title: 9.React-Native之ToolBarAndroid组件学习
date: 2016-07-19 10:30:31
tags:
- React-Native
- ToolBarAndroid
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1.`ToolBarAndroid`介绍
`ToolBarAndroid`组件是对Android平台的ToolBar组件进行封装，一个ToolBar可以显示一个`Logo图标`以及一些导航图片(如菜单功能按钮)，一个标题以及一个副标题还有一系列的功能列表，标题和副标题是上下位置显示，logo图标和导航图标显示在左边，标题和副标题显示在中间，功能列表显示在右边。
`如果ToolBar只有一个子节点，那就会显示在标题和功能列表之间`
`特别声明` 尽管`ToolBar`的logo图标，导航图标以及功能列表的图标支持加载远程的图片，但是那也只是在Dev模式(开发模式)中支持。但是在Release模式(发布模式)中，应该只使用应用中的资源来进行渲染，例如使用`require('./imgs/title.png')`会自动帮我们进行加载资源，所以尽量不要在开发中使用`{uri:http://...}`.

### 2.属性方法
1.该组件的会全部继承`View`组件的相关属性样式(宽和高,背景颜色,边距等相关属性样式)
2. `actions` 设置功能列表信息属性，传入的参数信息为[{title: string, icon: optionalImageSource, show: enum('always', 'ifRoom', 'never'), showWithText: bool}]进行设置功能菜单中的可用的相关功能，该功能会显示在组件的右侧(显示方向为图标或者文字)，如果界面上的区域已经放不下就会加入到隐藏的菜单中(弹出进行显示)，该属性的值是一组对象数组，每个对象包含一下一些参数:
  - `title` 必须的，该功能的标题
  - `icon` 功能图标，采用`require('./imgs/logo.png')`进行获取
  - `show` 表示设置的图标是显示，还是隐藏或者显示在弹出菜单中，`always`表示总是显示，`ifRoom`表示如果Bar中控件够显示，`never` 表示直接不显示
  - `showWithText` boolean 进行设置图标旁边是否要显示标题信息
3. `contentlnSetEnd` number 用于设置ToolBar的右边和屏幕右边缘的间距
4. `contentlnStartEnd` number 用于设置ToolBar的左边和屏幕左边缘的间距
5. `logo` optionalImageSource 可选图片资源，用于设置ToolBar 的Logo图标
6. `navIcon` optionalImageSource 可选图片资源用于设置导航图标
7. `onActionSelected` function 方法 当我我的功能被选中的时候回调方法，该方法只会传入唯一一个参数，点击功能在功能列表中的索引信息
8. `onIconClicked` function 当图标被选中的时候回调方法
9. `overFlowIcon` optionalImageSource 可选图片资源，设置功能列表中弹出菜单中的图标
10. `rtl` 设置 ToolBar 中的功能顺序是从右到左(RTL : Right To Left) ,为了让该效果生效你必须在Android应用中的`AndroidMainifest.xml` 中的`application`节点中添加`android:supportRtl` ,然后在你的主Activity (例如： MainActivity)的onCreate方法调用如下代码: setLayoutDirection(LayoutDirection.RTL).
11. `subtitle` string 设置toolbar 的副标题
12. `subtitleColor` color 设置toolbar副标题的颜色
13. `title` string 设置toolbar标题
14. `titleColor` color 设置toolbar 标题的颜色

### 3. `ToolBarAndroid` 简单实例
1. 简单的显示ToolBar 的标题/副标题以及功能列表，导航图片，代码如下:
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

var ToolbarAndroid = require('ToolbarAndroid');
class RNAppToolBar extends Component {

  render() {
    return (
        <ToolbarAndroid
            actions = {toolbarActions}
            navIcon = {require('./imgs/next_eight.png')}
            style={styles.toolBar}
            subtitle = '副标题'
            title = '主标题'
            onActionSelected = {this.onActionSelected}
        />
    );
  }
}

var toolbarActions = [
    {title:'Create', icon: require('./imgs/next_one.png'),show: 'always'},
    {title:'Filter'},
    {title:'Settings', icon: require('./imgs/next_nine.png'),show: 'always'}
];

const styles = StyleSheet.create({
  toolBar:{
      backgroundColor:'#e9eaed',
      height: 56
  }
});

AppRegistry.registerComponent('RNAppToolBar', () => RNAppToolBar);
```
运行效果如下:
{% img /images/2016-07-1914-00-18.png %}

2. 只设置标题以及功能列表，无导航图标效果代码如下:
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

var ToolbarAndroid = require('ToolbarAndroid');
class RNAppToolBar extends Component {

  render() {
    return (
        <ToolbarAndroid
            actions = {toolbarActions}
            style={styles.toolBar}
            title = 'only title'
        ></ToolbarAndroid>
    );
  }
}

var toolbarActions = [
    {title:'Create', icon: require('./imgs/next_one.png'),show: 'always'},
    {title:'Filter'},
    {title:'Settings', icon: require('./imgs/next_nine.png'),show: 'always'}
];

const styles = StyleSheet.create({
  toolBar:{
      backgroundColor:'#e9eaed',
      height: 56
  }
});

AppRegistry.registerComponent('RNAppToolBar', () => RNAppToolBar);
```
运行后看到的效果如下:
{%img /images/2016-07-1914-09-40.png %}

3. 只存在导航图标，logo图标以及功能列表代码如下:
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

var ToolbarAndroid = require('ToolbarAndroid');
class RNAppToolBar extends Component {

  render() {
    return (
        <ToolbarAndroid
            navIcon = {require('./imgs/app_logo.png')}
            logo = {require('./imgs/icon_settings.png')}
            actions = {toolbarActions}
            style={styles.toolBar}
        ></ToolbarAndroid>
    );
  }
}

var toolbarActions = [
    {title:'Create', icon: require('./imgs/next_one.png'),show: 'always'},
    {title:'Filter'},
    {title:'Settings', icon: require('./imgs/next_nine.png'),show: 'always'}
];

const styles = StyleSheet.create({
  toolBar:{
      backgroundColor:'#e9eaed',
      height: 56
  }
});

AppRegistry.registerComponent('RNAppToolBar', () => RNAppToolBar);
```
运行后效果如下:
{% img /images/2016-07-1914-12-40.png %}

4. ToolBarAndroid组件支持组件嵌套，下面是一个实现组件嵌套SwitchAndroid组件的实例，代码如下:
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

var ToolbarAndroid = require('ToolbarAndroid');
var SwitchAndroid = require('SwitchAndroid');
class RNAppToolBar extends Component {

  render() {
    return (
        <ToolbarAndroid
            navIcon = {require('./imgs/app_logo.png')}
            logo = {require('./imgs/next_four.png')}
            actions = {toolbarActions}
            style={styles.toolBar}
        >
            <SwitchAndroid value = {true} />
        </ToolbarAndroid>
    );
  }
}

var toolbarActions = [
    {title:'Create', icon: require('./imgs/next_one.png'),show: 'always'},
    {title:'Filter'},
    {title:'Settings', icon: require('./imgs/next_nine.png'),show: 'always'}
];

const styles = StyleSheet.create({
  toolBar:{
      backgroundColor:'#e9eaed',
      height: 56
  }
});

AppRegistry.registerComponent('RNAppToolBar', () => RNAppToolBar);
```
运行后得到的效果如下:
{% img /images/2016-07-1914-21-15.png %}
