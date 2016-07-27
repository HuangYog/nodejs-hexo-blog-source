---
title: 18.React-Native之Clipboard组件学习
date: 2016-07-26 16:19:56
tags:
- React-Native
- Clipboard
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. Clipboard组件介绍
`Clipboard`组件可以复制信息保存到粘贴板中或者从粘贴板中取数据

### 2. Clipboard组件属性方法
  1. `setString(message:string)`  static 静态方法  直接Clipboard调用该方法，设置相关内从到粘贴板中
  2. `getString()`   static静态方法   直接Clipboard调用该方法，从粘贴板中获取内容

### 3. Clipboard组件实例演示
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    Clipboard,
    ToastAndroid
} from 'react-native';

class RNAppCilpboard extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            content: '需要保存的内容: '
        };
    }

    async _setClipboardContent() {
        Clipboard.setString('Heoo ,React-Native');
        try {
            var content = await Clipboard.getString();
            ToastAndroid.show('粘贴板上的内容为: ' + content, ToastAndroid.SHORT);
        } catch (e) {
            ToastAndroid.show(e.message, ToastAndroid.SHORT);
        }
    }

    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}>
                    Clipboard 粘贴板演示 setString, getString 方法
                </Text>
                <Text
                    onPress={this. _setClipboardContent}
                    style={styles.instructions}>
                    点击我 复制 'Heoo ,React-Native' 添加到粘贴板,弹出Tost显示内容
                </Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    welcome: {
        fontSize: 16,
        textAlign: 'left',
        margin: 10,
        marginLeft: 10
    },
    instructions: {
        textAlign: 'center',
        color: 'green',
        marginBottom: 5,
        fontSize:20
    },
});

AppRegistry.registerComponent('RNAppCilpboard', () => RNAppCilpboard);
```
运行后看到的效果如下图:
{% img /images/1260726002.gif %}
