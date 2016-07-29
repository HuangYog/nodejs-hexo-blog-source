---
title: 24.React-Native之Alert学习
date: 2016-07-29 15:13:28
tags:
- React-Native
- Alert API
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. Alert(弹出框)介绍
`Alert模块`是创建弹出一个弹框，显示一个标题以及相关信息内容。该弹出框可以提供一系列的可选按钮，点击任何一个按钮都会调用onPress方法并且关闭弹框。默认情况下，只会显示一个'确定'按钮。
该模块API是在Android、iOS平台通用的显示静态的弹框。如果需要显示一个提示框可以让用户输入相关信息的，{% link 详请查看AlertIOS https://github.com/facebook/react-native/blob/6f1417c84982e0705912b57bf9d1aaaf1476d7d9/Libraries/Utilities/AlertIOS.js %};该带输入框的弹框只适用于iOS平台.
Android平台的弹出框的按钮有'中间态','取消','确认'三种状态:
  1. 如果你只有指定了一个按钮，那么该为'positive' (例如:确定)
  2. 如果你指定了两个按钮，那么该会'negative','positive' (例如:确定，取消)
  3. 如果你指定了三个按钮，那么该会'neutral','negative','positive'(例如:稍后再说,'确定','取消')

### 2. Alert方法
static `alert(title:string,message?:string,buttons?:Buttons,type?:AlertType)`  该方法会Alert模块显示弹框的静态方法，有四个参数，分别为标题，信息，按钮，以及按钮的风格类型

### 3. Alert实例
示例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    Alert,
    TouchableHighlight,
    ToastAndroid
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

class RNAppAlert extends Component {
    render() {
        return (
            <View>
                <Text style={{height: 30, color: 'black', margin: 8}}>
                    弹出框实例
                </Text>
                <CustomButton
                    text = '点击弹出默认'
                    onPress={() => Alert.alert('退出','您确定要退出吗?')}/>

                <CustomButton
                    text = '点击弹出两个按钮'
                    onPress={()=>Alert.alert('提醒','确定退出吗?',[
                            {text:'取消',onPress:()=>ToastAndroid.show('你点击了取消~',ToastAndroid.SHORT)},
                            {text:'确定',onPress:()=>ToastAndroid.show('你点击了确定~',ToastAndroid.SHORT)}
                    ])}/>
                <CustomButton
                    text = '点击弹出三个按钮'
                    onPress={()=>Alert.alert('提醒','您确定要退出吗?',[
                            {text:'取消',onPress:()=>ToastAndroid.show('你点击了取消',ToastAndroid.SHORT)},
                            {text:'不确定',onPress:()=>ToastAndroid.show('你点击了不确定',ToastAndroid.SHORT)},
                            {text:'确定',onPress:()=>ToastAndroid.show('你点击了确定',ToastAndroid.SHORT)}
                    ])}/>
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

AppRegistry.registerComponent('RNAppAlert', () => RNAppAlert);
```
运行后的效果如下图:
{%img /images/2016-07-29-16-16.gif %}
