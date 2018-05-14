---
layout: post
title:  "SpringBoot 7일차 (react)"
date:   2018-05-09 21:00:00
author: 이상현
categories: Naver_Hackday_Ready
---

# Form
<pre>
Form으로 input태그들을 감싸고 타입이 submit인 input을 이용한다.
submit을 했을 때, client(...)를 통한 API요청을 하며, PathVariable을 이용하도록
인풋태그의 값들을 UrlPath에 적절히 섞어서 넣어준다.
인풋태그의 값은 ref옵션으로 받아오며, 다음과 같은 예시이다.
</pre>
```js
ref={(input) => this.bno = input}
```
```js
handleSubmit(event) {
    client({
        method: 'PUT', path: `/insertBoard/${this.bno.value}/${this.title.value}/${this.content.value}`,
    }).done(response => {
        this.props._excuteSelect();
        console.log("success insert");
    });

    event.preventDefault();
}
```

# PathVariable
<pre>
  넘겨준 인자들을 path에서 추출하여 사용한다.
</pre>

# REST
<pre>
  REST는 path에 insertBoard와 같이 sql문 등이 오는 것이 아니고, 다루어질 데이터(테이블 등) ex) Board
  를 주고 뒤에 인자를 준 후, CRUD중 내가 하려고하는 작업에 맞는 http Method로 요청을 하면
  해당 작업을 찾아서 하는 형식이다.
</pre>
