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

{% highlight javascript %}
function App(){
  return (
    <div>
        <Clock />
        <Clock />
        <Clock />
    </div>
  );
}

  ReactDOM.render(
    <App />,
    document.getElementById('root')
  );
{% endhighlight %}
이 처럼하면 어느정도까지 프로젝트를 진행할 수 있다.

# Event
{% highlight javascript %}
class Toggle extends React.Component {
  constructor(props){
    super(props);
    this.state = {isToggleOn: true};
  }

  handleClick(){
		// console.log("handle clicked"); // 여기에 둘 경우 출력이 됨. 현재 setState가 제대로 동작하지 않기때문에.
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
    console.log("handle clicked"); // 여기에 둘 경우 출력이 안됨
  }

  render(){
    return (
      <button onClick={this.handleClick}> // 제대로 동작하지 않음.
        {this.state.isToggleOn ? 'ON' : "OFF"}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
{% endhighlight %}
정상적으로 동작하지 않음.
function도 this를 가지는데, 위처럼 되있는 경우 onClick이 불리면 this.handleClick에 할당되는
this는 button의 this가 된다.
하지만 우리가 원하는 것은 Toggle의 this이기 때문에 bind를 해주어야 한다.

{% highlight javascript %}
class Toggle extends React.Component {
  constructor(props){
    super(props);
    this.state = {isToggleOn: true};
  }

  handleClick(){
    console.log(this);
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render(){
    return (
      <button onClick={this.handleClick.bind(this)}>
        {this.state.isToggleOn ? 'ON' : "OFF"}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
{% endhighlight %}
bind(this)를 해주어야 handleClick의 this가 Toggle이 될 수 있음.

{% highlight javascript %}
class Toggle extends React.Component {
  constructor(props){
    super(props);
    this.state = {isToggleOn: true};

		this.handleClick = this.handleClick.bind(this);
  }

  handleClick(){
    console.log(this);
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render(){
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : "OFF"}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
{% endhighlight %}
생성자에서 bind를 해주면 onClick에서 bind를 해주지 않고 그냥 써줄 수 있다.

{% highlight javascript %}
handleClick = () => {
	console.log(this);
	this.setState(prevState => ({
		isToggleOn: !prevState.isToggleOn
	}));
}
{% endhighlight %}
또는 arrow를 통해 메소드를 정의하면 자동으로 bind가 된다

{% highlight javascript %}
<button onClick={() => this.handleClick()}>
  {this.state.isToggleOn ? 'ON' : "OFF"}
</button>
{% endhighlight %}
또는 arrow를 통해 메소드를 onclick을 정의하면 자동으로 bind가 된다

# Conditional Rendering Example
{% highlight javascript %}
function UserGreeting(props){
  return <h1>Welcome back!</h1>;
}
function GuestGreeting(props){
  return <h1>Please sign up.</h1>;
}
function Greeting(props){
  const isLoggedIn = props.isLoggedIn;
  return isLoggedIn ? <UserGreeting /> : <GuestGreeting />;
}

ReactDOM.render(
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
{% endhighlight %}


{% highlight javascript %}
function Mailbox(props){
	const unreadMessages = props.unreadMessages;
	return (
			<div>
				<h1>Hello!</h1>
				{unreadMessages.length > 0 &&
					<h2>
						You have {unreadMessages.length} unread messages.
					</h2>
				}
			</div>
	);
}

const messages = ['리액트', '여긴어디', '나는누구'];
ReactDOM.render(
		<Mailbox unreadMessages={messages} />,
		document.getElementById('root')
);
{% endhighlight %}
&&하면.. 앞에있는 것을 만족하면 뒤에있는 것을 실행함.

{% highlight javascript %}
function WarningBanner(props){
  if(!props.warn){
    return null;
  }
  return (
    <div className="warning">
      Warning
      </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state={showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render(){
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
 	<Page />,
 	document.getElementById('root')
);
{% endhighlight %}

{% highlight javascript %}

{% endhighlight %}
