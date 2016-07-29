---
title: 20.React-Native之StatusBar组件学习
date: 2016-07-28 21:44:28
tags:
- React-Native
- StatusBar
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. StatusBar组件介绍
`StatusBar`状态栏组件用来实现控制应用的状态栏的效果。根据官方文档说明了StatusBar(状态栏)和Navigator(导航器)搭配的用法：
  StatusBar组件可以同时加载多个组件，该组件的属性可以按照加载的顺序进行合并。一种常见的用法就是:我们可以在使用Navitator的时候，针对不同的路由页面进行设置特殊的状态栏样式。具体可以看一下官方实例代码:
```
<View>
  <StatusBar
    backgroundColor="blue"
    barStyle="light-content"
  />
  <Navigator
    initialRoute={{statusBarHidden: true}}
    renderScene={(route, navigator) =>
      <View>
        <StatusBar hidden={route.statusBarHidden} />
        ...
      </View>
    }
  />
</View>
```

### 2. StatusBar组件属性
  1. `animated` bool   进行设置当状态栏的状态发生变化的时候是否需要加入动画。当前该动画支持backgroundColor,barStyle和hidden属性
  2. `hidden`  bool  进行设置状态栏是否隐藏
  3. `backgroundColor`   color类型，仅支持Android设备，设置状态栏的背景颜色
  4. `translucent` bool类型，仅支持Android设备, 进行设置状态栏是否为透明。当状态栏的值为true的时候，应用将会在状态栏下面进行绘制显示。这样在Android平台上面就是沉浸式的效果，可以达到Android和iOS应用一致性效果。该值常常配置半透明效果的状态栏颜色一起使用
  5. `barStyle`  enum('default','light-content')  枚举类型，仅支持iOS设备。进行设置状态栏文字的颜色
  6. `networkActivityIndicatorVisible`   bool类型，仅支持iOS设备。设置状态栏上面的网络进度加载器是否进行显示
  7. `showHideTransition`   enum('fade','slide') 枚举类型，仅支持iOS设备。进行设置当隐藏或者显示状态栏的时候的动画效果。默认值为'fade'

### 2. StatusBar组件实例
具体实例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    StatusBar,
    TouchableHighlight
} from 'react-native';

class CustomButton extends React.Component {
    render() {
        return (
            <TouchableHighlight
                style={styles.button}
                underlayColor='#a5a5a5'
                onPress={this.props.onPress}>
                <Text >{this.props.text}</Text>
            </TouchableHighlight>
        );
    }
}

class RNAppStatusBar extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {};
    }

    render() {
        return (
            <View style={styles.container}>
                <StatusBar
                    backgroundColor='green'
                    translucent = {true}
                    hidden = {true}
                    andimated = {true}
                >
                <CustomButton text = '状态栏隐藏透明效果'/>
                </StatusBar>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    button: {
        margin: 5,
        backgroundColor: 'white',
        padding: 15,
        borderBottomWidth: StyleSheet.hairlineWidth,
        borderBottomColor: '#cdcdcd'
    }
});

AppRegistry.registerComponent('RNAppStatusBar', () => RNAppStatusBar);
```
运行后的效果就是手机的转态栏变透明了.
