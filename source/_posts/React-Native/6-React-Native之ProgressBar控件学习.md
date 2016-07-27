---
title: 6.React-Native之ProgressBar控件学习
date: 2016-07-18 09:25:03
tags:
- React-Native
- ProgressBarAndroid
- React
categories:
- 日志
- 笔记
---
本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1.`ProgressBarAndroid`是一个Android 的进度信息控件。
`ProgressBarAndroid` 的React组件封装了Android平台的Progress控件，下采用ProgressBarAndroid来实现一个最简单的例子，
```
<View >
    <Text>
        ProgressBarAndroid控件实例
    </Text>
    <ProgressBarAndroid styleAttr='Inverse'/>
</View>  
);
}
```
在运运行前还需要先‘引进`ProgressBarAndroid`组件，运行后实现的效果如下图：
{% img /images/2016-07-1810-09-35.png %}

### 2.属性方法
1.支持View组件的属性方法(继承自View组件)，例如：大小，布局，边距
2.color设置进度的颜色属性值
3.indeterminate 设置是否要显示一个默认的进度信息，``注意``如果styleAttr的风格设置成Horizontal(水平)的时候该值必须设置成`false`。
4.progress number设置当前的加载进度值(0-1之间)
5.styleAttr 进度条框的风格，可以取的值如下：
  - `Horizontal` (水平)
  - `Small`
  - `Large`
  - `Inverse` (逆)
  - `SmallInverse`
  - `LargeInverse`

### ProgressBarAndroid使用实例
使用上面的各种风格来实现效果，具体如下:
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
  ProgressBarAndroid,
  View
} from 'react-native';

class RNAppProgressBar extends Component {
  render() {
    return (
        <View>
            <Text>
                ProgressBarAndroid 控件的各种实例
            </Text>
            <ProgressBarAndroid color='red' styleAttr = 'Inverse'/>
            <ProgressBarAndroid color='green' styleAttr = 'Horizontal' progress = {0.7}
            indeterminate = {false} style = {{marginTop: 10}}
            />
            <ProgressBarAndroid color = 'blue' styleAttr = 'Horizontal' indeterminate = {true}
                                style={{marginTop:10}}/>
            <ProgressBarAndroid color = 'black' styleAttr='SmallInverse'
                                style={{marginTop:10}} />
            <ProgressBarAndroid styleAttr = "LargeInverse" style={{marginTop: 10}} />

        </View>
    );
  }
}
AppRegistry.registerComponent('RNAppProgressBar', () => RNAppProgressBar);
```
最后运行实现的效果如下:
{% img /images/2016-07-1810-39-44.png %}
