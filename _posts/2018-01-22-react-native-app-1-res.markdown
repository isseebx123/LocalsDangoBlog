---
layout: post
title:  "3회차. 리액트 JS 유튜브 강의 수강 (1/2) / 결과"
date:   2018-01-15 22:02:00
author: 이상현
categories: 로컬(모각코)
---

# 3회차. 리액트 JS 유튜브 강의 수강 (1/2) / 결과

## 개요

기존에는 개인적으로 구상하고 있는 프로젝트의 진행을 수행하려고 했지만, 모각코 모임이전에 요구사항 문서와 UI명세서를 작성해보니 이를 구현하려면 아직 리액트에 대해 더 자세히 알아야할 필요성을 느꼈다. 그래서 유튜브의 리액트 강의를 들으면서 기본 구조를 좀 더 공부하고자 한다.

---

## 1-2편. React.js 소개

[리액트의 이해를 돕는 영상](https://www.youtube.com/watch?v=BYbgopx44vo)

이전 노마드 코더의 강의를 들으면서 들었던 말 중 신기했던 것이 리액트는 데이터가 UI를 바꾼다는 것이었다. 이 말은 즉슨 데이터가 변하면 우리가 다시 확인하면서 그것의 변화를 알아야 되는 것이 아니라, 변한 데이터가 알아서 UI를 바꾸는 것이다. 당시에는 아 그런가 싶으면서도 잘 이해가 되지 않았는데, 이 영상을 보고 나서는 리액트가 가지는 Virtual DOM이 어떤 역할을 하는지 확실히 알 수 있게 되었다.

## 1-3편. 리액트의 장점과 단점

### 장점
1. 뛰어난 Garbage Collection
2. 메모리 관리
3. 성능
4. 서버 사이드와 클라이언트 사이드 렌더링을 모두 지원
5. 다른 프레임워크나 라이브러리와 혼용가능

### 단점
1. 리액트는 뷰(보여지는 부분)만 관리하므로 데이터모델링, 라우팅, ajax 등의 기능은 없다. 하지만 다른 라이브러리를 사용하여 구현가능하다.
2. IE8 이하는 지원 불가. 하지만 리액트 버전 14이하를 사용하고 이를 지원해주는 폴리필을 사용하면 된다.

## 2-1편. Codepen 설정, ES6 클래스

### 클래스 선언
<pre>
class Polygon {
  constructor(hegiht, width) {
    this.height = height;
    this.width = width;
  }
}
자바스크립트 클래스안에서는 메소드만 정의가능하고, 변수를 사용하고 싶다면 위처럼 생성자를 통해 initialize를 해야한다.

var p = new Polygon();
class Polygon {}
위와 같이 클래스 선언하기도 전에 클래스를 사용하면 레퍼런스 에러가 발생한다.

class Polygon {
  constructor(hegiht, width) {
    this.height = height;
    this.width = width;
  }

  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.sqrt(dx*dx + dy*dy);
  }
}
위와 같이 메소드를 정적으로 선언할 수 있다. 이렇게 하면 클래스를 생성하지 않고도 메소드를 사용할 수 있다는 차이점이 있다.

class Codelab extends React.Component {

}
extends 키워드를 통해 상속을 할 수 있다. 리액트에서는 컴포넌트를 만들때 React.Component를 상속한다. 상속을 했을때 super키워드를 통해 상위 클래스에서 정의된 것에 접근할 수 있다.
</pre>
