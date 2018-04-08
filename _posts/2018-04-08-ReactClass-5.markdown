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
import CoinDetail from './CoinDetail';

class CoinView extends React.Component {
    render(){
        return (
            <View style={this.props.style}>
                <CoinDetail />
                <CoinDetail />
                <CoinDetail />
                <CoinDetail />
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

## CoinDetail
```js
import React from 'react';
import { StyleSheet, Text, View, Image } from 'react-native';

class CoinDetail extends React.Component {
    render(){
        let date = new Date();
        let now = date.toLocaleString();

        return(
            <View style={styles.container}>
                <Image
                style={{width: 50, height: 50}}
                source={{uri: 'https://bitcoin.org/img/icons/opengraph.png'}}
                />
                <Text style={[styles.text]}>{'#' + (this.props.rank || 'Rank')}</Text>
                <Text style={[styles.text]}>{this.props.name || 'Name'}</Text>
                <Text style={[styles.text]}>{'Price: ' + (this.props.price || 0)}</Text>
                <Text style={[styles.text]}>{'Volume: ' + (this.props.volumn || 0)}</Text>
                <Text style={[styles.text]}>{'Updated: ' + (this.props.time || 0)}</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        width: '100%',
        height: 80,
        flexDirection: 'row',
        backgroundColor: 'blue',
        alignItems: 'center',
        justifyContent: 'space-around',
        marginTop: 5,
        marginBottom: 5,
    },
    text: {
        color: 'white',
    }
});

export default CoinDetail;
```

## CoinDetail
```js
import React from 'react';
import { StyleSheet, Text, View, Image } from 'react-native';

class CoinDetail extends React.Component {
    render(){
        let date = new Date();
        let now = date.toLocaleString();

        return(
            <View style={styles.container}>
                <Image
                style={{width: 50, height: 50}}
                source={{uri: 'https://bitcoin.org/img/icons/opengraph.png'}}
                />
                <Text style={[styles.text]}>{'#' + (this.props.rank || 'Rank')}</Text>
                <Text style={[styles.text]}>{this.props.name || 'Name'}</Text>
                <Text style={[styles.text]}>{'Price: ' + (this.props.price || 0)}</Text>
                <Text style={[styles.text]}>{'Volume: ' + (this.props.volumn || 0)}</Text>
                <Text style={[styles.text]}>{'Updated: ' + (this.props.time || 0)}</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        width: '100%',
        height: 80,
        flexDirection: 'row',
        backgroundColor: 'blue',
        alignItems: 'center',
        justifyContent: 'space-around',
        marginTop: 5,
        marginBottom: 5,
    },
    text: {
        color: 'white',
    }
});

export default CoinDetail;
```

## CoinView
```js
import React from 'react'
import { StyleSheet, Text, View } from 'react-native';
import CoinDetail from './CoinDetail';

const sampleData = [
    {
        "id": "bitcoin",
        "name": "Bitcoin",
        "symbol": "BTC",
        "rank": "1",
        "price_usd": "6965.45",
        "price_btc": "1.0",
        "24h_volume_usd": "3800210000.0",
        "market_cap_usd": "118167292024",
        "available_supply": "16964775.0",
        "total_supply": "16964775.0",
        "max_supply": "21000000.0",
        "percent_change_1h": "-0.42",
        "percent_change_24h": "1.41",
        "percent_change_7d": "1.01",
        "last_updated": "1523167466"
    },
    {
        "id": "ethereum",
        "name": "Ethereum",
        "symbol": "ETH",
        "rank": "2",
        "price_usd": "388.814",
        "price_btc": "0.0559943",
        "24h_volume_usd": "913623000.0",
        "market_cap_usd": "38370409742.0",
        "available_supply": "98685772.0",
        "total_supply": "98685772.0",
        "max_supply": null,
        "percent_change_1h": "-0.53",
        "percent_change_24h": "1.93",
        "percent_change_7d": "-0.23",
        "last_updated": "1523167454"
    },
    {
        "id": "ripple",
        "name": "Ripple",
        "symbol": "XRP",
        "rank": "3",
        "price_usd": "0.489639",
        "price_btc": "0.00007051",
        "24h_volume_usd": "164705000.0",
        "market_cap_usd": "19142201983.0",
        "available_supply": "39094520623.0",
        "total_supply": "99992405149.0",
        "max_supply": "100000000000",
        "percent_change_1h": "-0.28",
        "percent_change_24h": "-0.08",
        "percent_change_7d": "-2.42",
        "last_updated": "1523167441"
    },
    {
        "id": "bitcoin-cash",
        "name": "Bitcoin Cash",
        "symbol": "BCH",
        "rank": "4",
        "price_usd": "648.761",
        "price_btc": "0.0934299",
        "24h_volume_usd": "208976000.0",
        "market_cap_usd": "11068843911.0",
        "available_supply": "17061513.0",
        "total_supply": "17061513.0",
        "max_supply": "21000000.0",
        "percent_change_1h": "-0.37",
        "percent_change_24h": "2.61",
        "percent_change_7d": "-3.97",
        "last_updated": "1523167451"
    },
]

class CoinView extends React.Component {
    render(){
        let coinDetailCells = (
            <View>
              <CoinDetail />
              <CoinDetail />
              <CoinDetail />
              <CoinDetail />
            </View>
        );

        let detailCells = [];

        sampleData.map((data) => {
            let coinDetail = (
                <CoinDetail
                    key={data.index}
                    rank={data.rank}
                    name={data.name}
                    price={data.price_usd}
                    volumn={data.market_cap_usd}
                />
            );

            detailCells.push(coinDetail);
        });

        return (
            <View style={this.props.style}>
                {detailCells}
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
샘플데이터를 이용

```js
class CoinView extends React.Component {
    _getCoinDatas(limit) {
        this.setState({
            isLoaded: false,
        })

        fetch(
            'https://api.coinmarketcap.com/v1/ticker/?limit=${limit}'
        )
        .then(response => response.json())
        .then(data => {
            this.setState({
                coinDatas: data,
                isLoaded: true,
            })
        });
    }

    componentDidMount() {
        this._getCoinDatas(10);

        setInterval(() => {
            this._getCoinDatas(10);
            console.log('toggled!');
        }, 1000)
    }

    constructor(props) {
        super(props);
        this.state = {
            coinDatas: [],
            isLoaded: false,
        }
    }

    render(){
        let detailCells = [];

        this.state.coinDatas.map((data, index) => {
            let coinDetail = (
                <CoinDetail
                    key={data.index}
                    rank={data.rank}
                    name={data.name}
                    price={data.price_usd}
                    volumn={data.market_cap_usd}
                />
            );

            detailCells.push(coinDetail);
        });

        return (
            <View style={this.props.style}>
                {detailCells}
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
api 데이터를 이용

```js
render(){
		let detailCells = [];

		this.state.coinDatas.map((data, index) => {
				let coinDetail = (
						<CoinDetail
								key={index}
								rank={data.rank}
								name={data.name}
								price={data.price_usd}
								volumn={data.market_cap_usd}
						/>
				);

				detailCells.push(coinDetail);
		});

		return (
				<ScrollView style={this.props.style}>
						{detailCells}
				</ScrollView>
		);
}
```
scroll view를 사용

## Constants
```js
/**
  Icons from: https://github.com/cjdowner/cryptocurrency-icons/tree/master/32%402x/icon
*/
export function getCoinIconUri(coinName) {
    switch (coinName) {
      case 'Bitcoin':
        return 'https://github.com/cjdowner/cryptocurrency-icons/blob/master/32@2x/icon/btc@2x.png?raw=true';

      case 'Ethereum':
        return 'https://github.com/cjdowner/cryptocurrency-icons/blob/master/32@2x/icon/eth@2x.png?raw=true';

      case 'Ripple':
        return 'https://github.com/cjdowner/cryptocurrency-icons/blob/master/32@2x/icon/xrp@2x.png?raw=true';

      case 'Bitcoin Cash':
        return 'https://github.com/cjdowner/cryptocurrency-icons/blob/master/32@2x/icon/bcc@2x.png?raw=true';

      case 'Litecoin':
        return 'https://github.com/cjdowner/cryptocurrency-icons/blob/master/32@2x/icon/ltc@2x.png?raw=true';

      default:
        return 'https://assets-cdn.github.com/images/modules/logos_page/GitHub-Mark.png';
    }
}
```

## CoinView
```js
import { getCoinIconUri } from '../libs/Constants';
...
render(){
        let detailCells = [];

        this.state.coinDatas.map((data, index) => {
            let coinDetail = (
                <CoinDetail
                    iconUri={getCoinIconUri(data.name)}
                    key={index}
                    rank={data.rank}
                    name={data.name}
                    price={data.price_usd}
                    volumn={data.market_cap_usd}
                />
            );

            detailCells.push(coinDetail);
        });

        return (
            <ScrollView style={this.props.style}>
                {detailCells}
            </ScrollView>
        );
    }
```
icon 사용
