---
layout: post
title:  "Apache thrift - 알아보기"
date:   2018-06-29 14:05:00
author: 이상현
categories: Apache thrif
---

# 간단하게 먼저 알아보자 !
## Apache thrift는 무엇인가?
<p>
[RPC - REST 관련 슬라이드쉐어]("https://www.slideshare.net/WonchangSong1/rpc-restsimpleintro") <br/>
클라이언트가 서버와 통신하기 위해서 간단한 코드 몇줄로도 가능하지만, 네트워크 트래픽 문제나 기타 여러가지 에러사항을 고려해보았을 때 개발자가 신경써주어야할 부분이 너무많다. 그래서 등장한 것이 RPC이며, RPC(Remote Procedure Call)는 서버에 있는 프로그램 정보를 알고있는(?) IDL을 이용하여 여러가지 기타사항들을 신경써준다. 즉 개발자는 network통신과 관련된 작업은 신경쓰지 않아도 되고, 원격지의 프로그램을 로컬의 프로그램처럼 사용할 수 있다.
</p>

## 제공되는 언어들은?
<p>
20개 이상의 언어가 제공된다. 예제는 대부분 c, php이며, npm패키지에 아파치공식에서 올린 [패키지]("https://www.npmjs.com/package/thrift")가 있는 모양이다.
1. c++
2. java
3. php
4. javascript
...
</p>

## thrift npm 패키지
<p>
나중에 천천히 읽어보자. <br/>
[라이센스]("http://www.apache.org/licenses/LICENSE-2.0") <br/>
[공식사이트 튜토리얼]("https://thrift.apache.org/lib/nodejs") <br/>
[npm 패키지]("https://www.npmjs.com/package/thrift") <br/>
</p>

# thrift 서버 해보기
## cygwin 설치
<p>
컴퓨터별로 환경이 다르기 때문에 직접 빌드해서 만들어진 exe를 쓰라고 하는 것인가? <br/>
package에서 devel의 make 패키지를 설치해야 make 명령어를 사용할 수 있다.
</p>

## windows에서의 공식사이트 설명
<p>
1. thrift 컴파일러 exe파일을 직접 빌드해서 만들수도 있고(visual studio 또는 cygwin)
2. 미리 만들어진 thrift exe파일을 쓸 수도 있다.
미리 만들어진 것을 쓰면 내가 필요한 것(사용하는 언어)외에도 지원을 하느라 더 무겁거나 상세한 옵션설정이 안되어있을 수 있는 단점이 있지만, 현재는 옵션이 무엇이 있는지도 모를 뿐더러 빌드환경구성에도 시간이 걸리므로 미리 만들어진 것을 쓴다.
</p>

<hr/>

<h2>
Prebuilt Thrift compiler
</h2>
<p>
The windows Thrift compiler is available as a prebuilt exe available here. Note that there is no installation tool, rather this EXE file is already the Thrift compiler utility. Download the file and put it into some suitable location of your choice.
<br/><br/>
Now pick one of the "Build and install target libraries" below to continue.
</p>

## .thrift 작성
<p>[참고블로그]("http://theeye.pe.kr/archives/2063")</p>

```c
namespace js tutorial.arithmetic.gen  // define namespace for java code
// namespace java tutorial.arithmetic.gen  // define namespace for java code

typedef i64 long
typedef i32 int
service ArithmeticService {  // defines simple arithmetic service
  long add(1:int num1, 2:int num2),
  long multiply(1:int num1, 2:int num2),
}
```
<p>
먼저 js로 해보기 위해 위 java namespace는 주석처리를 하였다. 이 tutorial.thrift파일을 만들고 다운로드 받은 exe 컴파일러로 컴파일을 하면 파일이 2개 튀어나온다. <br/>
<b>공식사이트의 튜토리얼 파일이 깨져서 원본 코드를 알 수가 없었다. 참고블로그에서는 add와 multiply만 하려고하는 것 같다.</b>
</p>

```sh
./thrift -r --gen js tutorial.thrift
```
<img src="{{ site.baseurl }}/assets/postImages/20180629/generated.png">

## 서버만들기
<p>
[자바스크립트 튜토리얼]("https://thrift.apache.org/tutorial/js")<br/>
[자바 튜토리얼]("https://thrift.apache.org/tutorial/java")<br/>
[노드 튜토리얼]("https://thrift.apache.org/tutorial/nodejs")<br/>
자바스크립트 튜토리얼에서는 Java와 node.js튜토리얼에 서버만드는 것을 올려놨으니 그것을 보고 서버를 미리 만들어라고 한다. 자바스크립트 튜토리얼에는 단순히 client 코드밖에 없다. <br/>
여기서 node.js를 통해 서버를 만들어보기로 하자.
</p>

</hr>

```js
var thrift = require("thrift");
var Calculator = require("./gen-nodejs/Calculator");
var ttypes = require("./gen-nodejs/tutorial_types");
var SharedStruct = require("./gen-nodejs/shared_types").SharedStruct;

var data = {};

var server = thrift.createServer(Calculator, {
  ping: function(result) {
    console.log("ping()");
    result(null);
  },

  add: function(n1, n2, result) {
    console.log("add(", n1, ",", n2, ")");
    result(null, n1 + n2);
  },

  calculate: function(logid, work, result) {
    console.log("calculate(", logid, ",", work, ")");

    var val = 0;
    if (work.op == ttypes.Operation.ADD) {
      val = work.num1 + work.num2;
    } else if (work.op === ttypes.Operation.SUBTRACT) {
      val = work.num1 - work.num2;
    } else if (work.op === ttypes.Operation.MULTIPLY) {
      val = work.num1 * work.num2;
    } else if (work.op === ttypes.Operation.DIVIDE) {
      if (work.num2 === 0) {
        var x = new ttypes.InvalidOperation();
        x.whatOp = work.op;
        x.why = 'Cannot divide by 0';
        result(x);
        return;
      }
      val = work.num1 / work.num2;
    } else {
      var x = new ttypes.InvalidOperation();
      x.whatOp = work.op;
      x.why = 'Invalid operation';
      result(x);
      return;
    }

    var entry = new SharedStruct();
    entry.key = logid;
    entry.value = ""+val;
    data[logid] = entry;

    result(null, val);
  },

  getStruct: function(key, result) {
    console.log("getStruct(", key, ")");
    result(null, data[key]);
  },

  zip: function() {
    console.log("zip()");
    result(null);
  }

});

server.listen(9090);
```
<p>node.js server의 tutorial 코드이다.</p>

```js
var thrift = require("thrift");
var Calculator = require("./gen-nodejs/Calculator");
var ttypes = require("./gen-nodejs/tutorial_types");
var SharedStruct = require("./gen-nodejs/shared_types").SharedStruct;
```
<p>
gen-nodejs가 컴파일로 만들어진 파일을 말하는 듯 하다. 구조에 맞게 아까 파일을 넣어주었다. 대신 ArithmeticService와 tutorial_types만 가지고 있는데 Calculator, shared_types를 요구해서 이부분에 대해서는 수정이 필요할 듯 하다. <br/>
또 첫줄에서 thrift를 요구하므로 npm install i thrift로 설치를 해주어야 할 것이다.
</p>

<img src="{{ site.baseurl }}/assets/postImages/20180629/node.png">

### 튜토리얼 코드 수정
```js
if (typeof tutorial === 'undefined') {
  tutorial = {};
}
if (typeof tutorial.arithmetic === 'undefined') {
  tutorial.arithmetic = {};
}
if (typeof tutorial.arithmetic.gen === 'undefined') {
  tutorial.arithmetic.gen = {};
}
```
<p>먼저 tutorial_types를 살펴보았다. thrift에서 namespace로 설정한 것에 의미를 부여하는 것 같은데 크게 신경쓸 부분은 아닌 것 같다.</p>

```js
var thrift = require("thrift");
var ArithmeticService = require("./gen-nodejs/ArithmeticService");
var ttypes = require("./gen-nodejs/tutorial_types");

var data = {};

var server = thrift.createServer(ArithmeticService, {
  add: function(n1, n2, result) {
    console.log("add(", n1, ",", n2, ")");
    result(null, n1 + n2);
  },
});

server.listen(9090);
```
<p>
add만 가능하도록 server를 수정하였다. 이후 backend폴더에서 npm init -y로 초기화를 하고 npm install i thrift로 설치하였다. 만약 init을 안해주면 노드모듈이 바깥쪽에 설치된다.
</p>

### referenceError
<p>
namespace의 첫번째였던 tutorial이 defined되어있지 않다는 황당한 에러가 나왔는데, stack overflow에 찾아보니 <br/>
nodejs로 할때는 thrift 컴파일을 할때 단순히 js flag로 주면 안된다고 한다. <br/>
nodejs일때는 다음과 같다.
</p>
```sh
thrift -r --gen js:node tutorial.thrift
```

### 서버 띄우기 성공
<img src="{{ site.baseurl }}/assets/postImages/20180629/server.png">
<p>
단일 명령으로는 요청-응답을 확인해볼수 없는가? ex) curl..
</p>

## 클라이언트 만들기
<p>클라이언트도 역시 add만 남기고 일단은 주석처리한다.</p>

```js
var thrift = require('thrift');
var ArithmeticService = require('./gen-nodejs/ArithmeticService');
var ttypes = require('./gen-nodejs/tutorial_types');
const assert = require('assert');

var transport = thrift.TBufferedTransport;
var protocol = thrift.TBinaryProtocol;

var connection = thrift.createConnection("localhost", 9090, {
  transport : transport,
  protocol : protocol
});

connection.on('error', function(err) {
  assert(false, err);
});

// Create a ArithmeticService client with the connection
var client = thrift.createClient(ArithmeticService, connection);

client.add(1,1, function(err, response) {
  console.log("1+1=" + response);
});
```
<p>그러고 index html의 body에 script를 삽입해서 시도를 해본다.</p>

```js
  <script src="../src/client.js"></script>
```
<img src="{{ site.baseurl }}/assets/postImages/20180629/error.png">
<p>
  스크립트를 강제로 넣었더니 require을 인식하지 못한다. react에서 불러오는 방식으로 해본다.
</p>

```js
import React, {Component} from 'react';
import thrift from 'thrift';
import assert from 'assert';
const ArithmeticService = require('./gen-nodejs/ArithmeticService');

var transport = thrift.TBufferedTransport;
var protocol = thrift.TBinaryProtocol;

const Client = () => {
  var connection = thrift.createConnection("localhost", 9090, {
    transport: transport,
    protocol: protocol
  });

  connection.on('error', function (err) {
    assert(false, err);
  });

  var client = thrift.createClient(ArithmeticService, connection);

  client.add(1, 1, function (err, response) {
    console.log("1+1=" + response);
    return (<span>resopnse</span>);
  });
}

export default Client;
```
<p></p>
