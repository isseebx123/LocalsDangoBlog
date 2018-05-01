---
layout: post
title:  "Start SpringBoot 교재학습 3일차(챕터2)"
date:   2018-05-01 23:36:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 1. 에러 !!!
> org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource </br>
<pre>
  이라는 에러가 발생! 여러 시도를 해봤지만 아직 해결되지 않았음.
  1. hibernate에 대한 디펜던시 버젼이 잘못되었거나
  2. entitymanager에 대한 디펜던시가 없어서 그렇거나
  3. spring boot의 버전이 높아졌는데 추가 설정을 해주지 않아서 그렇거나
</pre>

> Error creating bean with name 'org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration': Unsatisfied dependency expressed through constructor parameter 0;
<pre>
  스프링부트의 버전을 1.5.4로 바꾸니 위와 같은 에러가 발생하였음.
</pre>

# 2. 에러 원인 파악과 해결
## application.properties -> application.yml
<pre>
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/jpa_ex?useSSL=false&characterEncoding=utf8
    username: jpa_user
    password: jpa_user

  jpa:
    database: mysql
    hibernate:
      ddl-auto: create

    generate-ddl: true
    show-sql: true
    properties:
      hibernate.format_sql: true
      hibernate.use_sql_comments: true
      hibernate.connection.CharSet: utf8
      hibernate.connection.characterEncoding: utf8
      hibernate.connection.useUnicode: true
</pre>
<pre>
  왜 확장자가 properties일때는 정상작동하지 못하는가는 해결하지 못했지만,
  ConvertProperties to YAML 플러그인을 설치하고, 명시적으로 properties파일에서 오른쪽클릭 후
  yml로 변환해준후 위와같은 형식으로 변경해야 한다.
</pre>

```js
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>1.5.3.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>
```
<pre>
	그리고 pom에서는 스프링부트 버전을 낮춰준다. 다시 올렸을때도 동일한 현상이 나타나는지는 확인해봐야한다.
  아마 에러가 안나타나지 않을까?
  마지막으로.. 성렬이형 감사합니다. ㅠ
</pre>
