---
layout: post
title:  "SpringBoot 1일차(1-1)"
date:   2018-04-27 13:00:00
author: 이상현
categories: Naver_Hackday_Ready
---

# Hello Spring Boot [영상링크]("https://www.youtube.com/watch?v=sUdH_DuDr14")
1. 인텔리제이 최신버젼 설치
- 커뮤니티 or 낮은버젼은 스프링 프로젝트 생성 플러그인 별도설치 해야함.
2. 프로젝트 생성
- 필요한 라이브러리 설정: 롬북, Devtools, Web, Mysql, Redis, mybatis
3. 실행
- src/main/resources/static에 정적인 html,css,img 파일 등이 들어감
- index.html이 있어야 localhost:8080으로 확인가능
4. Hello controller 추가
- src/main/java/com.github.io.springboot/api에 자바클래스 추가
```java
package com.github.io.springboot.api;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Boot Spring Boot!";
    }
}
```
- localhost:8080/hello를 통해 확인가능
5. 크롬의 확장프로그램 live reload를 이용 (hotreload)
- 이를 사용하려면 스프링부트 프로젝트에 devtools 디펜던시가 추가되어 있어야함.
- 추가로 인텔리제이에서는 springloaded 디펜던시를 추가하고 설정해야 하는듯.
