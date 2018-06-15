---
layout: post
title:  "SpringBoot-Book-5"
date:   2018-06-08 11:20:00
author: 이상현
categories: Springboot
---

# JPA
## querysql 사용해보기
```xml
<dependency>
  <groupId>com.querydsl</groupId>
  <artifactId>querydsl-apt</artifactId>
  <version>4.1.4</version>
</dependency>
<dependency>
  <groupId>com.querydsl</groupId>
  <artifactId>querydsl-jpa</artifactId>
  <version>4.1.4</version>
</dependency>
```
```xml
			<plugin>
				<groupId>com.mysema.maven</groupId>
				<artifactId>apt-maven-plugin</artifactId>
				<version>1.1.3</version>
				<executions>
					<execution>
						<goals>
							<goal>process</goal>
						</goals>
						<configuration>
							<outputDirectory>target/generated-sources/java</outputDirectory>
							<processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
						</configuration>
					</execution>
				</executions>
			</plugin>
```
<pre>
  위와 같이 추가하면 target/generated-sources/java에 QBoard가 생성된다고 한다.
  그런데 생성이 안되어 [여기]("https://stackoverflow.com/questions/1607315/build-maven-project-without-running-unit-tests")를 참고했는데,
  maven clean, maven install을 하라고 한다. 그런데 그래도 생성이 되지 않는다...
  참고로 메이븐 빌드시 테스트를 뛰어넘는 옵션은 -DskipTests이다.
</pre>
<img src="{{ site.baseurl }}/assets/postImages/20180608/jpa.png"> <br>
<pre>
  아니네.. 다시봤더니 생성은 됬었는데 내가 못찾았었던 것 같다.
  이후에 Board 인터페이스가 QueryDslPredicateExecutor를 추가로 상속하도록 설정하여 querydsl을 사용할 수 있게 한다.
</pre>
```java
@Generated("com.querydsl.codegen.EntitySerializer")
public class QBoard extends EntityPathBase<Board> {

    private static final long serialVersionUID = 1140750113L;

    public static final QBoard board = new QBoard("board");

    public final NumberPath<Long> bno = createNumber("bno", Long.class);

    public final StringPath content = createString("content");

    public final DateTimePath<java.sql.Timestamp> regdate = createDateTime("regdate", java.sql.Timestamp.class);

    public final StringPath title = createString("title");

    public final DateTimePath<java.sql.Timestamp> updatedate = createDateTime("updatedate", java.sql.Timestamp.class);

    public final StringPath writer = createString("writer");

    public QBoard(String variable) {
        super(Board.class, forVariable(variable));
    }

    public QBoard(Path<? extends Board> path) {
        super(path.getType(), path.getMetadata());
    }

    public QBoard(PathMetadata metadata) {
        super(Board.class, metadata);
    }

}
```
<pre>
  domain을 기준으로 동적쿼리를 위한 파일이 생성되었다.
  그냥 봐서는 어떤 기능을 하는지는 잘 모르기에 그냥 가져다가 사용하면 되겠다.
</pre>
```java
@Test
public void testPredicate() {
  String type = "t";
  String keyword = "17";

  BooleanBuilder builder = new BooleanBuilder();
  QBoard qBoard = QBoard.board;

  if (type.equals("t")) {
    builder.and(qBoard.title.like("%" + keyword + "%"));
  }

  // bno > 0
  builder.and(qBoard.bno.gt(0L));
  Pageable pageable = new PageRequest(0, 10);
  Page<Board> result = boardRepository.findAll(builder, pageable);

  System.out.println("PAGE SIZE: " + result.getSize());
  System.out.println("TOTAL PAGES: " + result.getTotalPages());
  System.out.println("TOTAL COUNT: " + result.getTotalElements());
  System.out.println("NEXT: " + result.nextPageable());

  List<Board> list = result.getContent();
  list.forEach(b -> System.out.println(b));
}
```
<pre>
  위는 querydsl을 사용한 테스트이다.
</pre>
