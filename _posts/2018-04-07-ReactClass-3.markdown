---
layout: post
title:  "리액트 특강 2일차(10:00 ~ 12:00)"
date:   2018-04-07 00:00:00
author: 이상현
categories: React
---

# Lists
```js
const numbers = [1,2,3,4,5];
let listItems = [];

for(number in numbers) {
  listItems.push(<li>{number}</li>)
}

ReactDOM.render(
<ul>{listItems}</ul>,
  document.getElementById('root')
);
```
유니크 key를 넣어주지 않아서 console창에서 오류가 발생한다.

```js
const numbers = [1,2,3,4,5];
let listItems = [];

for(number in numbers) {
  listItems.push(<li>{number}</li>)
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) => <li>{number}</li>);
  return ( <ul>{listItems}</ul> );
}

ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
map형태로 바꾸어서 어떻게 리액트형식으로 바뀌어가는가를 살펴본다.

```js
const numbers = [1,2,3,4,5];
let listItems = [];

for(number in numbers) {
  listItems.push(<li>{number}</li>)
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) => <li key={number.toString()}>{number}</li>);
  return ( <ul>{listItems}</ul> );
}

ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
이런식으로 key를 주면 warning이 사라진다.

```js
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key = {post.id}>
        {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
      </div>
    )
}

const posts = [
  {id: 1, title: "안녕", content: "환영"},
  {id: 2, title: "잘가", content: "ㅂㅇ"},
  {id: 3, title: "ㅎㅇ", content: "ㅂㅂㅇ"}
]

ReactDOM.render(
  <Blog posts={posts}/>,
  document.getElementById('root')
);
```
블로그가 짜여진 형식을 알아볼 수 있다.
