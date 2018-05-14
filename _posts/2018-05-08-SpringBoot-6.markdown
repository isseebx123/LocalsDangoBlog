---
layout: post
title:  "SpringBoot 7일차 (react)"
date:   2018-05-09 17:00:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 본격적으로 CRUD를 구성해보자
### Controller <br/>
```java
package io.github.isseebx123.tutorial.controller;

import io.github.isseebx123.tutorial.domain.BoardVO;
import io.github.isseebx123.tutorial.mapper.BoardMapper;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.List;

@RestController
public class SampleController {

    @Autowired
    private SqlSessionTemplate sqlSession;

    @GetMapping("/")
    public String hello() {
        return "Hello world!";
    }

    //    C
    @RequestMapping(value = "/insertBoard", method = RequestMethod.PUT)
    public void insertBoard(@RequestParam("bno") Long bno, @RequestParam("title") String title,
                            @RequestParam("content") String content) {
        BoardVO boardVO = new BoardVO(bno, title, content);
        sqlSession.getMapper(BoardMapper.class).insertBoard(boardVO);
    }

    //    R
    @RequestMapping(value = "/selectBoard", method = RequestMethod.GET)
    public List<BoardVO> selectBoard() {
        List<BoardVO> list = sqlSession.getMapper(BoardMapper.class).selectBoard();
        return list;
    }

    //    U
    @RequestMapping(value = "/updateBoard", method = RequestMethod.POST)
    public void updateBoard(@RequestParam("bno") Long bno, @RequestParam("title") String title,
                            @RequestParam("content") String content) {
        sqlSession.getMapper(BoardMapper.class).updateBoard();
    }

    //    D
    @RequestMapping(value = "/deleteBoard", method = RequestMethod.DELETE)
    public void deleteBoard(@RequestParam("bno") Long bno) {
        sqlSession.getMapper(BoardMapper.class).deleteBoard();
    }
}
```

### Mapper <br/>
```java
package io.github.isseebx123.tutorial.mapper;

import io.github.isseebx123.tutorial.domain.BoardVO;
import org.apache.ibatis.annotations.Mapper;

import java.util.HashMap;
import java.util.List;

@Mapper
public interface BoardMapper {
    int boardCount();
    String getTime();

    void insertBoard(BoardVO boardVO);
    List<BoardVO> selectBoard();
    void updateBoard();
    void deleteBoard();
}
```

### Mapper.xml <br/>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="io.github.isseebx123.tutorial.mapper.BoardMapper">
    <!--C-->
    <insert id="insertBoard" parameterType="io.github.isseebx123.tutorial.domain.BoardVO">
        insert into board (bno, title, content)
        values (#{bno}, #{title}, #{content})
    </insert>

    <!--R-->
    <select id="boardCount" resultType="int">
        SELECT COUNT(*) FROM board
    </select>
    <select id="getTime" resultType="String">
        SELECT NOW()
    </select>
    <select id="selectBoard" resultType="io.github.isseebx123.tutorial.domain.BoardVO">
        SELECT bno, title, content FROM board LIMIT 0, 1000
    </select>

    <!--U-->
    <update id="updateBoard" parameterType="io.github.isseebx123.tutorial.domain.BoardVO">
        update boardd set
          title = #{title},
          content = #{content}
        where bno = #{bno}
    </update>

    <!--D-->
    <delete id="deleteBoard">
        delete from board where bno = #{bno}
    </delete>

</mapper>
```

### BoardPage <br/>
```js
import React from 'react'

const client = require('../client')

class BoardPage extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            boards: [],
            isLoading: false,
        }
    }

    componentDidMount() {
        client({method: 'GET', path: '/selectBoard'}).done(response => {
            this.setState({
                boards: response.entity,
                isLoading: true,
            });
        });
    }

    render() {
        if (this.state.isLoading === true) {
            return (
                <div>
                    <Boards boards={this.state.boards}/>
                    {insertBoard()}{/*<insertBoard />*/}
                    {updateBoard()}{/*<updateBoard />*/}
                    {deleteBoard()}{/*<deleteBoard />*/}
                </div>
            )
        }
        else {
            return (
                <div>
                    <p>Loading ...</p>
                </div>
            )
        }
    }
}

// tag::board-list[]
class Boards extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        var boards = this.props.boards.map(board =>
            <Board key={board.bno} board={board}/>
        );
        return (
            <table>
                <tbody>
                <tr>
                    <th>bno</th>
                    <th>title</th>
                    <th>content</th>
                </tr>
                {boards}
                </tbody>
            </table>
        )
    }
}

// end::board-list[]

// tag::board[]
class Board extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <tr>
                <td>{this.props.board.bno}</td>
                <td>{this.props.board.title}</td>
                <td>{this.props.board.content}</td>
            </tr>
        )
    }
}
// end::board[]

function insertBoard() {
    return (
        <div>
            <h2>Insert Board</h2>
            <p style={pNextToInputStyle}>bno: </p><input type="number" name="bno" /><br/>
            <p style={pNextToInputStyle}>title: </p><input type="text" name="title" /><br/>
            <p style={pNextToInputStyle}>content: </p><input type="text" name="content" />
        </div>
    );
}

function updateBoard() {
    return (
        <div>
            <h2>Update Board</h2>
            <p style={pNextToInputStyle}>bno(primary key): </p><input type="number" name="bno" /><br/>
            <p style={pNextToInputStyle}>title: </p><input type="text" name="title" /><br/>
            <p style={pNextToInputStyle}>content: </p><input type="text" name="content" />
        </div>
    );
}

function deleteBoard() {
    return (
        <div>
            <h2>Delete Board</h2>
            <p style={pNextToInputStyle}>bno: </p><input type="number" name="bno" /><br/>
        </div>
    );
}

const pNextToInputStyle = {
    display: 'inline'
}

export default BoardPage;
```
<pre>
	insertBoard, update, delete를 태그형식으로 하면 안되던데 왜 안되는 건지 잘모르겠음.
  functional component로 적용이 안되는건가?
</pre>

### css 적용 -> style로
```js
<p style={pNextToInputStyle}>bno: </p><input type="number" name="bno" /><br/>

const pNextToInputStyle = {
    display: 'inline'
}
```
<pre>
  css적용시 에러가 발생. 일단 예전에 배운대로 style로 지정
</pre>

### img 출력 -> url을 인터넷주소 및 서버로
<pre>
  단순 상대경로로 해서 프로젝트 내 이미지를 뿌리려 하니 경로를 잘 찾지못함.
  시간되면 이것도 원인을 파악해보자.
</pre>

# React에서 form은 어떻게?
## Controlled Component
입력할때마다 state를 갱신해주는 것
## UnControlled Component
submit 등 필요할 때만 state를 갱신하거나 값을 가져다 쓰는 것
---

## 참고사이트
[React에서 form]("https://blog.coderifleman.com/2015/06/27/learning-react-3/")
