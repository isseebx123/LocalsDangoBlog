---
layout: post
title:  "2. 리액트 특강 2일차(13:10 ~ 18:00)"
date:   2018-04-07 00:00:00
author: 이상현
categories: React
---

# Try React
[HTML을 통해 시범적으로 React를 하는 법](https://reactjs.org/docs/try-react.html)
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel">

      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('root')
      );

    </script>
    <!--
      Note: this page is a great way to try React but it's not suitable for production.
      It slowly compiles JSX with Babel in the browser and uses a large development build of React.

      To set up a production-ready React build environment, follow these instructions:
      * https://reactjs.org/docs/add-react-to-a-new-app.html
      * https://reactjs.org/docs/add-react-to-an-existing-app.html

      You can also use React without JSX, in which case you can remove Babel:
      * https://reactjs.org/docs/react-without-jsx.html
      * https://reactjs.org/docs/cdn-links.html
    -->
  </body>
</html>
```
# create-react app을 통해 구축
## CLI(Command Line Interface)
노드 개발을 하게되면 모든 명령을 터미널이나 프롬프트를 통해서 명령을 내리게 되는데
CLI도구를 command라인에서 사용하여 새로운 프로젝트를 생성한다.

## node
node를 설치하고 www.npmjs.com에 들어가본다.
검색해서 하나를 들어가보면 사람들이 쓰라고 올려놓은 것이 있다.
소개된 컴포넌트를 제공하고, 레포지터리도 올라와있음.

## create-react-app
cmd에서 npm install -g create-react-app으로 설치하고
create-react-app my-app으로 프로젝트를 생성한다.
