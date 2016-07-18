---
title: 5.React-Native之Image控件学习
date: 2016-07-15 17:09:13
tags:
- React-Native
- Text
- React
categories:
- 日志
- 笔记
---
本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}
和之前一样我们先初始化一个项目
{%codeblock%}
$ react-native init RNAppImage
{%endcodeblock%}

### 1.Image基本用法
项目创建好之后我们先简单的修改下 `index.android.js`文件如下:
```
  <View style = {{marginLeft: 10, marginTop: 10}}>
    <Text style = {{fontSize: 16}}>test demo</Text>
    <Image source = {require('./img/my_icon.png')}/>
  </View>
```
运行之后得到结果如下
![](/images/2016-07-1517-20-50.png)
该图片资源的查找和js模块相似，根据填写的图片路径相对于当前的js文件来进行收索，React-Native会根据平台选择的相应文件 例如:my_icon.ios.png和my_icon.android.png两个文件(命名方式android或者ios)。该会根据android或者ios平台选择相应的文件。
注意这边使用的`Image`组件中的`require`中的图片名称必须为一个静态的字符串信息.不能在require中进行拼接如下:
```
<Image source = {require('./imgmy_icon' + '.png')}/>
```
这中写法是会报错的。

### 2.加载使用APP中的图片
React-Native允许开发者在React-Native项目中使用原生代码开发，也可以使用已经打包在app中的图片资源。如下:
```
<Image source = {{uri:'ic_launcher'}} style = {{width: 80, height: 80}}/>
```

### 3.加载网络图片
```
<Image source={{uri:'https://d13yacurqjgara.cloudfront.net/users/845817/screenshots/2837728/pikachu_by_adriana_depta.png'}} style={{width: 160, height: 160}}></Image>
```

### 4.Image实现某些控件的背景图效果
```
<Image source={{uri:'https://d13yacurqjgara.cloudfront.net/users/845817/screenshots/2837728/pikachu_by_adriana_depta.png'}} style={{width: 160, height: 160}}><Text style={{color: 'green'}}>这是从网络上加载的图片</Text></Image>
```
最后的效果图如下:
![](/images/2016-07-1517-43-41.png)

### 5.Image的一些属性方法
1.onLayout   (function) 当Image布局发生改变的，会进行调用该方法，调用的代码为:
{nativeEvent: {layout: {x, y, width, height}}}.
2.onLoad (function):当图片加载成功之后，回调该方法
3.onLoadEnd (function):当图片加载失败回调该方法，不管图片加载成功还是失败
4.onLoadStart (fcuntion):当图片开始加载的时候调用该方法
5.resizeMode  缩放比例,可选参数('cover', 'contain', 'stretch') 该当图片的尺寸超过布局的尺寸的时候，会根据设置Mode进行缩放或者裁剪图片
6.source {uri:string} 进行标记图片的引用，该参数可以为一个网络url地址或者一个本地的路径

### 6.Image的样式
1.FlexBox         支持弹性盒子风格
2.Transforms      支持属性动画
3.resizeMode      设置缩放模式
4.backgroundColor 背景颜色
5.borderColor     边框颜色
6.borderWidth     边框宽度
7.borderRadius    边框圆角
8.overflow        设置图片尺寸超过容器可以设置显示或者隐藏('visible','hidden')
9.tintColor       颜色设置  
10.opacity        设置不透明度0.0(透明)-1.0(完全不透明)

### 7.Image实例-仿照美团首页顶部分类
直接上代码吧
```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    Image
} from 'react-native';

class RNAppImage extends Component {
    render() {
        return (
            <View style={{marginLeft: 5, marginTop: 10, marginRight: 5}}>
                <View style={{flexDirection: 'row'}}>

                    <View style={{width: 70}}>
                        <Image source={require('./img/one.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>美食</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/two.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>美食</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/three.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>酒店</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/four.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>KTV</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/five.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>外卖</Text>
                    </View>
                </View>
                <View style={{flexDirection: 'row'}}>

                    <View style={{width: 70}}>
                        <Image source={require('./img/six.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>优惠买单</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/seven.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>周边游</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/eight.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>休闲娱乐</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/nine.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>今日新单</Text>
                    </View>

                    <View style={{width: 70}}>
                        <Image source={require('./img/ten.png')} style={{alignSelf: 'center', width: 45, height: 45}} />
                        <Text style={{marginTop: 5, textAlign: 'center', fontSize: 11, color:'#555555'}}>丽人</Text>
                    </View>
                </View>
            </View>
        );
    }
}

AppRegistry.registerComponent('RNAppImage', () => RNAppImage);
```
运行后看到的效果图如下:
{% img /images/2016-07-1517-54-51.png %}

`图片下载地址`:链接: http://pan.baidu.com/s/1kUKJ64Z 密码: 9p3q

`再次声明`:本文是参考 `From Sky丶清（http://www.lcode.org/）`的 `{% link React-Native系列文章 http://www.lcode.org/react-native/ %}`
