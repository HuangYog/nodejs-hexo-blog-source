---
title: 11.React-Native之ViewPagerAndroid组件学习
date: 2016-07-19 16:50:11
tags:
- React-Native
- ViewPagerAndroid
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. ViewPagerAndroid组件介绍
`ViewPagerAndroid组件介绍`组件和Android中的ViewPager控件类似。该组件运行容器中的子视图相互的左右滑动，每个ViewPagerAndroid中的子视图都会当做一个单独的页面，并且会撑满整个ViewPagerAndroid组件的界面 `特别注意`ViewPagerAndroid 中的所有子View必须为<View>组件，不能为复合类型的组件，可以为每个子视图添加样式，如: padding或者backgroundColor 之类的属性。

### 2. 官方例子
实现一个图片轮播效果，代码如下：
```
'use strict';
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
    ViewPagerAndroid
} from 'react-native';

class RNAppViewPager extends Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          ViewPagerAndroid 实例 一
        </Text>
        <ViewPagerAndroid
            style={styles.pageStyle}
            initialPage = {0}>
            <View style={{backgroundColor: '#6d9eeb'}}>
                <Text>First Page !</Text>
            </View>
            <View style={{backgroundColor: '#674ea7'}}>
                <Text>Second Page !</Text>
            </View>
        </ViewPagerAndroid>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  pageStyle:{
      alignItems: 'center',
      padding: 20,
      height: 200
  }
});

AppRegistry.registerComponent('RNAppViewPager', () => RNAppViewPager);
```
运行后看到的效果,用手滑动会看到 Second Page 如下:
{% img /images/2016-07-1917-28-51.png %}{% img /images/2016-07-1917-31-12.png %}

### 3. 属性方法
  1. `View`相关属性样式全部继承(宽和高,背景颜色,边距等相关属性样式)
  2. `initialPage` number `ViewPagerAndroid` 初始索引叶，不过我们可以使用SetPage方法来跟新页码，通过onPageSelected方法来监听页面滑动
  3. `keyBoardDismissMode` enum('none', 'on-drag') 枚举类型，进行设置在拖拽滑动的过程中是否要显示键盘:
    - `none` 默认值，在拖拽中不隐藏键盘
    - ‘on-drag’ 当拖拽滑动开始的时候隐藏键盘
  4. `onPageScroll` function 方法，该方法在页面进行滑动的时候执行(不管是因为页面滑动动画原因还是鱼鱼页面之间的拖拽以及滑动原因)，会回调传入的event.nativeEvent 对象会有携带如下的参数:
    - `postition` 从左起开始第一个可见的页面索引
    - `offset` 该value值的范围为[0~1),用来表示当前页面的切换状态，值x表示该索引页面(1-x)的范围可见，另外x范围代表下一个页面可见的区域
  5. `onPageScrollStateChanged` function 该回调方法会在页面滚动状态发生变化的时候进行调用，页面的滚动状态有下面三种:
    - `idle` 表示当前用户和页面滚动没有任何交互
    - `dragging` 表示当前页面正在被拖拽滑动中
    - `setting` 表示存在页面拖拽或者是滑动的交互，页面滚动正在结束，并且正在关闭或者打开动画
  6.onPageSelected function 表示在该页面拖拽滑动切换完成之后回调，该方法回调参数中的event.nativeEvent 对象会携带如下一个属性： `position` 表示当前选中的页面的索引

### 4. ViewPagerAndroid 实例
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    Image,
    ViewPagerAndroid
} from 'react-native';

var titles_first_data = ["美食", "电影", "酒店", "KTV", "外卖", "优惠买单", "周边游", "休闲娱乐", "今日新单", "丽人"];
var titles_second_data = ["购物", "火车票", "生活服务", "旅游", "汽车服务", "足疗按摩", "小吃快餐", "经典门票", "境外游", "全部分类"];

class RNAppViewPager extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {page: 1};
    }

    onPageSelected = (e) => {
        this.setState({page: 1 + e.nativeEvent.position});
    };

    render() {
        return (
            <View>
                <Text style={{textAlign: 'center'}}>
                    模仿实现美团首页顶部分类
                </Text>
                <ViewPagerAndroid
                    style={styles.pageStyle}
                    initialPage={0}
                    onPageSelected={this.onPageSelected}
                >
                    <View>
                        <View style={{flexDirection: 'row'}}>
                            <View style={{width: 70}}>
                                <Image source={require('./img/one.png')} style={styles.imageStyle} />
                                <Text style={styles.textStyle}>{titles_first_data[0]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/two.png')} style={styles.imageStyle} />
                                <Text style={styles.textStyle}>{titles_first_data[1]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/three.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[2]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/four.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[3]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/five.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[4]}</Text>
                            </View>
                        </View>
                        <View style={{flexDirection: 'row'}}>
                            <View style={{width: 70}}>
                                <Image source={require('./img/six.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[5]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/seven.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[6]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/eight.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_first_data[7]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/nine.png')} style={styles.imageStyle} />
                                <Text style={styles.textStyle}>{titles_first_data[8]}</Text>
                            </View>
                            <View style={{width: 70}}>
                                <Image source={require('./img/ten.png')} style={styles.imageStyle} />
                                <Text style={styles.textStyle}>{titles_first_data[9]}</Text>
                            </View>
                        </View>
                    </View>
                    <View>
                        <View style={{flexDirection:'row'}}>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_one.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[0]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_two.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[1]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_three.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[2]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_four.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[3]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_five.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[4]}</Text>
                            </View>
                        </View>
                        <View style={{flexDirection:'row',marginTop:10}}>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_six.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[5]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_seven.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[6]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_eight.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[7]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_nine.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[8]}</Text>
                            </View>
                            <View style={{width:70}}>
                                <Image source={require('./img/next_ten.png')} style={styles.imageStyle}/>
                                <Text style={styles.textStyle}>{titles_second_data[9]}</Text>
                            </View>
                        </View>
                    </View>
                </ViewPagerAndroid>
                <Text style={{flex: 1, alignSelf: 'center'}}>当前是第 {this.state.page} 页</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    pageStyle: {
        alignItems: 'center',
        padding: 20,
        height: 150
    },
    imageStyle: {
        alignSelf: 'center',
        width: 45,
        height: 45
    },
    textStyle: {
        marginTop: 5,
        alignSelf: 'center',
        fontSize: 11,
        color: '#555555',
        textAlign: 'center'
    }
});

AppRegistry.registerComponent('RNAppViewPager', () => RNAppViewPager);
```
以上代码需要注意的是这个地方的写法:
```
// 报错 this.setState is not a function
onPageSelected (e){
    this.setState({page: 1 + e.nativeEvent.position});
};

// 不报错
onPageSelected = (e) => {
    this.setState({page: 1 + e.nativeEvent.position});
};
```

运行后看到的效果如下:
{% img /images/2016-07-1923-14-42.png%}{% img /images/2016-07-1923-14-59.png%}
