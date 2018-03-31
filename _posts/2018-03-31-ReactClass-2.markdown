---
layout: post
title:  "1. 리액트 특강 1일차(15:00 ~ 18:00)"
date:   2018-03-31 00:00:00
author: 이상현
categories: React
---

# State
{% highlight javascript %}
function tick() {
	const element = (
			<div>
				<h1>Hello, world!</h1>
				<h2>It is {new Date().toLocaleTimeString()}.</h2>
			</div>
	);
	ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
{% endhighlight %}
하지만 위는 리액트를 잘못 사용한 예제임

{% highlight javascript %}
function Clock(props){
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
};

setInterval(tick, 1000);
{% endhighlight %}
이것도 리액트를 잘못 사용하는 예제임.

{% highlight javascript %}
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state = {date: new Date()};
  }
  tick() {
    this.setState({
      date: new Date()
    });
  }
  componentDidMount(){
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  componentWillUnMount() {
    clearInterval(this.timerID);
  }
  render(){ // React.Component에 있는 render를 override
    return(
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

  ReactDOM.render(
    <Clock />,
    document.getElementById('root')
  );
	{% endhighlight %}

# 이전 State값을 이용하여 State값을 바꾸는 경우
{% highlight javascript %}
this.setState({
	counter: this.state.counter + this.props.increment
	});
{% endhighlight %}
는 틀린예제임.

{% highlight javascript %}
this.setState((prevState, props) => ({
	counter: prevState.counter + props.increment
	}));
{% endhighlight %}
이 처럼 사용해야 함.

# Top-down data flow (위에서 아래로 데이터전송)
{% highlight javascript %}
function FormattedDate(props){
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}

class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state = {date: new Date()};
  }
  tick() {
    this.setState({
      date: new Date()
    });
  }
  componentDidMount(){
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  componentWillUnMount() {
    clearInterval(this.timerID);
  }
  render(){ // React.Component에 있는 render를 override
    return(
      <div>
        <h1>Hello, world!</h1>
        <FormattedDate date={this.state.date} />
      </div>
    );
  }
}

  ReactDOM.render(
    <Clock />,
    document.getElementById('root')
  );
	{% endhighlight %}
리액트에서 주로 사용하는 방법으로 이렇게 하지않고 위로갔다 아래로 갔다가 하면 복잡해진다.
