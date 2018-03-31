---
layout: post
title:  "1. 리액트 특강 1일차"
date:   2018-03-31 00:00:00
author: 이상현
categories: React
---

# e
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
	return `Hello,  ${name}`; // '이 아닌 `임을 주의
}
{% endhighlight %}

# 함수의 리턴
{% highlight javascript %}
const test = (a, b) => return a + b;
{% endhighlight %}
한 경우는 
