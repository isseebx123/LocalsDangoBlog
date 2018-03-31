---
layout: post
title:  "1. 리액트 특강 1일차"
date:   2018-03-31 00:00:00
author: 이상현
categories: React
---

# ES5 => ES6
ES5
{% highlight javascript %}
function test(a){
	return function (b) {
		return a + b;
	}
}
{% endhighlight %}

ES6
{% highlight javascript %}
const test = a => b => {
	return a + b;
}
{% endhighlight %}

{% highlight javascript %}
const test = (a, b) => return a + b;
const test = (a, b) => (c, d) => return a + b + c + d;
{% endhighlight %}

{% highlight javascript %}
const test = name => {
	return `Hello,  ${name}`; // '이 아닌 `임을 주의.
}
{% endhighlight %}

# 함수의 리턴
{% highlight javascript %}
const test = (a, b) => return a + b;
{% endhighlight %}

{% highlight javascript %}
const formatName = userObject => {
  // const firstName = userObject.firstName;
  // const lastName = userObject.lastName;
  // 을 아래로 변경
  // const { firstName, lastName } = userObject;

  // 아래도 가능
  return `${userObject.firstName} ${userObject.lastName}`;
}

// 이것도 가능
//const formatName = ({ firstName, lastName }) => {
//  return `${firstname} ${lastName}`;
//}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);


/*
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}
*/

ReactDOM.render(element, document.getElementById('root'));
{% endhighlight %}

# 컴포넌트 사용
{% highlight javascript %}
function Test2(props){
  return <h1>Welcome to {props.country}</h1>
}

const element = (
  <div>
    <h1>
      Hello, {formatName(user)}!, {test()}
    </h1>
    <h2>cc</h2>
    <Test2 country="test"/>
  </div>  
);
{% endhighlight %}

# 재사용이 가능한 컴포넌트
{% highlight javascript %}
function WelcomeFunc(props){
  return <h1>Welcome to {props.country}</h1>
}
ReactDOM.render(
  <div>
    <WelcomeFunc country="Korea"/>
    <WelcomeFunc country="America"/>
    {element}
  </div>,
  document.getElementById('root')
);
{% endhighlight %}

# 태그처럼 사용할 수 있는 방법
{% highlight javascript %}
const Abc = () => {
  return 'abc';
}
// 확실히 가능한지 다시확인하기
const Bbc = () => {
  return <h1>qwe</h1>;
}

<Abc/> <Bbc/>가능
{% endhighlight %}
<b>첫문자가 대문자이고 리턴문이 있으면 가능하다.</b>
