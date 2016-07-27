---
title: 17.React-Native之Navigator组件学习
date: 2016-07-26 15:02:21
tags:
- React-Native
- Navigator
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}


### 1. Navigator组件介绍
`Navigator`组件(导航栏)可以让我们客户端在不同的页面见进行切换，`Navigator`提供了路由对象功能进行区分识别每一个页面。同时我们可以通过`renderScene`方法，`Navaigator`根据指定的路由进行渲染指定的界面.
除了以上功能之外，为了改变页面切换的动画或者页面的手势，该组件还提供`configureScene`属性来进行获取指定路由页面的配置对象信息({% link 官方介绍 https://github.com/facebook/react-native/blob/master/Libraries/CustomComponents/Navigator/NavigatorSceneConfigs.js %})
关于动画手势还有一些属性:
  1. PushFromRight
  2. FloatFromRight
  3. FloatFromLeft
  4. FloatFromBottom
  5. FloatFromBottomAndroid
  6. FadeAndroid
  7. HorizontalSwipeJump
  8. HorizontalSwipeJumpRight
  9. VerticalUpSwipeJump
  10. VerticalDownSwipeJump

### 2. Navigator组件方法和属性
在使用导航器的时候如果已经获取了导航器对象的引用，我们可以进行调用以下一些方法来实现页面导航功能:
 方法:
  1. `getCurrentRoutes()`    该进行返回存在的路由列表信息
  2. `jumpBack()`    该方法进行回退操作  但是不会卸载(删除)当前的页面
  3. `jumpForward()`    进行跳转到相当于当前页面的下一个页面
  4. `jumpTo(route)`    根据传入的一个路由信息，跳转到一个指定的页面(该页面不会卸载删除)
  5. `push(route) `    导航切换到一个新的页面中，新的页面进行压入栈。通过jumpForward()方法可以回退过去
  6. `pop()`   当前页面弹出来，跳转到栈中下一个页面，并且卸载删除掉当前的页面
  7. `replace(route)`   只用传入的路由的指定页面进行替换掉当前的页面
  8. `replaceAtIndex(route,index)`     传入路由以及位置索引，使用该路由指定的页面跳转到指定位置的页面
  9. `replacePrevious(route)`    传入路由，通过指定路由的页面替换掉前一个页面
  10. `resetTo(route)`  进行导航到新的界面，并且重置整个路由栈
  11. `immediatelyResetRouteStack(routeStack)`  该通过一个路由页面数组来进行重置路由栈
  12. `popToRoute(route)`   进行弹出相关页面，跳转到指定路由的页面，弹出来的页面会被卸载删除
  13. `popToTop() ` 进行弹出页面，导航到栈中的第一个页面，弹出来的所有页面会被卸载删除
 属性:
  1. `configureScene`  function 方法，为可选的方法进行配置页面切换动画和手势,会通过路由和路由栈两个参数调用，进行返回一个页面参数配置对象：(route, routeStack) => Navigator.SceneConfigs.FloatFromRight
  2. `initialRoute`  object  参数对象  进行设置导航初始化的路由页面。路由是标识导航器渲染标识每一个页面的对象。initialRoute必须为initialRouteStack中的路由。同时initialRoute默认为initialRouteStack中路由栈的最后一项
  3. `initialRouteStack`  [object] 参数对象数组   该是一个初始化的路由数组进行初始化。如果initalRoute属性没有设置的话，那么就必须设置initialRouteStack属性，使用该最后一项作为初始路由。 如果initalRouteStack属性没有设置的话，该会生成只包含initalRoute值的数组
  4. `navigationBar`  node  为可选的参数，在页面切换中用来提供一个导航栏
  5. `navigator` object   为可选参数，可以从父类导航器中获取导航器对象
  6. `onDidFoucs`  function  该方法已经废弃，我们可以使用
  7. `navigationContext.addListener('didfocus',callback)`方法进行替代。该会在每次页面切换完成或者初始化之后进行调用该方法。该参数为新页面的路由
  8. `onWillFocus` function  该方法已经废弃，我们可以使用`navigationContext.addListener('willfocus',callback)`方法进行替代。该会页面每次进行切换之前调用
  9. `renderScene`  function  该为必须调用的方法，该用来渲染每一个路由指定的页面。参数为路由以及导航器对象两个参数，具体是方法如下:(route, navigator) =><MySceneComponent title={route.title} navigator={navigator} />
  10. `sceneStyle` 样式风格，该继承了View视图的所有样式风格。可以参照:{% link 官方样式 http://facebook.github.io/react-native/docs/view.html#content %} ，设置用于每个页面容器的风格

### 4. Navigator组件实例
 实例具体代码如下:
 ```
 'use strict';
 import React, {Component} from 'react';
 import {
     AppRegistry,
     StyleSheet,
     Text,
     View,
     TouchableHighlight,
     Navigator
 } from 'react-native';

 class NavButton extends Component {
     render() {
         return (
             <TouchableHighlight
                 style={styles.button}
                 underlayColor="#b5b5b5"
                 onPress={this.props.onPress}>
                 <Text style={styles.buttonText}>{this.props.text}</Text>
             </TouchableHighlight>
         );
     }
 }

 class NavMenu extends Component{
     render() {
         return(
             <View style={styles.scene}>
                 <Text style={styles.messageText}>{this.props.message}</Text>
                 <NavButton
                     onPress={() => {
                         this.props.navigator.push({
                             message: '向右拖拽关闭页面',
                             sceneConfig: Navigator.SceneConfigs.FloatFromRight
                         });
                     }}
                     text = '从右边向左切入页面(带有透明度变化)'
                 />
                 <NavButton
                     onPress={() => {
                         this.props.navigator.push({
                             message: '向下拖拽关闭页面',
                             sceneConfig: Navigator.SceneConfigs.FloatFromBottom,
                         });
                     }}
                     text="从下往上切入页面(带有透明度变化)"
                 />
                 <NavButton
                     onPress={() => {
                         this.props.navigator.popToTop();
                     }}
                     text = '页面弹出(回退到最后一页)'
                 />
                 <NavButton
                     onPress={() => {
                         this.props.navigator.pop();
                     }}
                     text = '页面弹出(回退一页)'
                 />
             </View>
         );
     }
 }

 class RNAppNavigator extends Component{
     render() {
         return (
             <Navigator
                 style={styles.container}
                 initialRoute= {{message: '页面初始化'}}
                 renderScene= {(route, navigator) => <NavMenu
                     message = {route.message} navigator = {navigator}/>}
                 configureScene={(route) => {
                     if (route.sceneConfig) {
                         return route.sceneConfig;
                     }
                     return Navigator.SceneConfigs.FloatFromBottom;
                 }}
             />
         );
     }
 }

 const styles = StyleSheet.create({
     container: {
         flex: 1
     },
     messageText: {
         fontSize: 17,
         fontWeight: '500',
         padding: 15,
         marginTop: 50,
         marginLeft: 15
     },
     button: {
         backgroundColor: 'white',
         padding: 15,
         borderBottomWidth: StyleSheet.hairlineWidth,
         borderBottomColor: '#cdcdcd'
     }
 });

 AppRegistry.registerComponent('RNAppNavigator', () => RNAppNavigator);
 ```
 最后运行出来的效果如下:
 {% img /images/1260726001.gif %}
