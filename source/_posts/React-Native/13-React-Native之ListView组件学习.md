---
title: 13.React-Native之ListView组件学习
date: 2016-07-20 15:09:17
tags:
- React-Native
- ListView
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. ListView组件介绍
`ListView`组件是`React Native`中一个比较核心的组件，用途非常的广。该组件设计用来高效的展示垂直滚动的数据列表。最简单的API就是创建一个ListView.DataSource对象，同时给该对象传入一个简单的数据集合。并且使用数据源(data source)实例化一个ListView组件,定义一个renderRow回调方法(该方法的参数是一个数组)，该renderRow方法会返回一个可渲染的组件(该就是列表的每一行的item)
下面是一个官方例子：
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    ListView
} from 'react-native';

class RNAppListView extends Component {
    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
        this.state = {dataSource: ds.cloneWithRows(['row 1', 'row 2','row 3','row 4','row 5','row 6','row 7','row 8'])};
      }
    render() {
        return (
            <ListView
                dataSource={this.state.dataSource}
                renderRow={(rowData) => <Text>{rowData}</Text>}
            />
        );
    }
}
AppRegistry.registerComponent('RNAppListView', () => RNAppListView);
```
运行后的效果如下:
{% img /images/2016-07-2015-57-46.png %}

### 2. ListView属性方法
  1. `ScrollView`相关属性样式全部继承
  2. `dataSource`   ListViewDataSource  设置ListView的数据源
  3. `initialListSize`  number  进行设置ListView组件刚刚加载的时候渲染的列表行数，用这个属性确定首屏或者首页加载的数量，而不是花大量的时间渲染加载很多页面数据，提高性能
  4. `onChangeVisibleRows`  function  (visibleRows,changedRows)=>void。当可见的行发生变化的时候回调该方法。visibleRows参数对所有可见的行为{selectionID:{rowId:true}}的形式，changedRow参数对已经改变可见的行为{selectionID:{rowID:true|false}}。该值true代表可见，false代表在视图之外不可见的行
  5. `onEndReachedThreshold`  number 当偏移量达到设置的临界值调用onEndReached
  6. `onEndReached` function 方法，当所有的数据项行被渲染之后，并且列表往下进行滚动。一直滚动到距离底部`onEndReachedThredshold`设置的值进行回调该方法。原生的滚动事件进行传递(通过参数的形式)
  7. `pageSize`   number 每一次事件的循环渲染的行数
  8. `removeClippedSubviews`  bool  该属性用于提供大数据列表的滚动性能。使用的时候需要给每一行(row)的布局添加over:'hidden'样式。该属性默认是开启状态
  9. `renderFooter` function 方法  ()=>renderable ，在每次渲染过程中头和尾总会重新进行渲染。如果发现该重新绘制的性能开销比较大的时候，可以使用`StaticContainer`容器或者其他合适的组件。在每一次渲染过程中Footer(尾)会一直在列表的底部，header(头)会一直在列表的头部
  10. `renderHeader`  function 方法 使用情况和上面的renderFooter差不多
  11. `renderRow` function 方法(rowData,sectionID,rowID,highlightRow)=>renderable   该方法有四个参数，其中分别为数据源中一条数据，分组的ID，行的ID，以及标记是否是高亮选中的状态信息
  12. `renderScrollComponent` function 方法 (props)=>renderable  该方法可以返回一个可以滚动的组件。默认该会返回一个`ScrollView`
  13. `renderSectionHeader` function (sectionData,sectionID)=>renderable   如果设置了该方法，这样会为每一个section渲染一个粘性的header视图。该视图粘性的效果是当刚刚被渲染开始的时候，该会处于对应的内容的顶部，然后开始滑动的时候，会跑到屏幕的顶端。直到滑动到下一个section的header(头)视图，然后被替代为止
  14. `renderSeparator` function  (sectionID,rowID,adjacentRowHighlighted)=>renderable 如果设置该方法，会在被每一行的下面渲染一个组件作为分隔。除了每一个section分组的头部视图前面的最后一行
  15. `scrollRenderAheadDistance` number  进行设置当该行进入屏幕多少像素以内之后就开始渲染该行

### 3. ListView高级特性
ListView可以支持一些高级特性，包括设置每一组的粘性头部(类似于iPhone)、支持设置列表的header以及footer视图、当数据列表滑动到最底部的时候支持onEndReached方法回调、设备屏幕列表可见的视图数据发生变化的时候回调onChangeVisibleRows以及一些性能方面的优化特性

ListView设计的时候，当需要动态加载非常大的数据的时候，下面有一些方法性能优化的方法可以让我们的ListView滚动的时候更加平滑：
  1. 只更新渲染数据变化的那一行  ，`rowHasChanged`方法会告诉ListView组件是否需要重新渲染当前那一行。具体可以查看ListViewDataSource实例
  2. 选择渲染的频率  默认情况下面 每一个`event-loop`(事件循环)只会渲染一行(可以同pageSize自定义属性设置)。这样可以把大的工作量进行分隔，提供整体渲染的性能

### 4. ListView 使用实例
1. 列表每一行显示一个图片以及文字，具体代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  ListView,
  TouchableOpacity,
  Image
} from 'react-native';
var THUMB_URLS = [
    require('./img/one.png'),
    require('./img/two.png'),
    require('./img/three.png'),
    require('./img/four.png'),
    require('./img/five.png'),
    require('./img/six.png'),
    require('./img/seven.png'),
    require('./img/eight.png'),
    require('./img/nine.png'),
    require('./img/ten.png')
];
class RNAppListView extends Component {
    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
        this.state = {dataSource: ds.cloneWithRows(['row 1', 'row 2', 'row 3', 'row 4', 'row 5', 'row 6', 'row 7', 'row 8', 'row 9', 'row10'])};

    }
    _renderRow(rowData: string, sectionID: number, rowID: number){
        var imgSource = THUMB_URLS[rowID];
        return(
            <TouchableOpacity>
                <View>
                    <View style={styles.row}>
                        <Image style={styles.thumb} source = {imgSource} />
                        <Text style={{flex: 1, fontSize: 16, color: 'blue'}}>
                            {rowData + ' 这些都是测试用的'}
                        </Text>
                    </View>
                </View>
            </TouchableOpacity>
        );
    }
    render() {
        return (
            <ListView
                dataSource={this.state.dataSource}
                renderRow={this._renderRow}
            />
        );
    }
}
const styles = StyleSheet.create({
    row:{
        flexDirection: 'row',
        justifyContent: 'center',
        padding: 10,
        backgroundColor: '#F6F6F6'
    },
    thumb:{
        width: 50,
        height: 50
    }
});
AppRegistry.registerComponent('RNAppListView', () => RNAppListView);
```
运行效果如下:
{% img /images/2016-07-2016-58-06.png %}
