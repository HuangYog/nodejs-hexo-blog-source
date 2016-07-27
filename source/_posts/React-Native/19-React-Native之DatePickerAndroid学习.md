---
title: 19.React-Native之DatePickerAndroid学习
date: 2016-07-27 21:21:35
tags:
- React-Native
- DatePickerAndroid
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. DatePickerAndroid组件介绍

`DatePickerAndroid`是Android系统原生的时间控件,`DatePickerAndroid`控件会进行加载打开一个标准的Android时间日期选择器弹框，注意该控件只适合Android平台.

### 2. DatePickerAndroid组件方法
  1. `Promise<Object> open(options:Object)`   `staitc`,静态方法，使用该方法进行加载弹出一个标准的Android时间日期选择器。该options(可选)参数有以下三种对象:
    - `:'date'`   日期Date对象或者时间戳以毫秒单位-日期已默认格式显示
    - `:'minDate'`  日期Date对象或者时间戳以毫秒单位-可以选择的最小时间
    - `:'maxDate'` 日期Date对象或者时间戳以毫秒单位-可以选择的最大时间
  该方法返回一个Promise对象，如果用户选择了日期，则会被一个包含'action'，'year','month(0-11)','day'的对象调用。如果用户关闭了选择器，那么该Promise对象将会使用'DatePickerAndroid.dismissedAction'调用
  [`注意`]: 原生的时间日期选择器当我们在使用'minDate'和'maxDate'参数的时候，可能在Android 4.0或者更低版本中出现界面不同的情况
  2. `dateSetAction()` ,static静态方法，选择时间日期选择器
  3. `dismissedAction()`,static静态方法，关闭时间日期选择器

### 3. DatePickerAndroid组件实例
下面用这个实例来对于DatePickerAndroid组件进行详细演示，具体实例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    DatePickerAndroid,
    TouchableHighlight
} from 'react-native';

class CustomButton extends Component {
    render () {
        return (
            <TouchableHighlight
                style={styles.button}
                underlayColor= '#a5a5a5'
                onPress={this.props.onPress}>
                <Text style={styles.buttonText}>{this.props.text}</Text>
            </TouchableHighlight>
        );
    }
}

class RNAppDatePickerAndroid extends Component {

    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            presetDate: new Date(2016, 3, 5),
            allDate: new Date(2020, 4, 5),
            simpleText: '选择日期,默认今天',
            minText: '选择日期,不能比今天早',
            maxText: '选择日期,不能比今天晚',
            presetText: '选择日期, 指定 2016/3/5'
        };
      }

      // 进行创建时间日期选择器
    async showPicker(stateKey, options){
        try{
            var newState = {};
            const {action, year, month, day} = await DatePickerAndroid.open(options);
            if(action === DatePickerAndroid.dismissedAction){
                newState[stateKey + 'Text'] = 'dismissed';
            } else{
                var date = new Date(year, month, day);
                newState[stateKey + 'Text'] = date.toLocaleString();
                newState[stateKey + 'Date'] = date;
            }
            this.setState(newState);
        }catch ({code, message}){
            console.warn(`Error in example '${stateKey}': `, message);
        }
    }

    render() {
        return (
            <View>
                <Text style={styles.welcome}>
                    日期选择器组件实例
                </Text>
                <TouchableHighlight
                    style={styles.button}
                    underlayColor="#a5a5a5"
                    onPress={this.showPicker.bind(this, 'simple', {date: this.state.simpleDate})}>
                    <Text style={styles.buttonText}>点击弹出基本日期选择器</Text>
                </TouchableHighlight>
                <CustomButton text={this.state.presetText}
                              onPress={this.showPicker.bind(this, 'preset', {date: this.state.presetDate})}/>
                <CustomButton text={this.state.minText}
                              onPress={this.showPicker.bind(this, 'min', {date: this.state.minDate,minDate:new Date()})}/>
                <CustomButton text={this.state.maxText}
                              onPress={this.showPicker.bind(this, 'max', {date: this.state.maxDate,maxDate:new Date()})}/>
            </View>
        );
    }
    formateDate (format) {
        var o = {
            "M+" : this.getMonth()+1, //month
            "d+" : this.getDate(), //day
            "h+" : this.getHours(), //hour
            "m+" : this.getMinutes(), //minute
            "s+" : this.getSeconds(), //second
            "q+" : Math.floor((this.getMonth()+3)/3), //quarter
            "S" : this.getMilliseconds() //millisecond
        }
        if(/(y+)/.test(format)) format=format.replace(RegExp.$1,
            (this.getFullYear()+"").substr(4- RegExp.$1.length));
        for(var k in o)if(new RegExp("("+ k +")").test(format))
            format = format.replace(RegExp.$1,
                RegExp.$1.length==1? o[k] :
                    ("00"+ o[k]).substr((""+ o[k]).length));
        return format;
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
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    button: {
        margin: 5,
        backgroundColor: 'white',
        borderBottomWidth: StyleSheet.hairlineWidth,
        padding: 15,
        borderBottomColor: '#cdcdcd'
    }
});

AppRegistry.registerComponent('RNAppDatePickerAndroid', () => RNAppDatePickerAndroid);
```
运行后看到的效果如下图(android 5.0 os):
{% img /images/201607260003.gif %}
