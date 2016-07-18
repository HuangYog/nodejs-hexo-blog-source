---
title: 3.React-Native之View控件学习
date: 2016-07-13 16:40:24
tags:
- React-Native
- View
- React
categories:
- 日志
- 笔记
---
本文的参考资料为:{% link http://blog.csdn.net/llew2011/article/details/51068619%}

### 1. 查看项目中的index.android.js去的代码如下:
{%codeblock%}
import React, {  
  AppRegistry,  
  Component,  
  StyleSheet,  
  Text,  
  View  
} from 'react-native';  

class RNAppProject extends Component {  
  render() {  
    return (  
      <View style={styles.container}>  
        <Text style={styles.welcome}>  
          Welcome to React Native!  
        </Text>  
        <Text style={styles.instructions}>  
          To get started, edit index.android.js  
        </Text>  
        <Text style={styles.instructions}>  
          Shake or press menu button for dev menu  
        </Text>  
      </View>  
    );  
  }  
}  

const styles = StyleSheet.create({  
  container: {  
    flex: 1,  
    justifyContent: 'center',  
    alignItems: 'center',  
    backgroundColor: '#F5FCFF',  
  },  
  welcome: {  
    fontSize: 20,  
    textAlign: 'center',  
    margin: 10,  
  },  
  instructions: {  
    textAlign: 'center',  
    color: '#333333',  
    marginBottom: 5,  
  },  
});  

AppRegistry.registerComponent('RNAppProject', () => RNAppProject);
{%endcodeblock%}
该文件首先通过`import`引入了一些`React-Native`的组件，在下面定义了一个`RNAppProject`类，并且继承了`Component(组件)`，在往下看还有个style=StyleSheet.create(),该方法定义了一个全局的样式器。其实了解一点`Reactjs`的伙计应该都不难看懂这写代码。

### 2.尝试修改index.android.js 文件
下面我们 开始使用`View`控件来布局，代码如下:
{%codeblock%}
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

class RNAppProject extends Component {
  render() {
    return (
        <View style={styles.container}>
          <View style={styles.custom_container}>
            <View style={styles.custom_container_inner}></View>
          </View>
        </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF'
  },
  custom_container: {
    width: 400,
    height: 300,
    backgroundColor: '#7cb342',
    borderBottomColor: 'darkgray',
    borderBottomWidth: 20,
    borderLeftWidth: 10,
    borderLeftColor: 'antiquewhite',
    borderRightWidth: 5,
    borderRightColor: 'crimson',
    borderTopWidth: 2,
    borderTopColor: 'black',
    paddingLeft: 130,
    paddingTop: 110
  },
  custom_container_inner: {
    width: 150,
    height: 55,
    backgroundColor: '#e32a61',
    borderWidth: 1,
    borderRadius: 5,
    borderStyle: 'dashed',
    borderColor: '#ffffff'
  }

});

AppRegistry.registerComponent('RNAppProject', () => RNAppProject);
{%endcodeblock%}
当我们用真机调试时，将该项目部署后可以看到下面的界面:
{% img /images/webwxgetmsg.jpg %}
下面根据官网的质料和 {% link 这位老师的质料 http://blog.csdn.net/llew2011/article/details/51068619 %} 得到下面的总结出下面这张地图
{% img /images/2016-07-1317-01-43.png %}

`申明，本文产考资料为`http://blog.csdn.net/p106786860/article/details/51100811

### 3.控件中的style的写法
在控件中使用style在控制样式时有两种写法如下:
写法一
```
<View style={{flexDirection: 'Row', margin: 20, height: 100}}>
     <View style={{backgroundColor: 'red', flex:1}}></View>
 </View>
```
写法二
```
<View style={styles.view_1}>
    <View style={styles.view_2}></View>
</View>

var styles = StyleSheet.create({
  view_1: {
      flexDirection: 'row',
      padding: 20,
      height: 100
  },
  view_2: {
      backgroundColor: 'red',
      flex: 1
  }
});
```
