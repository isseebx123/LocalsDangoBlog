---
layout: post
title:  "Start SpringBoot 교재학습 2일차(챕터2)"
date:   2018-04-30 12:00:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 전날했던 리액트에서 스프링부트 API사용 예제
```js
componentDidMount() {
  this.setState({
    isLoading: true,
  })
  // fetch('http://localhost:8080/name')
  //   .then(response => response.json())
  //   .then(data => this.setState({name: name, isLoading: false}))
  fetch('http://localhost:8080/name')
     .then(response => console.log(response))
     .then(console.log(this.state.name))
}
```
<pre>
	componentDidMount에서 위의 작업으로 api를 통해 데이터를 받아옴.
  아직 제대로된 데이터를 구성하지 않아서 제대로는 확인이 불가능.
</pre>

```java
@CrossOrigin(origins = {"http://localhost:3000", "http://localhost:5000"})
```
<pre>
	또 스프링부트의 컨트롤러의 해당 api메소드에서는 위의 작업을 해주어야 에러가 안뜸. CORS?
</pre>

# 1. Spring Data JPA 입문 전 계속 ...
## properties에 설정
```java
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=mysql://localhost:3306/jpa_ex?useSSL=false
spring.datasource.username=jpa_user
spring.datasource.password=jpa_user
```
<pre>
Datasource에 대한 설정을 해주어야 한다.
</pre>

## 도메인 생성
```java
package io.github.isseebx123.tutorial.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.*;
import java.sql.Timestamp;

@Getter
@Setter
@ToString
@Entity
@Table(name="tbl_boards")
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long bno;
    private String title;
    private String writer;
    private String content;

    @CreationTimestamp
    private Timestamp regdate;
    @UpdateTimestamp
    private Timestamp update;
}
```
<pre>
	@Table: 테이블이름을 별도로 지정해주고 싶을 때 사용
  @Id: 식별자
  @GeneratedValue: @Id와 같이 사용되고, 식별자를 선택하는 전략을 설정
  @CreationTimestamp: 엔티티가 생성될 때 시간체크
  @UpdateTimestamp: 엔티티가 업데이트될 때 시간체크
  추가로 빨간 줄 뜨는 것은 Alt+Enter-Add maven dependency로 추가
</pre>

## properties에 JPA관련 설정 추가
```java
#스키마 생성
spring.jpa.hibernate.ddl-auto=create
#DDL 생성시 데이터베이스의 고유기능을 사용하는가?
spring.jpa.generate-ddl=false
#실행되는 SQL문을 보여줄 것인가?
spring.jpa.show-sql=true
#데이터베이스는 무엇을 사용하는가?
spring.jpa.database=mysql
#로그 레벨
logging.level.org.hibernate=info
#MYSQL 상세 지정
spring.jpa.database-platform=org.hibernate.dialect.MYSQL5InnoDBDialect
```
<pre>
이후 프로젝트를 실행하면 테이블을 생성하는 SQL의 동작을 확인 가능
</pre>
