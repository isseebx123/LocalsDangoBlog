---
layout: post
title:  "SpringBoot-Book-6"
date:   2018-06-25 02:31:00
author: 이상현
categories: Springboot
---

# 연관관계 처리
## 양방향관계 FreeBoard와 FreeBoardReply
### FreeBoard
```java
@Getter
@Setter
@ToString(exclude = "replies")
@Entity
@Table(name = "tbl_freeboards")
@EqualsAndHashCode(of = "bno")
@Builder
public class FreeBoard {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long bno;
  private String title;
  private String writer;
  private String content;

  @CreationTimestamp
  private Timestamp regdate;
  @UpdateTimestamp
  private Timestamp updatedate;

  @OneToMany(mappedBy = "board")
  private List<FreeBoardReply> replies;
}
```
<pre>
  새삼스럽지만 이제와서 @EqualsAndHashCode에 대해서 부연설명을 달자면
  equals()와 hashcode()를 생성해주고, of는 명시적으로 해당필드에 대해 생성해준다는 것이다.
  만약 exclude 옵션을 쓰면 명시적으로 생성에서 제외한다는 것이다.

  mappedBy는 board가 reply에 매여있음을 표시하여 OneToMany로 인해 별도의 테이블이 생성되지 않도록 한다.
  소공때 배웠던 것으로 따지면 새로 생성되는 것은 관계 테이블(릴레이션)이 될 수 있겠다.
  매여있음의 관계는 테이블을 삭제할때로 생각해보라고 책에 명시되어 있다.
  board를 삭제하려 해도 reply가 board에 매여있으므로(외래키 제약관계)
  board에 해당하는 reply가 살아있으면 삭제가 불가능한 것처럼 말이다.

  또 테스트에서 빌더패턴을 사용해보기 위해 롬북의 builder인 @Builder를 걸어줬다.
</pre>

### FreeBoardReply
```java
@Getter
@Setter
@ToString(exclude = "board")
@Entity
@Table(name = "tbl_free_replies")
@EqualsAndHashCode(of = "rno")
public class FreeBoardReply {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long rno;
  private String reply;
  private String replyer;

  @CreationTimestamp
  private Timestamp replydate;
  @UpdateTimestamp
  private Timestamp updatedate;

  @ManyToOne
  private FreeBoard board;
}
```
<pre>
  다른 것과 큰 차이는 없는 코드인 것 같다.
</pre>

### FreeBoardTest
```java
@Test
public void insertDummy(){
  IntStream.range(1,200).forEach(i -> {
    FreeBoard board = FreeBoard.builder()
        .title("Free Board ... " + i)
        .content("Free content .... " + i)
        .writer("user" + i%10)
        .build();

    freeBoardRepository.save(board);
  });
}
```
<pre>
  빌더패턴을 이용하여 객체를 생성하였다.
  IntStream이나 빌더나 모두 java8의 기능이기 때문에 별도의 공부가 필요할 것 같다.
  이들은 대부분 옛날의 함수형언어를 다시 가져오려는 것으로
  그냥 일반적으로 for문이나, 생성자를 통해 대입하는 것보다 코드의 가독성이 높아진다!
</pre>

### Builder 사용으로 인한 오류!
<b>
  No default constructor for entity
</b>
<pre>
  
</pre>
