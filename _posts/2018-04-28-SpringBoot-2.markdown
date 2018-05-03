---
layout: post
title:  "SpringBoot 3일차 (mybatis)"
date:   2018-04-27 13:00:00
author: 이상현
categories: Naver_Hackday_Ready
---

# mybatis 튜토리얼 사이트와 블로그를 참조!
- [참조 블로그]("http://private.tistory.com/52")
- [튜토리얼]("http://www.mybatis.org/spring/ko/getting-started.html")
- [참조 블로그2]("https://brunch.co.kr/@ourlove/66")
- [참조 블로그3]("http://addio3305.tistory.com/62")

## 먼저 에러 처리부터..
> 'org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration': Unsatisfied dependency ..
<pre>
  자세히 보니 jpa 뭐시기라 되어있었네.. jpa안쓰므로 디펜던시 삭제하니 이 에러는 사라졌음. 에러 자세히 보자 ! ㅠ
</pre>

> org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'helloController' defined in file </br>
> @Resource(name="io.github.isseebx123.tutorial.mapper.BoardMapper")
<pre>
  위 에러가 떠서 혹시나해서 @Resource구문을 지워봤더니 아래 에러가 떴음. @Resource는 뭐하는 애일까?
</pre>
> Error creating bean with name 'sqlSessionFactory' defined in io.github.isseebx123.tutorial.TutorialApplication: Unsatisfied dependency

## 결국 jpa때문에 다른 것에서 뜨는 에러를 못잡았음. 처음으로 돌아가서 하나씩 다시 해봐야함.
