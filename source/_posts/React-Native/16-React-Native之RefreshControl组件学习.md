---
title: 16.React-Native之RefreshControl组件学习
date: 2016-07-21 16:09:16
tags:
- React-Native
- RefreshControl
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}
### 1. RefreshControl组件介绍
该组件也可以实现下拉刷新的功能，是用在ScrollView的内部的，为ScrollView添加一个下拉刷新的功能。当ScrollView的垂直方向的偏移量scrollY:0的时候，手指往下拖拽ScrollView就会触发onRefresh事件方法

### 2. RefreshControl属性方法
1. `onRefresh`  function方法 当视图开始刷新的时候调用
2. `refreshing`  bool  决定加载进去指示器是否为活跃状态，也表明当前是否在刷新中
3. `colors` [ColorPropType]   android平台适用  进行设置加载进去指示器的颜色，至少设置一种，最多可以设置4种
4. `enabled`  bool   android平台适用   用来设置下拉刷新功能是否可用
5. `progressBackgroundColor` ColorPropType  设置加载进度指示器的背景颜色
6. `size` RefreshLayoutConsts.SIZE.DEFAULT  android平台适用  加载进度指示器的尺寸大小 ，具体可以查看RefreshControl.SIZE {% link 官方介绍 https://github.com/facebook/react-native/blob/master/Libraries/Components/RefreshControl/RefreshControl.js %}
7. `tintColor` ColorPropType   iOS平台适用  设置加载进度指示器的颜色
8. `title` string iOS平台适用  设置加载进度指示器下面的标题文本信息

### 3. RefreshControl 实例
以下代码在官方实例中进行修改而来，具体代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    ScrollView,
    StyleSheet,
    RefreshControl,
    Text,
    View,
} from 'react-native';

const styles = StyleSheet.create({
    row: {
        borderColor: 'red',
        borderWidth: 5,
        padding: 20,
        backgroundColor: '#3a5795',
        margin: 5
    },
    text: {
        alignSelf: 'center',
        color: '#fff'
    },
    scrollview: {
        flex: 1
    }
});


class Row extends Component{
    _onClick () {
        this.props.onClick(this.props.data);
    }
    render () {
        return (
            <View style={styles.row}>
                <Text style={styles.text}>
                    {this.props.data.text}
                </Text>
            </View>
        );
    }
}

class RNAppRefreshControl extends Component {
    // 构造
    constructor(props) {
        super(props);
        this.state = {
            isRefreshing: false,
            loaded: 0,
            rowData: Array.from(new Array(20)).map(
                (val, i) => ({text: '初始行 ' + i}))
        };
        // 初始状
    }


    render() {
        const rows = this.state.rowData.map((row, ii) => {
            return <Row key={ii} data={row}/>;
        });
        return (
            <ScrollView
                style={styles.scrollview}
                refreshControl={
          <RefreshControl
            refreshing={this.state.isRefreshing}
            onRefresh = {this._onRefresh.bind(this)}
            colors={['#ff0000', '#00ff00', '#0000ff','#3ad564']}
            progressBackgroundColor="#ffffff"
          />
        }>
                {rows}
            </ScrollView>
        );
    }
    _onRefresh() {
        this.setState({isRefreshing: true});
        setTimeout(() => {
            // 准备下拉刷新的5条数据
            const rowData = Array.from(new Array(5))
                .map((val, i) => ({
                    text: '刷新行 ' + (+this.state.loaded + i)
                }))
                .concat(this.state.rowData);

            this.setState({
                loaded: this.state.loaded + 5,
                isRefreshing: false,
                rowData: rowData
            });
        }, 5000);
    }
}
AppRegistry.registerComponent('RNAppRefreshControl', () => RNAppRefreshControl);
```
运行后看到的效果如下:
{% img /images/2016-07-2116-29-22.png %}
