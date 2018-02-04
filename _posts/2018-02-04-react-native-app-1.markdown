---
layout: post
title:  "리액트 JS 유튜브 강의 수강"
date:   2018-02-41 11:04:00
author: 이상현
categories: 개인공부
---

# 리액트 JS 유튜브 강의 수강

## 2-5. 컴포넌트 매핑

{% highlight javascript %}
arr.map(callback, [thisArg])
{% endhighlight %}
{% highlight javascript %}
var numbers == [1,2,3,4,5];
var processed = numbers.map(function(num) {
    return num * num;
  });
{% endhighlight %}
<pre>
자바스크립트 map 함수.
callback: 새로운 배열의 요소를 생성하는 함수로써, 다음 세가지 인수를 가진다.
  currentValue 현재 처리되고 있는 요소
  index 현재 처리되고 있는 요소의 index 값
  array 메소드가 불려진 배열
thisArg(선택항목) callback 함수 내부에서 사용할 this값을 설정
</pre>

{% highlight javascript %}
let one = a => console.log(a);

let two = (a, b) => console.log(a,b);

let three = (c, d) => {
  console.log(c);
  console.log(d);
}

let four = () => {
  console.log('no params');
}
{% endhighlight %}
<pre>
arrow function 사용법.
파라미터가 하나일때, 2개일때, 여러줄일때, 4개일때.
</pre>


{% highlight javascript %}
class ContactInfo extends React.Component {
  render(){
    return (
        <div>{this.props.contact.name} {this.props.contact.phone}</div>
      );
  }
}

class Contact extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      contactData: [
        {name: 'a', phone: '010-0000-0000'},
        {name: 'b', phone: '010-0000-0001'},
        {name: 'c', phone: '010-0000-0002'},
        {name: 'd', phone: '010-0000-0003'}
      ]
    }
  }

  render(){
    const mapToComponent = (data) => {
      return data.map((contact, i) => {
        return (<ContactInfo contact={contact} key={i}/>)
      });
    };

    return (
      <div>
        {mapToComponent(this.state.contactData)}
      </div>
    );
  }
}

class App extends React.Component {
  render() {
    return (
        <Contact/>
      );
  };
}
{% endhighlight %}
<pre>
</pre>
