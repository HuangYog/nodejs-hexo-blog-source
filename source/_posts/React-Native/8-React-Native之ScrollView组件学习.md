---
title: 8.React-Native之ScrollView组件学习
date: 2016-07-18 15:14:49
tags:
- React-Native
- ScrollView
- React
categories:
- 日志
- 笔记
---
本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1.ScrollView组件的介绍
`ScrollView`组件封装了Android平台的ScrollView组件(滚动视图)，并且提供触摸事件`responder`系统功能。在使用 `ScrollView`的时候我们必须要确保该有一个固定的高度，因为该组件包含很多不固定高度的字符控件装入到固定的容器中(通过滑动交互),如果给`ScrollView`设置高度，要么我们直接给该`ScrollView`进行设置高度(不建议使用该方法)，要么就是该`ScrollView`的父控件设置相关高度，使用第二种方法视图任意位置中是不能忘记`{flex：1}`，不然不会有效果的。
下面我们来看看官方实例:
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
    ScrollView,
  StyleSheet,
  Text,
  TouchableOpacity,
  View
} from 'react-native';
var NUM_ITEMS = 20;
class RNAppScrollView extends Component {

    makeItems(nItems: number, styles){
        var items = [];
        for (var i = 0; i < nItems; i++) {
            items[i] = (
                <TouchableOpacity key={i} style={styles}>
                    <Text>{'Item ' + i}</Text>
                </TouchableOpacity>
            );
        }
        return items;
    }

  render() {
      // One of the items is a horizontal scroll view
      var items = this.makeItems(NUM_ITEMS, styles.itemWrapper);
      items[4] = (
          <ScrollView key={'scrollView'} horizontal={true}>
              {this.makeItems(NUM_ITEMS, [styles.itemWrapper, styles.horizontalItemWrapper])}
          </ScrollView>
      );

      var verticalScrollView = (
          <ScrollView style={styles.verticalScrollView}>
              {items}
          </ScrollView>
      );

      return verticalScrollView;
  }
}

const styles = StyleSheet.create({
  verticalScrollView:{
      margin:10
  },
    itemWrapper:{
        backgroundColor:'#dddddd',
        alignItems:'center',
        borderRadius:5,
        borderWidth: 5,
        borderColor: '#a52a2a',
        padding:30,
        margin:5
    },
    horizontalItemWrapper:{
        padding:50
    }
});

AppRegistry.registerComponent('RNAppScrollView', () => RNAppScrollView);
```
运行后看到的效果如下图:
{% img /images/2016-07-1815-56-34.png %}
注意查看右侧的阴影滚动条和水平方向，垂直方向的滑动

### 2.基本的额属性方法
1.继承自View相关的属性样式(宽和高,背景颜色,边距等相关属性样式)
2.contentContainerStyle 样式风格属性(传入StyleSheet创建Style文件)该样式会作用于被ScrollView包裹的所有的子视图如下:
```
return(
  <ScrollView contentContainerStyle = {styles.contentContainer}></ScrollView>
  );
  var styles = StyleSheet.create({
    contentContainer:{
      paddingVertical:20
    }
    });
```
3.horizontal 表示ScrollView是横向滑动还是纵向滑动，该值默认为false 表示纵向滑动
4.keyBoardDismissMode 枚举类型表示键盘隐藏类型，可选('none','on-drag'，'interactive')三个值的意思分别如下:
  - `none` 默认值，表示在进行拖拽滑动的时候步隐藏键盘
  - `interactive` 表示在拖拽触摸移动的同时隐藏键盘，向上拖拽的时候取消隐藏，(在android平台上面部支持该选项，所以会和none的效果一样)
  - `on-drag` 表示在进行拖拽滑动开始的时候隐藏键盘
5.keyboardshouldPersistTaps 该属性默认为`false`，表示如果当前是textinput控件并且键盘是弹出状态的话，点击textinput之外的其他地方键盘就会隐藏，反之不会有效果，键盘还是打开状态。
6.onContentSizeChange function 该滚动视图的内容尺寸大小发生变化的时候进行调用
7.onScroll function 该方法在滚动的时候每frame(帧)调用一次，该方法事件的调用频率可以使用scrollEventThrottle 属性进行设置
8.refreshControl element 设置元素控件，可以进行指定RefreshControl 组件，这样可以为ScrollView 添加`下拉刷新的功能`。
9.removeClippedSubviews 测试属性，当该值为 true的时候在ScrollView视图之外的视图(该视图的overflow属性值必须要为`hidden`)会从被暂停是移除，该设置可以提高滚动的性能
10.showsHorizontalScrollIndicator 该值设置是否需要显示横向滚动指示条
11.showsVerticalScrollIndicator 该值设置是否需要显示纵向滚动指示条
12.sendMomentumEvents 当ScrollView有onMonentumScrollBegin或是onMomentumScrollEnd方法设置，该sendMomentumEvents 值设置为true 的时候，变化的事件信息会通过该android框架自动发送出来，然后被之前设置的方法进行捕捉。
还有很多属性是基于iOS的，这里需要自己去查看{% link 官方文档 http://facebook.github.io/react-native/docs/scrollview.html#content %}

### 3.使用实例
```
import React, { Component } from 'react';
import {
    AppRegistry,
    ScrollView,
    StyleSheet,
    Text,
    TouchableOpacity,
    View
} from 'react-native';

class RNAppScrollView extends Component {
    render(){
       return (
           <View style = {styles.container}>
               <Text style={styles.welcome}> 进行测试ScrollView控件</Text>
               <ScrollView showsVerticalScrollIndicator={true} contentContainerStyle={styles.contentContainer}>
                   <Text style={{color: '#fff', margin:5, fontSize:16, backgroundColor:'#43CD80'}}>
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       React-Native之DrawerLayoutAndroid控件学习
                       ...
                   </Text>
               </ScrollView>
           </View>
       );
    }
}
const styles = StyleSheet.create({
    container:{
        height: 300,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#f5fcff'
    },
    contentContainer:{
        margin:10,
        color: 'yellow',
        backgroundColor: '#ff0000'
    }
});

AppRegistry.registerComponent('RNAppScrollView', () => RNAppScrollView);
```
运行后的效果如下图：
{% img /images/2016-07-1816-57-39.png %}
