---
layout: post
title:  "SpringBoot-Book-6"
date:   2018-06-20 00:22:00
author: 이상현
categories: Springboot
---

# 연관관계 설정 (계속..)
## 기본적인 연관관계 설정방법 (ManyToOne)
```java
@Getter
@Setter
@ToString(exclude = "member")
@Entity
@Table(name = "tbl_profile")
@EqualsAndHashCode(of = "fno")
public class Profile {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long fno;
  private String fname;
  private boolean current;

  @ManyToOne
  private Member member;
}
```
<pre>
  Profile에서 Member를 가지는데 ManyToOne으로 설정하였다.
  하나의 멤버는 프로파일을 여러 개 가질 수 있으니까.
  이러한 연관관계는 프로파일을 통해 멤버에 접근할 필요가 있을 때만 위같이 설정한다.
  멤버에서는 현재 멤버를 통해 Profile을 관리하지 않으므로 OneToMany라던가 하는 것이 없다.
</pre>

```java
@ToString(exclude = "member")
```
<pre>
  @ToString의 exclude를 한 이유는 양방향 참조를 설정했을 때
  Profile의 toString에서는 Member의 toString을 호출하고
  Member의 toString에서는 Profile의 toString을 호출하여
  무한 루프가 돌 수 있기 때문이다.
</pre>

## Repository는 모두 만들어야 할까?
<pre>
  모두 만들어도, 만들지 않아도 된다. 엔티티 클래스마다 레포지터리를 설계해줄 수도 있지만
  연관관계 설정에 따라서는 레포지터리를 설계할 필요가 없을 수도 있다.
</pre>
```java
@Getter
@Setter
@ToString
@Entity
@Table(name = "tbl_pds")
@EqualsAndHashCode(of = "pid")
public class PDSBoard {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long pid;
  private String pname;
  private String pwriter;

  @OneToMany(cascade = CascadeType.ALL)
  @JoinColumn(name = "pdsno")
  private List<PDSFile> files;
}
```
```java
@Getter
@Setter
@ToString
@Entity
@Table(name = "tbl_pdsfiles")
@EqualsAndHashCode(of = "fno")
public class PDSFile {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long fno;
  private String pdsfile;
}
```
<pre>
  board와 file을 보면 PDSBoard의 레포지터리는 만들어주어야 하지만
  PDSFile은 PDSBoard를 통해서 관리할 수 있으므로 굳이 만들어주지 않아도 된다.
  하지만 그러려면 PDSBoard에서 추가적인 작업이 필요하다.
</pre>

## 레포지터리가 없는 엔티티의 수정에 대한 방법
### 첫째, @Query
```java
public interface PDSBoardRepository extends CrudRepository<PDSBoard, Long> {

  @Modifying
  @Query("UPDATE FROM PDSFile f set f.pdsfile = ?2 WHERE f.fno = ?1 ")
  public int updatePDSFile(Long fno, String newFileName);
}
```
<pre>
  이 처럼 Board는 가만히 놔두고 관계가 걸려있는 File 엔티티를 수정, 삭제하고 싶으면
  @Query로 native sql을 해주어야 한다.
  또 @Query는 기본적으로 Select만 지원하는데 @Modifying을 해야 수정이 가능하다고 한다.
</pre>
```java
@Modifying
@Query("DELETE FROM PDSFile f where f.fno = ?1")
public int deletePDSFile(Long fno);
```
<pre>
  삭제도 유사하다.
</pre>

### 둘째, 전통적인 방식으로 조회후 save
```java
@Transactional
@Test
public void testUpdateFileName2() {

  String newName = "updatedFile2.doc";
  Optional<PDSBoard> result = pdsBoardRepository.findById(2L);

  result.ifPresent(pds -> {
    log.info("데이터가 존재하므로 update 시도");
    PDSFile target = new PDSFile();
    target.setPdsfile(newName);
    target.setFno(3L);

    int idx = pds.getFiles().indexOf(target); // fno가 3인 녀석을 찾아서 인덱스를 반환
    log.info("idx : " + idx);
    if (idx > -1) {
      List<PDSFile> list = pds.getFiles();
      list.remove(idx);
      list.add(target);
      pds.setFiles(list);
    }

    pdsBoardRepository.save(pds);
  });
}
```

## OneToMany
<pre>
  위의 pds_board에 해당하는데 OneToMany와 ManyToOne은 동작방식이 다르다.
  ManyToOne은 추가 테이블을 만들지 않는데에 반해,
  OneToMany는 소프트웨어공학시간 때 배웠던 관계테이블을 만들어준다.
  추가로 @JoinColumn을 하면 관계테이블에 참조를 위한 필드가 추가된다.
</pre>

## 양방향관계 (OneToMany - ManyToOne)
<img src="{{ site.baseurl }}/assets/postImages/20180620/relation.jpg"> <br>
<pre>
  아무런 조작도 해주지 않으면 위와 같이 된다.
  OneToMany에 의해서 테이블이 생기는데 이는 우리가 원하는 방식이 아니다.
  그래서 MappedBy라는 것을 써서 댓글은 게시판에 매여있다고 알려주어야
  테이블을 생성하지 않는다고 한다.
</pre>
