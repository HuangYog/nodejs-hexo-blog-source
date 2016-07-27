---
title: 15.React-Native之PullToRefreshView组件学习
date: 2016-07-21 11:09:09
tags:
- React-Native
- PullToRefreshView
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. PullToRefreshViewAndroid组件 介绍

`PullToRefreshViewAndroid`视图是封装了Android平台的下拉刷新组件(`SwipeRefreshLayout`)，该组件支持设置单一的可以滚动的子视图(例如:ScrollView)。当内部的子视图的垂直方向的偏移量scrollY:0的时候，手指往下拖拽该视图的时候回触发onRefresh事件方法
`注意`该组件风格需要设置成{flex:1}。当我们滚动的子视图为ScrollView或者ListView的时候


### 2.PullToRefreshViewAndroid 属性方法
  1. 继承可以使用View组件的所有Style(具体查看{% link 官方介绍 http://facebook.github.io/react-native/docs/view.html#style%})
  2. `colors` [ColorPropType] 设置下拉刷新加载进度指示器的颜色，可以设置多种颜色(最多设置四种)
  3. `enabled`  boolean  设置是否启动下拉刷新的功能
  4. `progressBackgroundColor`   ColorPropType   设置设置下拉刷新加载进去指示器的背景颜色
  5. `refreshing` boolean   设置当前进去指示器是否在活跃状态，也表明当前是不是在下拉刷新状态
  6. size   RefreshLayoutConsts.SIZE.DEFAULT   下拉刷新指示器的尺寸大小，详细请查看PullToRefreshViewAndroid.SIZE 的{% link 官方介绍  https://github.com/facebook/react-native/blob/master/Libraries/PullToRefresh/PullToRefreshViewAndroid.android.js %}

### 3.PullToRefreshViewAndroid  实例
 1. 类似官方实例,代码如下:
 ```
 'use strict';

 const React = require('react-native');
 const {
   AppRegistry,
   ScrollView,
   StyleSheet,
   PullToRefreshViewAndroid,
   Text,
   View,
 } = React;

 const styles = StyleSheet.create({
   row: {
     borderColor: 'red',
     borderWidth: 2,
     padding: 20,
     backgroundColor: '#3ad734',
     margin: 5,
   },
   text: {
     alignSelf: 'center',
     color: '#fff',

   },
   layout: {
     flex: 1,
   },
   scrollview: {
     flex: 1,
   },
 });
 const Row = React.createClass({
   render: function() {
     return (
         <View style={styles.row}>
           <Text style={styles.text}>
             {this.props.data.text }
           </Text>
         </View>
     );
   },
 });
 const RNAppPullToRefreshView = React.createClass({
   getInitialState() {
     return {
       isRefreshing: false,
       loaded: 0,
       rowData: Array.from(new Array(20)).map(
         (val, i) => ({text: '初始行' + i})
       ),
     };
   },
   render() {
     const rows = this.state.rowData.map((row, ii) => {
       return <Row key={ii} data={row} />;
     });
     return (
       <PullToRefreshViewAndroid
         style={styles.layout}
         refreshing={this.state.isRefreshing}
         onRefresh={this._onRefresh}
         colors={['#ff0000', '#00ff00', '#0000ff','#123456']}
         progressBackgroundColor={'#ffffff'}
         >
         <ScrollView style={styles.scrollview}>
           {rows}
         </ScrollView>
       </PullToRefreshViewAndroid>
     );
   },

   _onRefresh() {
     this.setState({isRefreshing: true});
     setTimeout(() => {
       // 进行准备5项新数据
       const rowData = Array.from(new Array(5))
       .map((val, i) => ({
         text: '下拉刷新行' + (+this.state.loaded + i)
       }))
       .concat(this.state.rowData);

       this.setState({
         loaded: this.state.loaded + 5,
         isRefreshing: false,
         rowData: rowData,
       });
     }, 5000);
   },
 });
 AppRegistry.registerComponent('RNAppPullToRefreshView', () => RNAppPullToRefreshView);
 ```
