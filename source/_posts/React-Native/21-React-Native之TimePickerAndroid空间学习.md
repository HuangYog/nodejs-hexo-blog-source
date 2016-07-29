---
title: 21.React-Native之TimePickerAndroid空间学习
date: 2016-07-29 10:15:52
tags:
- React-Native
- TimePickerAndroid
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}


### 1. TimePickerAndroid组件介绍
`TimePickerAndroid`进行渲染打开一个原生的Android时间选择弹框，首先看一下官方给的一个示例代码:
```
try {
  const {action, hour, minute} = await TimePickerAndroid.open({
    hour: 14,
    minute: 0,
    is24Hour: false, // Will display '2 PM'
  });
  if (action !== DatePickerAndroid.dismissedAction) {
    // Selected hour (0-23), minute (0-59)
  }
} catch ({code, message}) {
  console.warn('Cannot open time picker', message);
}
```
### 2. TimePickerAndroid组件属性方法
  1. `open(opstions:Object)`  static静态方法 ,用来进行弹出一个Android标准的时间选择器的弹框。传入参数options对象的key-value值如下:
    - `hour` :需要进行显示的小时，范围在0-23之间，默认显示为当前时间
    - `minute` :需要进行显示的分钟，范围在0-59之间，默认显示为当前时间
    - `is24Hour` : boolean类型 ,如果该值为true,选择器使用24小时格式制,如果设置成false，那么选择器会显示AM/PM格式。如果没有定义，那么该值会默认显示当地设置
  当用户进行使用选择器选择了一个时间之后，那么该静态方法会返回一个Promise对象,该对象会携带action,hour(0-23),minute(0-59)三个参数。如果用户取消了弹框，那么该Promise仍然会进行返回。该携带的action为TimePickerAndroid.dismissedAction，但是其他的keys为未定义。正因为如此我们在进行读取Promise中的值的时候就一定需要检查action
  2. `timeSetAction()`   static静态方法,选择一个时间
  3. `dismissedAction()`   static静态方法,选择器弹框被关闭

### 3. TimePickerAndroid组件实例
实例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableHighlight,
    TimePickerAndroid,
    ToastAndroid
} from 'react-native';

class CustomButton extends Component{
    render(){
        return(
            <TouchableHighlight
                style={styles.button}
                underlayColor="#a5a5a5"
                onPress={this.props.onPress}>
                <Text style={styles.buttonText}>{this.props.text}</Text>
            </TouchableHighlight>
        );
    }
}

class RNAppTimePickerAndroid extends Component {

    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            isoFormatText: 'pick a time (24-hour format)',
            presetHour: 4,
            presetMinute: 4,
            presetText: 'pick a time, default: 4:04AM',
            simpleText: 'pick a time'
        };
      }

      async showPicker(options){
          try {
              const {action, minute, hour} = await TimePickerAndroid.open(options);
              if (action === TimePickerAndroid.timeSetAction){
                  ToastAndroid.show('选择时间为:' + this._formatTime(hour, minute), ToastAndroid.SHORT);
              } else if (action === TimePickerAndroid.dismissedAction) {
                  ToastAndroid.show('选择器关闭取消', ToastAndroid.SHORT);
              }
          } catch ({code, message}){
           ToastAndroid.show('错误信息:' + message, ToastAndroid.SHORT);
          }
      }

      _formatTime(hour, minute){
          return hour + ":" + (minute < 10 ? '0' + minute : minute);
      }


    render() {
        return (
            <View>
                <Text style={{margin: 10}}>
                    Welcome to React Native! TimePickerAndroid 实例
                </Text>
                <CustomButton
                    text = '时间选择器默认当前时间'
                    onPress={this.showPicker.bind(this,{})}/>
                <CustomButton
                    text = '时间选择器-指定时间:20:34'
                    onPress={this.showPicker.bind(this,
                        {hour: 4,minute: 34}
                    )}/>
                <CustomButton
                    text = '时间选择器-指定时间以及时间制格式'
                    onPress={this.showPicker.bind(this,
                        {hour: 20, minute: 34, is24Hour: true})}
                />
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

AppRegistry.registerComponent('RNAppTimePickerAndroid', () => RNAppTimePickerAndroid);
```
运行后看到是效果如下,左图为默认当前时间,右图是给定显示某个时间:
{% img /images/2016-07-29-11-15-11.png 300 500 %}  {% img /images/016-07-29-11-13-39.png 300 500 %}
