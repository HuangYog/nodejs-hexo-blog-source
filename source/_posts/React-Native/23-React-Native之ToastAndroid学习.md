---
title: 23.React-Native之ToastAndroid学习
date: 2016-07-29 15:12:05
tags:
- React-Native
- ToastAndroid
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. ToastAndroid API 介绍
`ToastAndroid`模块是把Android平台原生模块Toast封装成一个JS模块使用，来进行弹出一个toast消息。该模块有一个'show'方法会传入下面两个参数:
  `message`  string 字符串格式，设置要进行toast显示的文本    
  `duration`  int格式 toast消息弹出显示的时长。有两个可选值`ToastAndroid.SHORT`或者`ToastAndroid.LONG`

### 2. ToastAndroid 方法/属性
  方法:
    1. `show(message:string,duration:number)`  static 静态方法，该设置toast消息的弹出
  属性:
    1. `SHORT`  静态int值，表示toast显示较短的时间
    2. `LONG`   静态int值，表示otast显示较长的时间

### 3. ToastAndroid 实例
示例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    ToastAndroid,
    TouchableHighlight
} from 'react-native';

class CustomButton extends Component{
    render(){
        return(
            <TouchableHighlight
            style={styles.button}
            underlayColor='#a5a5a5'
            onPress={this.props.onPress}>
                <Text style={styles.buttonText}>{this.props.text}</Text>
            </TouchableHighlight>
        );
    }
}

class RNAppToastAndroid extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}>
                    点击弹出段时间的 Toast
                </Text>
                <CustomButton
                text = '点击弹出段时间的 toast'
                onPress={() => ToastAndroid.show('只是弹出来的toast,是短时间的', ToastAndroid.SHORT)}/>
                <Text style={styles.welcome}>点击弹出时间长的Toast</Text>
                <CustomButton
                text="点击弹出时间长的toast"
                onPress={() => ToastAndroid.show("点击弹出长时间的 Toast",ToastAndroid.LONG)}/>
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
        borderBottomColor: '#CDCDCD'
    }
});

AppRegistry.registerComponent('RNAppToastAndroid', () => RNAppToastAndroid);
```
运行后看到的效果如下图:
{% img /images/2016-07-29-15-22.gif 300 500 %}
