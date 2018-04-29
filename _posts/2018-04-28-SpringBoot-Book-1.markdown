---
layout: post
title:  "Start SpringBoot 교재학습 1일차(챕터2)"
date:   2018-04-29 13:30:00
author: 이상현
categories: Naver_Hackday_Ready
---

# Spring Data JPA 입문 전 이론
## ORM(Object Relational Mapping)란?
> ORM은 ORM 프레임워크는 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결해 준다. 데이터베이스에 데이터를 저장할 때 INSERT/UPDATE SQL구문을 직접 작성하는 것이 아닌 자바 컬렉션에 저장하듯 프레임워크에 저장하면 된다. 그러면 ORM 프레임워크가 적절한 SQL을 생성해서 데이터베이스에 매핑된 객체를 저장해 준다.

## JPA(Java Persistence API)란?
> Java를 이용해서 데이터를 관리하는 기법을 하나의 스펙으로 정리한 표준이다. JPA는 Java진영 에서 사용하는 ORM 기술 표준 이다. 즉 자발 기반에서 구현할 수 있는 ORM 표준 기술로 위에서 애기했던 SQL 작성 없이 객체를 데이터베이스에 직접 저장 할 수 있게 도와주고, 객체와 관계형 데이터베이스의 패러다임 불일치 문제도 해결해 준다.

## 위의 2개에 대한 [출처]("https://wiseyoun07.blog.me/221040082705")

## JPA 구조에 대한 설명
1. 엔티티
- 데이터베이스에서 데이터로 관리하는 대상. 예를 들어, '상품', '회사', '직원' 등과 같음
- 데이터베이스에서는 엔티티를 위해서 일반적으로 테이블을 설계하고 데이터를 추가함
- JPA에서 '하나의 엔티티 타입을 생성한다'라는 의미는 '하나의 클래스를 작성한다'는 의미가 된다

2. 엔티티매니저
- 여러 엔티티 객체들을 관리하는 역할을 함
- 자신이 관리해야할 엔티티 객체들을 '영속 컨텍스트(Persistence Context)'라는 곳에 넣어둠

3. 영속 컨텍스트와 엔티티 객체 [출처]("http://heowc.tistory.com/55")
1) 비영속(new / transist)
 - 일반적인 인스턴스이라고 볼 수 있습니다. ( new Entity() )

2) 영속(managed)
 - EntityManager에 persist를 통해 인트스턴스를 영속성 컨텍스트에 넣어진 상태입니다.

3) 준영속(detached)
 - 영속성 컨텍스트로부터 나온 객체입니다.
 - detach, clear, close 등으로 가능합니다.
 - 거의 비영속 상태라고 볼 수 있습니다
 - 지연로딩이 불가능합니다.
 - 식별자(@Id)가 존재합니다.
 - merge 로 다시 영속성 상태로 만들 수 있습니다.

4) 삭제(removed)
 - Database Side에서 지워졌지만 영속성 컨텍스트에서는 존재하는 상태입니다.

# Spring Data JPA
## 위에서 했던 처리 없이 다음과 같은 형태로 개발을 함 [출처]("https://spring.io/guides/gs/accessing-data-jpa/")
```java
import java.util.List;
import org.springFramework.data.repository.CrudRepository;

public interface CustomerRepository extends CrudRepository<Customer, Long> {
  List<Customer> findByLastName(String lastName);
}
```
> Spring Data JPA는 동적으로 인터페이스를 구현하는 클래스를 만들어 내는 방식을 이용해서, 실제 클래스를 작성하지 않아도 <br/>
> 자동으로 만들어지기 때문에 별도의 코드를 작성할 필요가 없다
