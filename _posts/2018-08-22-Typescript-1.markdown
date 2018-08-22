---
layout: post
title:  "Typescript with React: Hello world!"
date:   2018-08-22 14:24:00
author: 이상현
categories: Typescript
---

# Typescript란?
<pre>
  구글에 찾아보면 다양한 얘기가 나오지만 컴파일언어가 아닌 javascript는 타입에러를 런타임에 가서 발견한다.
  에러가 발생한 다음에 발견이 되므로, 이를 해결하기 위해 자바와 같은 컴파일언어와 같이 타입을 제약하고,
  컴파일타임에 에러를 발견하여 개발경험을 증진시키는데 목적이 있다.
</pre>

# create-react-App
## Typescript는 React에 한정적인 용어일까?
```sh
  create-react-app my-app --scripts-version=react-scripts-ts
```
<pre>
  아니다!! 정확히는 Angular의 기본이 되는 언어이며, 다른 언어에서 같이 사용될 수 있는 문법이라고 생각하자.
  위에서와 같이 명령어를 실행하면 typescript가 적용된 react 프로젝트가 생성된다.
  기본적인 확장자는 .tsx이다. 방금 강의를 보며 JSX가 JavaScript Xml이라는 뜻이라는 데 상식으로 알아두자..
</pre>

## Hello world - class
```js
export interface AppProps {
  name: string;
}

interface AppState {
  age: number;
}

class App extends React.Component<AppProps, AppState> {
  private _name : string = 'hi';
  constructor(props: AppProps) {
    super(props);
    this.state = {
      age: 35,
    };
  }
  render() {
    _name;
    return(...);
  }
}
```
<pre>
  위와 같이 prop과 state의 타입을 정의해줄 수 있다. 단순히 <>안에 넣어줄 수도 있지만, 프로젝트가 커지면
  관리가 어려우므로 보통 따로 interface로 빼서 한다고 한다. 보통 export 형식으로 만들어서
  다른 곳에서도 사용하고, 더 확장해서 사용할 수 있도록 한다.
</pre>

## ts에서 defaultProps
```js
class App extends React.Component<AppProps, AppState> {
  static defaultProps = {
    company: 'Studio XID',
  }
  constructor(props: AppProps) {
    super(props);
  }
  render() {
    return(...);
  }
}
```
<pre>
  static defaultProps로 지정가능하다.
</pre>

## 그렇다면 정석적인 사용방법은?
<pre>
  언제나 그렇듯 React는 나온지 얼마 되지않은 언어고 자주 바뀌고 있으므로, 정해진 양식따위가 없다.
  좋은건지 나쁜건지는 모르겠지만, 깃허브의 프로젝트들을 살펴봐도 사용방식이 각각 다르다.
  만약 내가 사용한다면 한 파일에 모두 몰아놓고 import해서 사용하지 않을 까 싶다.
</pre>

## Hello world - stateless Component
```js
const StatelessComponent: React.SFC<AppProps> = (props) => {
  return (...);
}

StatelessComponent.defaultProps = {
  company: 'Home',
};
```
<pre>
  functional component도 ts를 적용하려면 React.SFC라는 특별한 것을 써야한다.
  SFC = Stateless Functional Component 이다.
</pre>

# Typescript를 써야할까?
<pre>
  typescript만이 답은 아니지만 대세는 맞다.
  타입을 체크하기 위한 것으로는 라이브러리로 제공되는 PropTypes,
  Facebook에서 제공하는 Flow,
  그리고 Typescript가 있다.
  자기 입맛에 맞게 사용하면 되지만, 그래도 Typescript가 제일 강력하다고 생각된다.
</pre>
