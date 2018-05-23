---
layout: post
title:  "React 16.3 context api"
date:   2018-05-23 15:00:00
author: 이상현
categories: React
---

# 먼저 프로젝트 세팅
## 필요한 모듈 추가
1. react 16.3.2
2.
## yarn과 npm의 차이?
1. npm에서 발생한 다양한 이슈들을 해결한 것이 yarn이라고 한다. [참고]("https://www.vobour.com/yarn-%EC%B2%98%EC%9D%8C-%EB%B3%B4%EB%8A%94-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%83%88-%ED%8C%A8%ED%82%A4%EC%A7%80-%EB%A7%A4%EB%8B%88%EC%A0%80-yarn-fir")
- yarn install --global yarn: Yarn을 글로벌으로 설치
- yarn self-update: Yarn의 새로운 최신 버전으로 업데이트
2. yarn install하면 package.json에 자동으로 추가되는데 npm은 그렇지 않다.
3. npm은 -S옵션을 주어야 dependencies에 추가된다.
4. 위 사항은 npm help install 명령어를 통해 확인하였다.
5. devDependencies 같은 것은 뭘까? dev server라는 의미와 비슷하게 개발시에만 사용한다는 것 일까?

# 진행
## 문법정리
1. ... syntax는 아래와 같은 역할을 한다.
```js
<Modal {...this.props} title='Modal heading' animation={false}>
<Modal a={this.props.a} b={this.props.b} title='Modal heading' animation={false}>
```
<pre>
위 2개는 같다.
</pre>
```js
this.setState(currentState => {
	return {
		...currentState,
		notifications: {
			...currentState.notifications,
			[id]: {
				...currentState.notifications[id],
				seen: true
			}
		}
	};
});
```
<pre>
	위와 같은 경우에는 기존 값을 그대로 가져가면서 seen만 true로 만든다.
</pre>
2. 폴더를 import하여 태그로 꺼내는 경우
```js
import App from "./Components/App";
ReactDOM.render(<App />, document.getElementById("root"));
```
<pre>
	App이 App.js가 아닌 폴더인 경우.. App폴더안에 index.js를 하나두고 다음과 같이 적는다.
	굳이 이렇게 해야할 필요성이 있나..?
</pre>
```js
import AppContainer from "./AppContainer";
export default AppContainer;
```
3. Object.keys를 사용하는 이유
<pre>
	state가 array로 되어있지 않아서 map을 사용할 수 없다.
	이 경우 Object.keys(store.notifications)를 하면 key값을 가지는 배열이 생성된다.
	배열을 .map해서 key를 이용하여 store.notifications[key]로 값을 가져온다.
</pre>
4. 배열에서 요소 삭제
```js
this._deleteNotification = id => {
	this.setState(currentState => {
		const newState = delete currentState.notifications[id];
		return newState;
	});
};
```
<pre>
	delete문으로 state에서 특정 요소를 제거한 다음 나머지 state를 newState로 받는 듯!
</pre>
## Context API
1. Provider
```js
<Store.Provider value={this.state}>
</Store.Provider>
```
2. Consumer
```js
<Store.Consumer>
	{/*함수만 들어가야 함*/}
	{store => (
		<Button seend={seen} onClick={store._changeMessage}>
		</Button>
	)}
</Store.Consumer>
```
3. Provider로 넘겨주고 싶은 함수는 state가 아닌 constructor에 정의해주어야 한다.

# 추가사항
1. react suspense
- 데이터가 들어오기까지 isloading해야되는 것을 하지않게 해준다.
