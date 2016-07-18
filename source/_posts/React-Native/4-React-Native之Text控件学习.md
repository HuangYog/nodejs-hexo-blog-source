---
title: 4.React-Native之Text控件学习
date: 2016-07-14 14:54:19
tags:
- React-Native
- Text
- React
categories:
- 日志
- 笔记
---
`{% link 本文参考江清清的React-Native系列文章 http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btext%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3/%}`


### Text 组件的嵌套使用
`Text` 组件和`View`组件一样都是`React-Native` 的基本组件，它对应的是android里的`TextView`,出来能实现基本的布局还可以进行嵌套显示，如下代码
{%codeblock%}

'use strict';

import React, {Component} from 'react';
import {
    AppRegistry, Text, StyleSheet
} from 'react-native';

class RNAppView extends Component {

    render(){
        return (
            <Text style={styles.titleBase}>
                I am root text !
                <Text style={styles.title}>
                    I am chid text !
                </Text>
            </Text>
        );
    }
}

var styles = StyleSheet.create({
    titleBase: {
        margin:10,
        textAlign: 'center',
        color: 'red',
        fontSize: 28,
        fontFamily: 'Cochin'
    },
    title: {
        color: 'green',
        fontWeight: 'bold'
    }
});

AppRegistry.registerComponent('RNAppView', () => RNAppView);
{%endcodeblock%}
运行后在手机上的效果如图所示:
{% img /images/2016-07-1416-04-50.png %}
这就是采用了简单的Text嵌套完成的样式。

### 2.Text 组件基本属性方法
 1.`allowFontScaling(bool)`: 控制字体是否根据IOS的设置进行自动缩放-iOS平台,Android平台不适用
 2.`numberOfLines (number)`:进行设置Text显示文本的行数，如果显示的内容超过了行数，默认其他多余的信息就不会显示了
 3.`onLayout (function)`：当布局位置发生变动的时候自动进行触发该方法, 其中该function的参数如下:
 [code lang="" start="" highlight=""]{nativeEvent: {layout: {x, y, width, height}}}[/code]
 4.`onPress (fcuntion)`:该方法当文本发生点击的时候调用该方法

### 3.Text 样式设置(style)
  1.继承可以使用View组件的所有Style，{% link 具体查看官方文档 http://facebook.github.io/react-native/docs/view.html#style %}
  2.color:字体颜色
  3.fontFamily:字体名称
  4.fontSize:字体大小
  5.fontStyle:字体风格((normal,italic)
  6.fontWeight:字体的粗细权重("normal", 'bold', '100', '200', '300', '400', '500', '600', '700', '800', '900')
  7.textShadowOffset:设置阴影效果{width: number,height:number}
  8.textShadowRadius: 阴影效果圆角
  9.textShadowColor:音乐效果的颜色
  10.letterSpacing:字符间距
  11.lineHeight:行高
  12.textAlign:文本对齐方式("auto", 'left', 'right', 'center', 'justify')
  13.textDecorationLine:横线位置(("none", 'underline', 'line-through', 'underline line-through')
  14.textDecorationStyle:线的风格("solid", 'double', 'dotted', 'dashed')
  15.textDecorationColor:线的颜色
  16.writingDirection 文本方向("auto", 'ltr', 'rtl')

### Text 嵌套需要注意的地方
相同的属性文本可以用父标签进行包裹，然后内部特殊的地方采用标签方案如下例子:
```
'use strict';

import React, {Component} from "react";
import {AppRegistry, Text, StyleSheet, View} from "react-native";

class RNAppView extends Component {

    render() {
        return (
            <View>
                <View>
                    <Text style={styles.titleBase}>
                        I am root text !
                        <Text style={styles.title}>
                            I am chid text !
                        </Text>
                    </Text>
                </View>
                <View>
                    <Text style={{fontWeight: 'bold', fontSize: 28}}>
                        I am bold
                        <Text style={{color: 'red'}}>     and red</Text>
                    </Text>
                </View>
            </View>
        );
    }
}

var styles = StyleSheet.create({
    titleBase: {
        margin: 10,
        textAlign: 'center',
        color: 'red',
        fontSize: 28,
        fontFamily: 'Cochin'
    },
    title: {
        color: 'green',
        fontWeight: 'bold'
    }
});

AppRegistry.registerComponent('RNAppView', () => RNAppView);
```
运行效果如下图所示
{% img /images/2016-07-1416-43-42.png %}
在布局方面的规则很重要，如前面说的`View`组件支持FlexBox(弹性布局),但是Text 组件只是文本布局，就是说只会横向布局，如果到了文本的末尾就只有换行，如下代码:
```
<View style={{borderWidth:1,borderStyle: 'dashed',borderColor: 'blue'}}>
    <Text style={{fontWeight: 'bold', fontSize: 28}}>
        水平分布  I am bold
        <Text style={{color: 'red'}}>     and red</Text>
    </Text>
</View>
```
如果父节点是采用`View`，那就能实现纵向布局
```
<View style={{borderWidth:1,borderStyle: 'dotted',borderColor: 'green'}}>
    <Text style={{fontWeight: 'bold', fontSize: 28}}> 垂直分布 I am bold</Text>
    <Text style={{color: 'red'}}>     and red</Text>
</View>
```
运行后的效果如下图所示:
{% img /images/2016-07-1417-06-42.png %}
从上面的例子中可以看出组件可以嵌套，而且样式还支持继承，也就是说父组件定义的样式，子组件没有重写颜色之前是可以继承福组件的样式的。

### Text 的具体实例
```
/**
 * 简单的Text 组件使用
 */
'use strict';


import React, {Component} from "react";
import {AppRegistry, Text, StyleSheet, View} from "react-native";

class RNAppView extends Component {

    render() {
        return (
            <View>
                <Text>
                    <Text style={styles.titleBase}>
                        I am root text !
                        <Text style={styles.title}>
                            I am chid text !
                        </Text>
                    </Text>
                </Text>
                <View style={{borderWidth:1,borderStyle: 'dashed',borderColor: 'blue'}}>
                    <Text style={{fontWeight: 'bold', fontSize: 28}}>
                        水平分布  I am bold
                        <Text style={{color: 'red'}}>     and red</Text>
                    </Text>
                </View>
                <View style={{borderWidth:1,borderStyle: 'dotted',borderColor: 'green'}}>
                    <Text style={{fontWeight: 'bold', fontSize: 28}}> 垂直分布 I am bold</Text>
                    <Text style={{color: 'red'}}>     and red</Text>
                </View>
                <View>
                    <Text style={{color: 'red'}}>这是红色</Text>
                    <Text style={{color: 'green', fontSize: 20}}>这是绿色，大小为20</Text>
                    <Text style={{color: 'green', fontFamily: 'Cochin'}}>这是绿色，字体为Cochin</Text>
                    <Text style={{color: 'pink', fontWeight: 'blod'}}>这是粉色，字体加粗</Text>
                    <Text style={{color: 'gray', fontStyle: 'italic'}}>这是灰色，斜字体</Text>
                    <Text style={{textAlign: 'center', fontStyle: 'italic'}}>居中显示，斜体</Text>
                    <Text style={{textAlign:'center',fontStyle:'italic'}} numberOfLines={1}>
                        测试行数 居中和斜体 ,再多的数据也只会显示一行        再多的数据也只会显示一行   再多的数据也只会显示一行
                    </Text>
                    <Text style={{marginLeft:50,marginTop:50,textAlign:'center',fontStyle:'italic'}}>
                        设置文本的间距,居左，居顶部50
                    </Text>
                    <Text numberOfLines={2} style={{lineHeight:50,textAlign:'center',fontStyle:'italic'}}>
                        测试行高 测试行高 测试行高 测试行高 测试行高 测试行高 测试行高 测试行高 测试行高 测试行高 测试行高
                        测试行高 测试行高 测试行高 测试行高 测试行高 测试行高
                    </Text>
                </View>
            </View>
        );
    }
}

var styles = StyleSheet.create({
    titleBase: {
        margin: 10,
        textAlign: 'center',
        color: 'red',
        fontSize: 28,
        fontFamily: 'Cochin'
    },
    title: {
        color: 'green',
        fontWeight: 'bold'
    }
});

AppRegistry.registerComponent('RNAppView', () => RNAppView);
```
运行后的效果如下:
{% img /images/2016-07-1417-29-57.png %}

``声明``本文主要是参考了`From Sky丶清` 的 {% link React-Native 系列为文章 http://www.lcode.org/react-native/ %}
