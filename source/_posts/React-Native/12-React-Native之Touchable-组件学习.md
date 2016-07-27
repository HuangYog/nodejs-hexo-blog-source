---
title: 12.React-Native之Touchable组件学习
date: 2016-07-20 09:20:29
tags:
- React-Native
- Touchable*
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. Touchable 系列组件介绍
Touchable 组件包括四种:
  - `TouchableHightlight`
  - `TouchableNativeFeedback`
  - `TouchableOpacity`
  - `TouchableWithoutFeedback`
这四个空间中最后一个(`TouchableWithoutFeedback`)是触摸点击不带反馈效果，另外三个都有反馈效果，可以理解我前三个都是继承自`TouchableWithoutFeedback`扩展而来的。

### 2. `TouchableWithoutFeedback`
#### 1) TouchableWithoutFeedback 介绍
`TouchableWithoutFeedback` 控件除非你是不得不使用，否则请不要使用该控件，任何可以响应事件的控件当触摸或者点击的时候应该会有视觉上面的反应效果(`TouchableWithoutFeedback`组件除外)，这就是一个很大的原因，看起来像Web效果而不是原生的Native效果。
`注意``TouchableWithoutFeedback` 只支持一个子节点，如果需要设置多个子视图组件，那就需要使用`View`节点进行包装。

#### 2) TouchableWithoutFeedback 属性方法
1. accessibilityComponentType View.AccessibilityComponentType 设置可访问的组件类型
2. accessibilityTraits View.AccessibilityTraits,[View.AccessibilityTraits]设置访问特征
3. accessible boolean 设置当前组件是否可以访问
4. delayLongPress number 设置延迟的时间，单位为毫秒， 从onPressIn方法开始，到onLongPress 被调用一段时间
5. delayPressIn number 设置延迟的时间，单位为毫秒，从用户触摸点击开始到onPressIn被调用这一段时间
6. delayPressOut number 设置延迟的时间，单位为毫秒，从用户出门点击时间释放开始到onPressOut 被调用这段时间
7. onLayout function 当组件加装或修改组件的布局发生变化的时候调用，调用传入的参数为{nativeEvent:{layout:{x, y, width, height}}}
8. onLongPress function 方法，当用户长时间按压组件(长按效果)的时候调用该方法
9. onPress function 方法 当用户点击的时候调用(触摸结束)。 但是如果事件被取消了就不会调用。(例如:当前被滑动事件所替代)
10. onPressIn  function  用户开始触摸组件回调方法
11. onPressOut function 用户完成触摸组件之后回调方法
12. pressRetentionOffset {top:number,left:number,bottom:number,right:number}   该设置当视图滚动禁用的情况下，可以定义当手指距离组件的距离。当大于该距离该组件会失去响应。当少于该距离的时候，该组件会重新进行响应

该组件我们一般不会直接进行使用，下面三种Touchable*系列组件对于该组件的属性方法都可以进行使用。具体会具体演示这些属性方法的使用实例

### 3. `TouchableHighlight`(触摸点击高亮效果)

#### 1) TouchableHighlight组件基本介绍
该组件进行封装视图触摸点击的属性。当手指点击按下的时候，该视图的不透明度会进行降低同时会看到相应的颜色(视图变暗或者变亮)。如果我们去查看该组件的源代码会发现，该底层实现是添加了一个新的视图。如果如果我们没有正确的使用，就可能不会出现正确的效果。例如:我们没有给该组件视图设置backgroudnColor的颜色值。
`注意`TouchableHighlight只支持一个子节点，如果你需要设置多个子视图组件，那么就需要使用View节点进行包装

#### 2) TouchableHighlight属性方法
1. TouchableWithoutFeedback的 属性，这边TouchableHighlight组件全部可以进行使用
2. activeOpacity  number 该用来设置视图在进行触摸的时候，要要显示的不透明度(通常在0-1之间)
3. onHideUnderlay  function  方法 当底层被隐藏的时候调用
4. onShowUnderlay  function 方法 当底层显示的时候调用
5. style  可以设置控件的风格演示，该风格演示可以参考View组件的style
6. underlayColor  当触摸或者点击控件的时候显示出的颜色

#### 3) TouchableHighlight 使用实例
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableHighlight
} from 'react-native';
class RNAppTouchable extends Component {
    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            touchCount: 0
        };
      }
    render() {
        return (
            <View style={styles.container}>
                <Text>
                    TouchableHighlight 实例 一
                </Text>
                <TouchableHighlight
                    underlayColor='rgba(106,168,79, 0.1)'
                    activeOpacity={0.5}
                    style={{borderRadius: 8, padding: 6, marginTop: 5}}
                    onPress={()=>{
                       this.setState({touchCount: this.state.touchCount+1});
                    }}
                >
                    <Text style={{fontSize: 16}}> Touch Me !!!</Text>
                </TouchableHighlight>
                <View style={{backgroundColor: '#ff00ff',marginTop:30}}>
                    <Text style={{fontSize: 20}}> 我被点击了 {this.state.touchCount} 次 !!!</Text>
                </View>
            </View>
        );
    }
}
const styles = StyleSheet.create({
    container: {
        flex: 1,
        // justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF'
    }
});
AppRegistry.registerComponent('RNAppTouchable', () => RNAppTouchable);
```
运行后效果如下图,可以点击《Touch Me》查看效果:
{% img /images/2016-07-2010-44-49.png %}

### 4. `TouchableOpacity`(透明度变化)

#### 1) TouchableOpacity组件介绍
该组件封装了响应触摸事件。当点击按下的时候，该组件的透明度会降低。该组件使用过程中并不会改变视图的层级关系，而且我们可以非常容易的添加到应用并且不会产生额外的异常错误

#### 2) TouchableOpacity 属性方法
1. TouchableWithoutFeedback的 属性，这边TouchableOpacity组件全部可以进行使用
2.activeOpacity  number  设置当用户触摸的时候，组件的透明度

#### 3) TouchableOpacity 使用实例
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableOpacity
} from 'react-native';
class RNAppTouchable extends Component {
    render() {
        return (
            <View>
                <View style={{justifyContent: 'center', alignItems: 'center',backgroundColor: '#F5FCFF', marginTop: 30,height: 100}}>
                    <Text> TouchableOpacity 使用实例</Text>
                    <TouchableOpacity
                        style={{marginTop: 20,backgroundColor: '#e69138'}}
                        activeOpacity = {0.1}>
                        <Text style={{fontSize: 16}}>点击我改变透明度</Text>
                    </TouchableOpacity>
                </View>
            </View>
        );
    }
}
AppRegistry.registerComponent('RNAppTouchable', () => RNAppTouchable);
```
运行后看到的效果，点击前左图，点击时右图:
{%img /images/2016-07-2010-59-44.png %}{%img /images/2016-07-2010-59-58.png %}

#### 4) TouchableOpacity 使用实例(onPress,onPressIn,onPressOut,onLongPress方法代码)
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableHighlight,
    TouchableOpacity
} from 'react-native';
class RNAppTouchable extends Component {
    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            eventLog: []
        };
      }
    _appendEvent(eventName){
        var limit = 6;
        var eventLog = this.state.eventLog.slice(0, limit - 1);
        eventLog.unshift(eventName);
        this.setState({eventLog});
    }
    render() {
        return (
            <View>
                <View style = {[styles.row,{justifyContent: 'center'}]}>
                    <TouchableOpacity
                        style={styles.wrapper}
                        onPress={() => this._appendEvent('press')}
                        onPressIn={() => this._appendEvent('pressIn')}
                        onpressOut={() => this._appendEvent('pressOut')}
                        onLongPress={() => this._appendEvent('LongPress')}>
                        <Text style={styles.button}>
                            Press Me
                        </Text>
                    </TouchableOpacity>
                </View>
                <View style={styles.eventLogBox}>
                    {this.state.eventLog.map((e, ii) => <Text key={ii}>{e}</Text>)}
                </View>
            </View>
        );
    }
}
const styles = StyleSheet.create({
    button: {
    color: "#007AFF"},
    wrapper: {
        borderRadius: 8
    },
    eventLogBox: {
        padding: 10,
        margin: 10,
        height: 120,
        borderWidth: StyleSheet.hairlineWidth,
        borderColor: '#f0f0f0',
        backgroundColor: '#f9f9f9'
    }
});
AppRegistry.registerComponent('RNAppTouchable', () => RNAppTouchable);
```
运行后出现的效果如图,长按和短按:
{%img /images/2016-07-2012-36-59.png%}

### 5. `TouchableNativeFeedback`
#### 1) TouchableNativeFeedback组件介绍
该组件封装了可以进行响应触摸事件的组件(仅限Android平台)。在Android平台上面该该组件可以使用原生平台的状态资源来显示触摸状态变化。【特别注意】现如今该组件只是支持仅有一个View的子视图实例(作为子节点)。在底层实现上面实际上面是创建一个新的RCTView的节点来进行替换当前的View节点视图，并且可以携带一些附加的属性。还有就是 该组件触摸反馈的背景图资源可以通过background属性进行自定义设置。
下面是简单的例子:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableNativeFeedback
} from 'react-native';
class RNAppTouchable extends Component {
    render() {
        return (
          <View>
              <Text>TouchableNativeFeedback实例 一</Text>
              <TouchableNativeFeedback
                  style={{marginTop:20}}>
                  <View style={{width: 150, height: 100, backgroundColor: 'red'}}>
                      <Text style={{margin: 30}}>Button</Text>
                  </View>

              </TouchableNativeFeedback>
          </View>
        );
    }
}

AppRegistry.registerComponent('RNAppTouchable', () => RNAppTouchable);
```
运行后的效果如下图:
{% img /images/2016-07-2014-45-01.png %}

#### 1) TouchableNativeFeedback 属性方法
  1. TouchableWithoutFeedback的 属性，这边TouchableNativeFeedback组件全部可以进行使用
  2. background  backgroundPropType  该用来设置背景资源的类型，该属性会传入一个type属性和依赖的额外资源的data的对象。官方推荐采用如下的静态方法来进行生成该dictionary对象：
    - TouchableNativeFeedback.SelectableBackground()   该会创建使用android默认主题背景(?android:attr/selectableItemBackground)
    - TouchableNativeFeedback.SelectableBackgroundBorderless()  该会创建使用android默认的无框的主题背景(?android:attr/selectableItemBackgroundBorderless)。不过该参数需要Android API 21+才可以使用
    - TouchableNativeFeedback.Ripple(color,borderless)  该会创建当组件被按下的时候有一个水滴的效果.通过color参数指定颜色，如果borderless为true的时候，那么该水滴效果会渲染到该组件视图的外边。同样该背景类型参数也需要Android API 21+才可以使用

#### 2) TouchableNativeFeedback 使用实例
实现`当组件被按下的时候有一个水滴的效果`，代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableNativeFeedback
} from 'react-native';
class RNAppTouchable extends Component {
    render() {
        return (
            <View style={{justifyContent: 'center',alignItems: 'center'}}>
              <Text>TouchableNativeFeedback实例 二</Text>
              <TouchableNativeFeedback
                  background = {TouchableNativeFeedback.Ripple('#CD1076',false)}>
                  <View style={{width: 150, height: 100,backgroundColor: "#00ffcc"}}>
                      <Text style={{margin: 30}}>Button</Text>
                  </View>
              </TouchableNativeFeedback>
            </View>
        );
    }
}
AppRegistry.registerComponent('RNAppTouchable', () => RNAppTouchable);
```
实现的效果如下，用手指按下产生的效果图:
{% img /images/2016-07-2015-03-05.png %}
