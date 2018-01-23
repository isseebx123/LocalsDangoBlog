---
layout: post
title:  "3회차. 리액트 JS 유튜브 강의 수강 (1/2) / 결과"
date:   2018-01-15 22:02:00
author: 이상현
categories: 로컬(모각코)
---

# 3회차. 리액트 JS 유튜브 강의 수강 (1/2) / 결과

## 결과

기존에는 개인적으로 구상하고 있는 프로젝트의 진행을 수행하려고 했지만, 모각코 모임이전에 요구사항 문서와 UI명세서를 작성해보니 이를 구현하려면 아직 리액트에 대해 더 자세히 알아야할 필요성을 느꼈다. 그래서 유튜브의 리액트 강의를 들으면서 기본 구조를 좀 더 공부하고자 한다.

---

## 1-2편. React.js 소개

[리액트의 이해를 돕는 영상](https://www.youtube.com/watch?v=BYbgopx44vo)

이전 노마드 코더의 강의를 들으면서 들었던 말 중 신기했던 것이 리액트는 데이터가 UI를 바꾼다는 것이었다. 이 말은 즉슨 데이터가 변하면 우리가 다시 확인하면서 그것의 변화를 알아야 되는 것이 아니라, 변한 데이터가 알아서 UI를 바꾸는 것이다. 당시에는 아 그런가 싶으면서도 잘 이해가 되지 않았는데, 이 영상을 보고 나서는 리액트가 가지는 Virtual DOM이 어떤 역할을 하는지 확실히 알 수 있게 되었다.

## 1-3편. 리액트의 장점과 단점

### 장점
1. 뛰어난 Garbage Collection
2. 메모리 관리
3. 성능
4. 서버 사이드와 클라이언트 사이드 렌더링을 모두 지원
5. 다른 프레임워크나 라이브러리와 혼용가능

### 단점
1. 리액트는 뷰(보여지는 부분)만 관리하므로 데이터모델링, 라우팅, ajax 등의 기능은 없다. 하지만 다른 라이브러리를 사용하여 구현가능하다.
2. IE8 이하는 지원 불가. 하지만 리액트 버전 14이하를 사용하고 이를 지원해주는 폴리필을 사용하면 된다.

## 2-1편. Codepen 설정, ES6 클래스

{% highlight javascript %}
class Polygon {
  constructor(hegiht, width) {
    this.height = height;
    this.width = width;
  }
}
{% endhighlight %}
<pre>자바스크립트 클래스안에서는 메소드만 정의가능하고, 변수를 사용하고 싶다면 위처럼 생성자를 통해 initialize를 해야한다.</pre>

{% highlight javascript %}
var p = new Polygon();
class Polygon {}
{% endhighlight %}
<pre>위와 같이 클래스 선언하기도 전에 클래스를 사용하면 레퍼런스 에러가 발생한다.</pre>

{% highlight javascript %}
class Polygon {
  constructor(hegiht, width) {
    this.height = height;
    this.width = width;
  }

  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.sqrt(dx*dx + dy*dy);
  }
}
{% endhighlight %}
<pre>위와 같이 메소드를 정적으로 선언할 수 있다. 이렇게 하면 클래스를 생성하지 않고도 메소드를 사용할 수 있다는 차이점이 있다.</pre>

{% highlight javascript %}
class Codelab extends React.Component {

}
{% endhighlight %}
<pre>extends 키워드를 통해 상속을 할 수 있다. 리액트에서는 컴포넌트를 만들때 React.Component를 상속한다. 상속을 했을때 super키워드를 통해 상위 클래스에서 정의된 것에 접근할 수 있다.</pre>

## 2-2편. JSX의 특징

### 컴포넌트 생성하고 렌더링해보기
{% highlight javascript %}
class Codelab extends React.Component {
  render() {

  }
}
{% endhighlight %}
<pre>모든 리액트 컴포넌트는 렌더 메소드를 가지는데, render 메소드는 컴포넌트가 어떻게 생길지 정의를 해준다.</pre>

{% highlight javascript %}
var a = (
    <div>
      Welcome to <b>React</b>
    </div>
  );
{% endhighlight %}
<pre>JSX에서는 위와같이 xml과 비슷하게 html코드를 작성할 수 있다.</pre>

html코드
{% highlight javascript %}
<div id="root"></div>
{% endhighlight %}

JS코드
{% highlight javascript %}
class Codelab extends React.Component {
  render() {
    return (
      <div>Codelab Text</div>
      );
  }
}
class App extends React.Component {
  render() {
    return (
        <Codelab/>
      );
  }
}
ReactDOM.render(<App/>, document.getElementById('root'));
{% endhighlight %}
<pre>페이지에 렌더링을 통해 App 컴포넌트와 Codelab 컴포넌트가 생성된 것을 확인할 수 있었다.</pre>

### JSX 유의사항
{% highlight javascript %}
render() {
  return (
      <div>
        <h1>Hi</h1>
        <h1>Bye</h1>
      </div>
    );
}
{% endhighlight %}
<pre>컴포넌트에서 여러 element를 렌더링 할때, 하나의 container element가 이를 모두 포함하는 형태가 되어야 한다.</pre>

{% highlight javascript %}
render() {
  let text = "hello";
  return (
      <div>{text}</div>
    );
}
{% endhighlight %}
<pre>JSX안에서 JavaScript를 표현하는 방법은 {}로 wrapping을 하면 된다. 이때 let이라는 키워드는
  ES6의 새로운 문법이다. var과 비슷하게 변수를 선언하는데 사용하지만, scope가 함수단위인데 비해,
  let은 블록범위 내에서만 가능하게 하여 스코프문제를 해결해준다. 또 한번 선언이 되었으면 다시 선언될
  수 없다. 리액트 js에서는 let을 사용하는 것이 관습이므로 이를 사용하도록 한다.</pre>

{% highlight javascript %}
class Codelab extends React.Component {
  render() {
    let text = "hi";
    let styleV = {
      backgroundColor: "aqua"
    };

    return (
        <div style={styleV}>{text}</div>
      );
  }
}
{% endhighlight %}
<pre>JSX안에서 style을 설정할때는 string형식을 사용하지 않고 key가 CamelCase인 객체가 사용된다. (ex) background-Color X)</pre>

{% highlight javascript %}
<div>
{ /*it's comment*/ }
</div>
{% endhighlight %}
<pre>JSX에서 주석을 작성할 때는 { /*...*/ }과 같이 작성을 한다.</pre>

## 2-3편. props

<pre>props는 컴포넌트 내부의 Immutable Data를 의미한다.
JSX 내부에 { this.props.propsName }으로 사용이 가능하다.
컴포넌트를 사용할 때, <> 괄호 안에 propsName="value"
this.props.children은 기본적으로 갖고있는 props로서,</pre>
{% highlight javascript %}<Cpnt>여기에 있는 값이 들어간다.</Cpnt>{% endhighlight %}
{% highlight javascript %}
class Codelab extends React.Component {
  render() {
    return (
        <div>
          <h1>Hello {this.props.name}</h1>
          <div>{this.props.children}</div>
        </div>
      );
  }
}
class App extends React.Component {
  render() {
    return (
        <Codelab name="velo">여기에 있는 값이 들어간다.</Codelab>
      );
  }
}
ReactDOM.render(<App/>, document.getElementById('root'));
{% endhighlight %}
<pre>이렇게 하면 h1과 div에 "velo", "여기에 있는.."이 들어간다.</pre>

{% highlight javascript %}
class App extends React.Component {
  render() {
    return (
        <Codelab name={this.props.name}>{this.props.children}</Codelab>
      );
  }
}
ReactDOM.render(<App = name="velo">여기에 있는 값이 들어간다.</App>, document.getElementById('root'));
{% endhighlight %}
<pre>또 이전 것의 연속에서 App과 ReactDom.render부분을 이렇게 바꾸면, 똑같이 동작한다.</pre>

{% highlight javascript %}
class App extends React.Component {
  render() {
    return (
        <div>{this.props.value}</div>
      );
  }
}
App.defaultProps = {
  value: 0
};
{% endhighlight %}
<pre>props의 기본값설정은 클래스 정의 직후에 위와같이 설정하면 된다.</pre>

{% highlight javascript %}
App.propTypes = {
  value: React.PropTypes.string,
  secondValue: React.PropTypes.number,
  thirdValue: React.PropTypes.any.isRequired
};
{% endhighlight %}
<pre>위와 같이 Type에 대한 검증도 가능하다.</pre>

## 2-4편. state

<pre>유동적인 데이터를 보여줄 때 사용하며, 초기값 설정이 필수이다. 이는 생성자에서 this.state = {}으로 설정한다.
  JSX 내부에서는 {this.state.stateName}으로 사용한다. 값을 수정할 때에는 this.setState({...})로 변경하며,
  렌더링 된 다음에는 this.state=를 절대 사용하면 않된다. this.setState는 state를 변경하면서 안전한 방법으로
  리렌더링을 진행한다.</pre>

  1분 46초까지 공부하였음.
