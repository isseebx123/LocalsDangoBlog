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
