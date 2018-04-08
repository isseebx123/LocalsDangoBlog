---
layout: post
title:  "3일차. 리액트 특강 1일차(10:00 ~ 18:00)"
date:   2018-04-08 00:00:00
author: 이상현
categories: React
---

# React에서 React Native로 migration
<table>
	<tr>
		<td>React</td>
		<td>React Native</td>
	</tr>
	<tr>
		<td>div</td>
		<td>View</td>
	</tr>
	<tr>
		<td>span</td>
		<td>Text</td>
	</tr>
	<tr>
		<td>li,ul</td>
		<td>ListView</td>
	</tr>
	<tr>
		<td>img</td>
		<td>image</td>
	</tr>
</table>

# React Native
## flex
- flex:1, flex:1이면 화면을 세로로 50대 50으로 가진다.
- flex:1, flex:2이면 화면을 세로로 33대 66으로 가진다.
## flexDirection
- flex direction의 default는 column이다.
- flex direction가 row이면 가로 방향으로 생긴다.
- ex) flexDirection: 'row'.. 'column'
## justifyContent
- ex) justifyContent: 'space-around'
## alignItems
- ex) alignItems: 'centers'
## alignSelf
- 부모가 정해준 것을 하지않음.
- ex) alignSelf: 'flex-end'.. 'flex-start', 'auto', 'center'

# Expo XDE
1. 프로젝트와 연동
- USB로 하는 경우 Host-Local으로 설정
- Expo에서 제공해주는 서버로 하는 경우 Host-Tunnel으로 설정
- Wifi로 하는 경우 Host-LAN으로 설정
- 만약 XDE가 잘안된다면 [스낵](https://snack.expo.io/@isseebx/mypj)을 이용

2. 컴포넌트란
- 독립전인 단위 모듈
- React.js에서 상속

# 프로젝트 진행
1. 컴포넌트 구현
- 관련 [깃허브](https://github.com/JeffGuKang/ReactNative-Tutorial)

## CoinView.js
```js
import React from 'react'
import { StyleSheet, Text, View } from 'react-native';

class CoinView extends React.Component {
    render(){
        return (
            <View style={styles.container}>
                <Text>AAA</Text>
                <Text>BBB</Text>
                <Text>CCC</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        flexDirection: 'column',
        backgroundColor: 'blue',
        alignItems: 'center',
        justifyContent: 'space-around',
    }
});

export default CoinView;
```

```js
import React from 'react'
import { StyleSheet, Text, View } from 'react-native';

class CoinView extends React.Component {
    render(){
        return (
            <View style={this.props.style}>
                <Text>AAA</Text>
                <Text>BBB</Text>
                <Text>CCC</Text>
            </View>
        );
    }
}

export default CoinView;
```
props(property)를 통해 style을 받을 수 있다.

## App.js
```js
import React from 'react';
import { StyleSheet, Text, View, StatusBar } from 'react-native';
import TopBar from './components/TopBar';
import CoinView from './components/CoinView';

export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
      <StatusBar hidden={true} barStyle="light-content" />
      <TopBar title="im title"/>
      <CoinView style={styles.coinView}></CoinView>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'yellow',
    //backgroundColor: '#fff',
    //alignItems: 'center',
    //justifyContent: 'center',
  },
  coinView: {
    width: '100%',
    flex: 1,
    flexDirection: 'column',
    backgroundColor: 'skyblue',
    alignItems: 'center',
    justifyContent: 'space-around',
  }
});
```
