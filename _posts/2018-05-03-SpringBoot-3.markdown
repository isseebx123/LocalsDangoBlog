---
layout: post
title:  "SpringBoot 4일차 (mybatis)"
date:   2018-05-03 17:46:00
author: 이상현
categories: Naver_Hackday_Ready
---

# hello mybatis와 에러 해결 계속!
## 1. sqlSessionFactory를 생성할 수 없다는 에러
> 그런데 depth를 따라가서 제일 아래를 자세히 보니 url을 못 읽어오는 듯 보였음. <br/>
> convert .yml을 해주었는데도 왜 안될까?
<pre>
  원래 properties로 해주니 에러가 해결되었다. 헐.. 콘솔창 좀 제대로 봐야할듯.
</pre>

## 2. No Mapper Found ... 에러
> @MapperScan(value = "io.github.isseebx123.tutorial.mappers")의 패키지명 경로를 잘못설정해주었음
<pre>
  인터페이스가 있는 패키지명으로 @MapperScan(value = "io.github.isseebx123.tutorial.mapper") 다시설정.
</pre>

# 결과 Hello Mybatis 샘플 코드
## MybatisConfiguration.java
```java
package io.github.isseebx123.tutorial.configuration;

import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.type.DateTypeHandler;
import org.apache.ibatis.type.TypeHandler;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

import javax.sql.DataSource;
import java.util.Properties;

@Configuration
@MapperScan(value = "io.github.isseebx123.tutorial.mapper")
public class MybatisConfiguration {

    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
        sessionFactory.setDataSource(dataSource);

        Resource[] res = new PathMatchingResourcePatternResolver()
                .getResources("classpath:/mappers/*Mapper.xml");

        sessionFactory.setMapperLocations(res);
        sessionFactory.setTypeAliasesPackage("io.github.isseebx123.tutorial.domain");
        sessionFactory.setTypeHandlers(new TypeHandler[]{
                new DateTypeHandler()
        });

        Properties properties = new Properties();
        properties.setProperty("mapUnderscoreToCamelCase", "true");

        sessionFactory.setConfigurationProperties(properties);

        return sessionFactory.getObject();
    }

    @Bean
    public SqlSessionTemplate sqlSession(SqlSessionFactory sqlSessionFactory) {
        return new SqlSessionTemplate(sqlSessionFactory);
    }
}
```

## SampleController.java
```java
package io.github.isseebx123.tutorial.controller;

import io.github.isseebx123.tutorial.mapper.BoardMapper;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class SampleController {

    @Autowired
    private SqlSessionTemplate sqlSession;

    @GetMapping("/")
    public String hello() {
        return "Hello world!";
    }

    @GetMapping("/name")
    @CrossOrigin(origins = {"http://localhost:3000", "http://localhost:5000"})
    public String name() {
        return "sanghyun lee";
    }

    @GetMapping("/count")
    public int boardCount() {
        return sqlSession.getMapper(BoardMapper.class).boardCount();
    }
}
```

## BoardVO.java
```java
package io.github.isseebx123.tutorial.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class BoardVO {
    private Long bno;
    private String title;
    private String content;
}
```

## BoardMapper.java
```java
package io.github.isseebx123.tutorial.mapper;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface BoardMapper {

    int boardCount();
}
```

## resources.mappers/BoardMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="io.github.isseebx123.tutorial.mapper.BoardMapper">
    <select id="boardCount" resultType="int">
        SELECT COUNT(*) FROM board
    </select>
</mapper>
```

## application.properties
<pre>
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/jpa_ex?useSSL=false&characterEncoding=utf8
spring.datasource.username=jpa_user
spring.datasource.password=jpa_user
</pre>
