---
layout: post
title:  "4회차. 리액트 JS 유튜브 강의 수강 (2/3) / 결과"
date:   2018-02-01 21:42:00
author: 이상현
categories: 로컬(모각코)
---

# 4회차. 리액트 JS 유튜브 강의 수강 (2/3) / 결과

## 결과

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
  리렌더링을 진행한다. [기본 코드펜 새 React 프로젝트 코드](http://bit.ly/ReactCodePen).</pre>

{% highlight javascript %}
  class Counter extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      value: 0
    };
  }

  handleClick(){
    this.setState({
      value: this.state.value + 1
    });
  }
  render(){
    return (
      <div>
        <h2>{this.state.value}</h2>
        <button onClick={this.handleClick.bind(this)}>Press Me</button>
      </div>
    );
  }
};

class App extends React.Component {
  render() {
    return (
      <Counter/>
    );
  }
};

ReactDOM.render(
  <App></App>,
  document.getElementById("root")
);
{% endhighlight %}
<pre>constructor에서 super(props)를 해줘야 상위인 React.Component의 생성자를 실행하여
props 등 다른 것들이 제대로 동작이 가능해진다.</pre>

{% highlight javascript %}
<button onClick={this.handleClick}>Press Me</button>
{% endhighlight %}
<pre>만약 위 처럼만 작성한 경우에는 제대로 동작하지 않는다. 왜냐하면 자바스크립트 컴포넌트에서
임의의 메소드를 호출하면은 메소드는 this가 무엇인지 모르기 때문이다. 그래서 .bind(this)를 해주어야한다.</pre>

{% highlight javascript %}
class Counter extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      value: 0
    };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick(){
    this.setState({
      value: this.state.value + 1
    });
  }
  render(){
    return (
      <div>
        <h2>{this.state.value}</h2>
        <button onClick={this.handleClick}>Press Me</button>
      </div>
    );
  }
};

class App extends React.Component {
  render() {
    return (
      <Counter/>
    );
  }
};

ReactDOM.render(
  <App></App>,
  document.getElementById("root")
);
{% endhighlight %}
<pre>컴포넌트에서 onClick에 처리해주는 것보다 생성자에서 bind설정을 해주는 것이 더 보기 좋다.</pre>

{% highlight javascript %}
        <button onClick={this.handleClick()}>Press Me</button>
{% endhighlight %}
<pre>그리고 this.handleClick()와 같이 괄호를 넣으면 안된다. 괄호를 넣으면 한번 실행되고,
state가 바뀌고, 다시 렌더링될 때 이를 또 호출하게 되어 무한으로 계속 호출하는 에러가 발생한다.</pre>

{% highlight javascript %}
handleClick(){
  this.state.value = this.state.value + 1;
  this.forceUpdate();
}
{% endhighlight %}
<pre>
만약 setState를 안주고 강제로 하는 경우 forceUpdate를 하면 업데이트가 되기는한다.
그런데 좋지도 않고, 이것을 써야하는 경우도 거의 오지않을 것이니 쓰지 마라!
</pre>
