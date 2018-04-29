---
layout: post
title:  "Start SpringBoot 교재학습 1일차(챕터1)"
date:   2018-04-29 12:37:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 스프링 부트 내의 빈 테스트
```java
@RestController
public class HelloController {

}
```
<pre> @RestController어노테이션을 통해 Controller를 추가, 스프링의 빈으로 등록되도록 한다. </pre>


```java
@RestController
public class HelloController {
  @GetMapping("/hello")
  public String sayHello(){
    return "Hello world";
  }
}
```
<pre> localhost:8080/hello를 통해 확인가능 </pre>

# lombok 사용
1. 설정
- 인텔리제이 Find Action 키가 Ctrl+Shift+A
- Plugins에서 lombok 설치
- maven으로 추가하더라도 플러그인으로 설치되어 있어야하나? 그런듯

2. 사용

```java
package com.github.io.springboot.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class SampleVO {
    private String val1;
    private String val2;
    private String val3;
}
```
<pre> domain 패키지에 SampleVO 클래스를 생성 </pre>

```java
@GetMapping("/sample")
public SampleVO makesample() {
    SampleVO vo = new SampleVO();
    vo.setVal1("v1");
    vo.setVal2("v2");
    vo.setVal3("v3");

    System.out.println(vo);
    return vo;
}
```
<pre> Controller에 추가하고 확인해보면 </pre>
```js
{"val1":"v1","val2":"v2","val3":"v3"}
```
<pre> 로 뜨는 것을 볼 수 있음 </pre>

# 컨트롤러 테스트
```java
package com.github.io.springboot;

import com.github.io.springboot.api.HelloController;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;

@RunWith(SpringRunner.class)
@WebMvcTest(HelloController.class)
public class SampleControllerTest {

    @Autowired
    MockMvc mock;

    @Test
    public void testHello() throws Exception {
        mock.perform(get("/hello")).andExpect(content().string("Hello, Boot Spring Boot!!"));
    }
}
```
<pre> @WebMvcTest와 같이 사용하면 별도의 생성 없이 주입(@Autowired)만으로 코드를 작성할 수 있다. </pre>
<pre> Alt + Enter로 자동 import를 했더니 content는 다른 것이 임포트됨 </pre>
<pre> 이런거를 외우기는 오바겠지.. Run하면 JUnit에서 테스트 </pre>

```java
    @Test
    public void testHello() throws Exception {
        MvcResult res = mock.perform(get("/hello")).
                andExpect(content().string("Hello, Boot Spring Boot!!")).andReturn();

        System.out.println(res.getResponse().getContentAsString());
    }
```
<pre> andReturn으로 결과값을 확인해볼 수도 있음. </pre>
