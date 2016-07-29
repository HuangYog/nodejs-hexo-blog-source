---
title: 22.React-Native之Modal组件学习
date: 2016-07-29 11:33:29
tags:
- React-Native
- Modal
- React
categories:
- 日志
- 笔记
---

本文查考`From Sky丶清（http://www.lcode.org/）` 的 {% link React Native专题 http://www.lcode.org/react-native/%}

### 1. Modal介绍
`Modal` 可以弹出来覆盖包含React Native跟视图的原生界面(例如:UiViewControllView,Activity)。在使用React Native开发的混合应用中使用Modal组件，该可以让你使用RN开发的内功呈现在原生视图的上面,如果使用React Native开发的应用，从跟视图就开始开发起来了，那么你应该是Navigator导航器进行控制页面弹出，而不是使用Modal模态视图。通过顶层的Navigator，你可以使用configureScene属性进行控制如何在你开发的App中呈现一个Modal视图

### 2. Modal属性方法
  1. `animated` bool  控制是否带有动画效果
  2. `onRequestClose`  Platform.OS==='android'? PropTypes.func.isRequired : PropTypes.func
  3. `onShow` function方法
  4. `transparent` bool  控制是否带有透明效果
  5. `visible`  bool 控制是否显示

### 2. Modal应用实例
实例代码如下:
```
'use strict';
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    TouchableHighlight,
    Modal,
    Switch
} from 'react-native';

class Button extends Component {
    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            active: false
        };
        this._onHighlight = this.onHighlight.bind(this);
        this._onUnhighlight = this.onUnhighlight.bind(this);
      }
    onHighlight(){
        this.setState({active: true});
    }
    onUnhighlight(){
        this.setState({active: false});
    }

    render(){
        var colorStyle = {
            color: this.state.active ? '#fff' : '#000'
        };
        return (
            <TouchableHighlight
                onHideUnderlay={this._onUnhighlight}
                onPress={this.props.onPress}
                onShowUnderlay={this._onHighlight}
                style={[styles.button, this.props.style]}
                underlayColor='#a9d9d4'>
                <Text style = {[styles.buttonText, colorStyle]}>{this.props.children}</Text>
            </TouchableHighlight>
        );
    }
}

class RNAppModal extends Component {

    // 构造
      constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            animationType: false,
            modalVisible: false,
            transparent: false
        };
        this._toggleTransparent = this.toggleTransparent.bind(this);
      }

      _setModalVisible(visible){
          this.setState({modalVisible: visible});
      }
      _setAnimationType(type){
        this.setState({animationType: type});
      }
    toggleTransparent(){
          this.setState({transparent: !this.state.transparent});
      }



    render() {

        const modalBackgroundStyle = {
            backgroundColor: this.state.transparent ? 'rgba(0, 0, 0, 0.5)' : '#f5fcff'
        };

        const innerConstainerTransparentSTyle = this.state.transparent ? {backgroundColor: 'red', padding: 20} : null;

        return (
            <View style={{padding: 20, paddingLeft: 10, paddingRight: 10}}>
                <Text style={{color: 'red'}}>Modal 实例演示</Text>
                <Modal
                    animatedType={this.state.animationType}
                    transparent={this.state.transparent}
                    visible={this.state.modalVisible}
                    onRequestClose={() => {this._setModalVisible(false)}}>
                    <View style={[styles.container, modalBackgroundStyle]}>
                        <View style={[styles.innerContainer, innerConstainerTransparentSTyle]}>
                            <Text>Modal 视图呗显示,
                                {this.state.animationType === false ? '没有' : '有' + this.state.animationType}动画效果
                            </Text>
                            <Button
                                onPress={this._setModalVisible.bind(this, false)}
                                style={styles.modalButton}>
                                 关闭 Modal
                            </Button>
                        </View>
                    </View>
                </Modal>
                <View style={styles.row}>
                    <Text style={styles.rowTitle}>动画类型</Text>
                    <Button
                        onPress={this._setAnimationType.bind(this, false)}
                        style={this.state.animationType === false ? {backgroundColor: 'red'}: {}}>
                        无动画
                    </Button>
                    <Button
                        onPress={this._setAnimationType.bind(this, true)}
                        style={this.state.animationType === true ? {backgroundColor: 'yellow'}: {}}>
                        无动画
                    </Button>
                </View>
                <View style={styles.row}>
                    <Text style={styles.rowTitle}>透明</Text>
                    <Switch value={this.state.transparent} onValueChange={this._toggleTransparent} />
                    <Button onPress={this._setModalVisible.bind(this, true)}>显示Modal</Button>
                </View>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        padding: 20
    },
    innerContainer: {
        borderRadius: 10,
        alignItems: 'center'
    },
    row: {
        alignItems: 'center',
        flex: 1,
        flexDirection: 'row',
        marginBottom: 20
    },
    rowTitle:{
        flex: 1,
        fontWeight: 'bold'
    },
    button: {
        borderRadius: 5,
        flex: 1,
        height: 44,
        alignSelf: 'stretch',
        justifyContent: 'center',
        overflow: 'hidden'
    },
    buttonText: {
        fontSize: 18,
        margin: 5,
        textAlign: 'center'
    },
    modalButton: {
        marginTop: 10
    }
});

AppRegistry.registerComponent('RNAppModal', () => RNAppModal);
```
运行后看到的效果如下:
{% img /images/2016-07-29-14-04-39.gif %}
