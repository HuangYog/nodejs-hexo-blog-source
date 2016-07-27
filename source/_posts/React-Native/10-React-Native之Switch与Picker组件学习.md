---
title: 10.React-Native之Switch与Picker组件学习
date: 2016-07-19 14:28:51
tags:
- React-Native
- Switch与Picker
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

## 一. Switch选择开关控件 介绍
### 1. Switch控件介绍
 Switch控件 为 Android/IOS 两个平台通用的两种状态选择开关组件

### 2. switch属性方法介绍(包括通用的和android的)
  - `View` 相关属性样式全部继承(宽和高,背景颜色,边距等相关属性样式)
  - `disabled` boolean 如果该值为true ，用户就无法点击switch开关，默认为false
  - `onValueChange` function 方法，当该组件的状态值发生变化的时候回调该方法
  - `value` boolean 该开关的值，如果为true的时候开关呈打开状态，默认为false

### 3. Switch 使用实例
  1.基础Switch 开关控件实例演示，添加点切换开关状态，代码如下:
    ```
    'use strict';
    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
      Switch
    } from 'react-native';

    class RNAppSwitchAndPicker extends Component {
        constructor(props) {
            super(props);
            this.state = {
                trueSwitchIsOn: true,
                falseSwitchIsOn: false
            };

        }
      render() {
        return (
          <View style={styles.container}>
            <Text style={styles.welcome}>
              Switch 实例 一
            </Text>
            <Switch
                onValueChange = {value => this.setState({falseSwitchIsOn: value})}
                style={{marginBottom: 10, marginTop:10}}
                value={this.state.falseSwitchIsOn}
            ></Switch>
              <Switch
                  onValueChange = {value => this.setState({trueSwitchIsOn: value})}
                  value={this.state.trueSwitchIsOn}
              ></Switch>
          </View>
        );
      }
    }
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        // justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      }
    });

    AppRegistry.registerComponent('RNAppSwitchAndPicker', () => RNAppSwitchAndPicker);
    ```
    运行后得到的效果如下:
{% img /images/2016-07-1915-29-33.png %}

    2. Switch 第一个开关控件设置无法点击状态，代码如下
    ```
    'use strict';
    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
      Switch
    } from 'react-native';

    class RNAppSwitchAndPicker extends Component {
        constructor(props) {
            super(props);
            this.state = {
                trueSwitchIsOn: true,
                falseSwitchIsOn: false
            };

        }
      render() {
        return (
          <View style={styles.container}>
              <Text style={styles.welcome}>
                  Switch 实例 二
              </Text>
              <Switch
                  disabled={true}
                  onValueChange = {value => this.setState({falseSwitchIsOn: value})}
                  style={{marginBottom: 10, marginTop:10}}
                  value={this.state.falseSwitchIsOn}
              ></Switch>
              <Switch
                  onValueChange = {value => this.setState({trueSwitchIsOn: value})}
                  value={this.state.trueSwitchIsOn}
              ></Switch>
          </View>
        );
      }
    }
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        // justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      }
    });

    AppRegistry.registerComponent('RNAppSwitchAndPicker', () => RNAppSwitchAndPicker);
    ```
    运行后看到的效果如下：
{% img /images/2016-07-1915-34-25.png %}

## 二. Picker选择器控件
### 1.Picker选择器介绍
Picker能渲染iOS和Android平台上面的原生选择器，官方实例如下:
```
<Picker
  selectedValue = {this.state.language}
  onValueChange = {(lang) => this.setState({language: lang})}
>
  <Picker.Item label = 'Jave' value = 'java' />
  <Picker.Item label = 'JavaScript' value = 'js' />
</Picker>
```

### 2.Picker 属性相关方法
 1. View相关属性样式全部继承
 2. onValueChange function 方法，当选择器item 被选择的时候进行调用，该方法调用的时候会传如两个参数
  - `itemValue`  表示该属性值为被选中的item的属性值
  - `itemPosition` 表示该选择器被选中的item的索引position
 3. selectedValue any 任何参数值，选择器选中的item所对应的值，值可以是一个字符串，一个数字
 4. enabled boolean 如果该值为false， picker 就无法被点击选中
 5. mode enum ('dialog','dropdown')， 选择器模式 在android平台上面设置mode可以控制用户点击picker弹出的风格：
  - `dialog` 该值为默认值，弹出一个dialog弹出框
  - `dropdown` 以picker 视图为基础，在该视图下面弹出下拉框
 6. prompt string 设置picker 的提示语(标语) 在android 平台上面模式设置成 `dialog`显示弹出框的标题10-React-Native之Switch与Picker组件学习.md

### 3.Picker使用实例
 1. 基础选择控件实例`dialog`弹出框，代码如下：
```
  import React, { Component } from 'react';
  import {
   AppRegistry,
   StyleSheet,
   Text,
   View,
   Picker
  } from 'react-native';

  class RNAppSwitchAndPicker extends Component {
     constructor(props) {
         super(props);
         this.state = {
             language: ''
         };

     }
   render() {
     return (
       <View style={styles.container}>
         <Text style={styles.welcome}>
           Picker 选择器 实例 一
         </Text>
         <Picker
             style={{width: 200}}
             selectedValue = {this.state.language}
             onValueChange = {(value) => this.setState({language: value})}>
             <Picker.Item label = 'Java' value = 'java'/>
             <Picker.Item label = 'JavaScript' value = 'js'/>
         </Picker>
           <Text>
               当前选择的是：{this.state.language}
           </Text>
       </View>
     );
   }
  }
  const styles = StyleSheet.create({
   container: {
     flex: 1,
     // justifyContent: 'center',
     alignItems: 'center',
     backgroundColor: '#F5FCFF',
   }
  });

  AppRegistry.registerComponent('RNAppSwitchAndPicker', () => RNAppSwitchAndPicker);
```
运行后看到的效果如下:
{% img /images/2016-07-1916-07-26.png %}

2.基础选择器控件:设置弹出框标题，代码如下：
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  Picker
} from 'react-native';

class RNAppSwitchAndPicker extends Component {
    constructor(props) {
        super(props);
        this.state = {
            language: ''
        };

    }

  render() {
      var promptStr = "请选择您喜欢的编程语言";
    return (
      <View style={styles.container}>
          <Text style={styles.welcome}>
              Picker 选择器 实例 二
          </Text>
          <Picker
              prompt = {promptStr}
              style={{width: 200}}
              selectedValue = {this.state.language}
              onValueChange = {(value) => this.setState({language: value})}>
              <Picker.Item label = 'Java' value = 'java'/>
              <Picker.Item label = 'JavaScript' value = 'js'/>
          </Picker>
          <Text>
              当前选择的是：{this.state.language}
          </Text>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    // justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  }
});

AppRegistry.registerComponent('RNAppSwitchAndPicker', () => RNAppSwitchAndPicker);
```
  运行后看到的效果如下:
 {% img /images/2016-07-1916-12-01.png %}


 3. 基础选择器实例: 设置下拉选择框，代码如下:
 ```
 'use strict';
 import React, { Component } from 'react';
 import {
   AppRegistry,
   StyleSheet,
   Text,
   View,
   Picker
 } from 'react-native';

 class RNAppSwitchAndPicker extends Component {
     constructor(props) {
         super(props);
         this.state = {
             language: ''
         };

     }

   render() {
       var promptStr = "请选择您喜欢的编程语言";
     return (
       <View style={styles.container}>
           <Text style={styles.welcome}>
               Picker 选择器 实例 三
           </Text>
           <Picker
               mode = {'dropdown'}
               style={{width: 200}}
               selectedValue = {this.state.language}
               onValueChange = {(value) => this.setState({language: value})}>
               <Picker.Item label = 'Java' value = 'java'/>
               <Picker.Item label = 'JavaScript' value = 'js'/>
           </Picker>
           <Text>
               当前选择的是：{this.state.language}
           </Text>

       </View>
     );
   }
 }
 const styles = StyleSheet.create({
   container: {
     flex: 1,
     // justifyContent: 'center',
     alignItems: 'center',
     backgroundColor: '#F5FCFF',
   }
 });

 AppRegistry.registerComponent('RNAppSwitchAndPicker', () => RNAppSwitchAndPicker);
 ```
 运行后出现的效果如下:
 {% img /images/2016-07-1916-46-32.png %}
