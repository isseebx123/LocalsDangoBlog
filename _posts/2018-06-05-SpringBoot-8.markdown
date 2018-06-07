---
layout: post
title:  "SpringBoot-Book-4"
date:   2018-06-05 20:13:00
author: 이상현
categories: Springboot
---

# 조바심 내지말고 천천히 하기!
# JPA
## Repository
<img src="{{ site.baseurl }}/assets/postImages/20180608/repo.png"> <br>
<pre>
  Repository는 모두 인터페이스로 무언가 코드를 구현해야 하는게 아니다.
  단순히 사용하고 싶은 Repository 인터페이스를 우리가 만든 엔티티레포지터리에서 상속한다.
  이후 쿼리메소드 등은 Spring Data JPA에서 정해진 명명 방식에 따라서 함수 이름을 명명하고 사용하면 된다.
  find로 시작하고 By이후에 조건절을 넣음으로써 select문과 같은 기능을 한다.
</pre>
```java
  public interface BoardRepository extends CrudRepository<Board, Long> {

  public List<Board> findBoardByTitle(String title);

  public Collection<Board> findByWriter(String writer);

  public Collection<Board> findByWriterContaining(String writer);

  public Collection<Board> findByTitleContainingOrContentContaining(String title, String content);

  public Collection<Board> findByOrderByBnoDesc();

  //  pageable을 이용하면 Collection을 리턴타입으로 지정불가. List, ..., ...의 3가지를 사용가능
  public List<Board> findByOrderByBnoDesc(Pageable pageable);

  // 단순히 pageable만 적용하려면 naming을 어떻게 적용해야 할까??
  // OrderBy로 주고 pageable에서는 limit처리만 하면 가능하긴 할듯
  public List<Board> findByBno(Long bno, Pageable pageable);

  public Page<Board> findByBnoGreaterThan(Long bno, Pageable pageable);

  // ?1은 첫번째로 전달된 파라미터라는 의미
  @Query("SELECT b FROM Board b WHERE b.title LIKE %?1% AND b.bnp > 0 ORDER BY b.bno DESC")
  public List<Board> findByTitle(String title);

  @Query("SELECT b FROM Board b WHERE b.content LIKE %:content% AND b.bnp > 0 ORDER BY b.bno DESC")
  public List<Board> findByContent(@Param("content") String content);
}
```

## Repository test
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Chapter03Test {

  @Autowired
  private BoardRepository boardRepository;

  @Test
  public void testInsert() {
    for (int i = 1; i <= 200; i++) {
      Board board = new Board();
      board.setTitle("제목.." + i);
      board.setContent("내용..:" + i);
      board.setWriter("user0" + (i % 10));
      boardRepository.save(board);
    }
  }

  @Test
  public void testByTitle() {
    boardRepository.findBoardByTitle("제목..:117")
        .forEach(board -> System.out.println(board));
  }

  @Test
  public void testByWriter() {
    Collection<Board> results = boardRepository.findByWriter("user00");

    results.forEach(
        board -> System.out.println(board)
    );
  }

  @Test
  public void testByWriterContaining() {
    Collection<Board> results = boardRepository.findByWriterContaining("05");
    results.forEach(
        board -> System.out.println(board)
    );
  }

  @Test
  public void testByTitleContainingOrContentContaining() {
    Collection<Board> results = boardRepository
        .findByTitleContainingOrContentContaining("06", "06");
    results.forEach(board -> System.out.println(board));
  }

  @Test
  public void testByOrderByBnoDesc() {
    Collection<Board> results = boardRepository.findByOrderByBnoDesc();
    results.forEach(
        board -> System.out.println(board)
    );
  }

  @Test
  public void testByOrderByBnoDescByPaging() {
    Pageable paging = new PageRequest(0, 10); // 페이지번호, 페이지당 데이터의 수
    Collection<Board> results = boardRepository.findByOrderByBnoDesc(paging);
    results.forEach(board -> System.out.println(board));
  }

  @Test
  public void testByBnoByPaging() {
    Pageable pageable = new PageRequest(0, 10, Sort.Direction.ASC, "bno");
    Collection<Board> results = boardRepository.findByBno(2L, pageable);
    results.forEach(board -> System.out.println(board));
  }

  @Test
  public void testByBnoPagingSort() {
    Pageable paging = new PageRequest(0, 10, Sort.Direction.ASC, "bno");
    Page<Board> result = boardRepository.findByBnoGreaterThan(0L, paging);

    System.out.println("PAGE SIZE: " + result.getSize());
    System.out.println("TOTAL PAGES: " + result.getTotalPages());
    System.out.println("TOTAL COUNT: " + result.getTotalElements());
    System.out.println("NEXT: " + result.nextPageable());

    List<Board> list = result.getContent();
    list.forEach(board -> System.out.println(board));
  }

  @Test
  public void testByTitle2() {
    boardRepository.findByTitle("17")
        .forEach(board -> System.out.println(board));
  }

  @Test
  public void testByContent2(){
    boardRepository.findByContent("17").forEach(board -> System.out.println(board));
  }
}
```
<pre>
  test 파일을 만들 때 제일 상단의 어노테이션을 추가하는 것을 까먹지 말자.
  어노테이션이 없어서 발생하는 오류에 대해서는 제대로 띄워주지 않는다.
</pre>
```java
@RunWith(SpringRunner.class)
@SpringBootTest
```
