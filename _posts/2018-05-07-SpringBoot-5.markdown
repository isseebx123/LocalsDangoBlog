---
layout: post
title:  "SpringBoot 6일차 (react)"
date:   2018-05-07 23:18:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 페이지를 분리해보자 !
## 구성
1. Home
```js
import React from 'react'

class Home extends React.Component {
    render() {
        return (
            <div>
                <h1>Home</h1>
            </div>
        )
    }
}

export default Home;
```
<pre>

</pre>
2. SideBar
```js
import React from 'react'

class SideBar extends React.Component{
    constructor(props) {
        super(props);
    }

    render(){
        return (
            <div>
                <a href="#" onClick={() => {this.props._setPage('Home');}}>Home</a>
                <a href="#" onClick={() => {this.props._setPage('Time');}}>Time</a>
                <a href="#" onClick={() => {this.props._setPage('BoardPage');}}>Board</a>
            </div>
        )
    }
}

export default SideBar;
```
<pre>

</pre>
3. BoardPage
```js
import React from 'react'

const client = require('../client');

class BoardPage extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            boards: [],
            isLoading: false,
        }
    }

    componentDidMount() {
        client({method: 'GET', path: '/selectBoard'}).done(response => {
            this.setState({
                boards: response.entity,
                isLoading: true,
            });
        });
    }

    render() {
        if (this.state.isLoading === true) {
            return (
                <Boards boards={this.state.boards}/>
            )
        }
        else {
            return (
                <div>
                    <p>Hi, Im BoardPage</p>
                    <p>Loading ...</p>
                </div>
            )
        }
    }
}

// tag::board-list[]
class Boards extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        var boards = this.props.boards.map(board =>
            <Board key={board.bno} board={board}/>
        );
        return (
            <table>
                <tbody>
                <tr>
                    <th>bno</th>
                    <th>title</th>
                    <th>content</th>
                </tr>
                {boards}
                </tbody>
            </table>
        )
    }
}

// end::board-list[]

// tag::board[]
class Board extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <tr>
                <td>{this.props.board.bno}</td>
                <td>{this.props.board.title}</td>
                <td>{this.props.board.content}</td>
            </tr>
        )
    }
}

// end::board[]

export default BoardPage;
```
<pre>

</pre>
4. Time
```js
import React from 'react'

class Time extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            date: null,
        }
        this._getCurrentTime = this._getCurrentTime.bind(this);
    }

    _getCurrentTime() {
        this.setState({
            date: new Date(),
        })
    }

    render() {
        return (
            <div>
                <p>This is page for time tutorial</p>
                <input type="button" value="currentTime" onClick={this._getCurrentTime}/>
                <p>Current Time is {this.state.date === null ? '' : this.state.date.toLocaleTimeString()} !!</p>
            </div>
        )
    }
}

export default Time;
```
<pre>

</pre>

## app.js
```js
'use strict';

// tag::vars[]
const React = require('react');
const ReactDOM = require('react-dom');
import SideBar from './component/SideBar'
import Time from './component/Time'
import BoardPage from './component/BoardPage'
import Home from './component/Home'
const client = require('./client');
// end::vars[]

// tag::app[]
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            page: 'Home',
        };
    }

    _setPage(pageName){ // pageName
        this.setState({
            page: pageName,
        })
    }

    _getPage(){
        const {page} = this.state;
        if(page === 'Home') {
            return <Home />
        }
        else if(page === 'Time') {
            return <Time />
        }
        else if(page === 'BoardPage') {
            return <BoardPage />
        }
    }

    render() {
        return (
            <div>
                <SideBar _setPage={(page) => {this._setPage(page)}}/>
                <p>Hi, im react!</p>
                {this._getPage()}
            </div>
        )
    }
}
// end::app[]

// tag::render[]
ReactDOM.render(
    <App/>,
    document.getElementById('react')
)
// end::render[]


```
<pre>

</pre>

# 에러가 계속 났었는데 원인들 ..
### 오타가 제일 큰 원인
1. props 넘겨줄 때 이름을 잘못 넘겨준 경우
2. 컴포넌트 이름을 잘못 입력한 경우
> 에러는 _currentElement를 찾을 수 없다.. 처럼 특이하게 나옴

### 문법 미숙
1. props로 인자를 가지는 함수를 넘겨줄 때 !!
> 리액트 16에서는 context로 함수를 넘기고 넘기고 하는 걸 막을 수 있다고 함.
2. 넘겨받은 함수를 사용할 때
3. 컨스트럭터에서 상태설정 this.state로 해야하는데 this.setState로 함;;

# 하면서 알게된 것들
1. static.built의 bundle.js는 리액트 -> js로 변환된 느낌이다. 이그노어로 뺌.
2. hot swap을 이용하기 위해 springloaded를 사용해보려 했지만, 이는 index.html같은 곳에만 적용되지, controller(api)수정이나 react수정은 적용 불가. 특히 react는 npm으로 한번더 돌려야하니까 그런 것 같음.
