---
title:  "Thymeleaf에서 enum 값을 넘겨주는 법"
excerpt: "Thymeleaf에서 enum값을 넘겨주는법"

categories:
  - Spring
tags:
  - Spring
 
date: 2025-01-16
last_modified_at: 2025-01-16
---

## Thymeleaf 에서 enum 값을 넘겨주는 법

![Image](https://github.com/user-attachments/assets/758c0ef9-a747-4790-a567-ca26eb04028c)

- 세 개의 게시판이 있지만 게시판에 글을 쓰는 view 페이지는 하나를 사용하고싶었다.
- 공지사항 게시판에서 글쓰기 버튼을 누르면 /board/create?subject=notice
  거래처 스펙 게시판에서 누르면 /board/create?subject=specs
  마음의 소리 게시판에서 누르면 /board/create?subject=heart
  같은 url이지만 파라미터 값을 달리하여 요청을 보내도록 했다.
- 게시판의 주제를 나타내는 subject 필드를 enum 클래스인 BoardSubject로 해두어서 글을 작성하는 페이지에서 String 타입으로 전달된 subject 값을 enum 으로 변경하지 않으면 타입이 맞지 않아 예외가 발생했다.

```java
public class Board {
    private Long boardId;
    private String date;
    private BoardSubject subject;
    private String memberId;
    private String title;
    private String content;
    private Long like;
}

public enum BoardSubject {
    NOTICE("notice"),
    SPECS("specs"),
    HEART("heart");

    private String subjectName;

    BoardSubject(String subjectName) {
        this.subjectName = subjectName;
    }

    public String getSubjectName() {
        return subjectName;
    }
}
```

- thymeleaf 의 반복문인 each, 조건문인 if 를 사용해서 해결했다.

```java
// enum 값 넘겨주기
@ModelAttribute("boardSubjects")
public BoardSubject[] boardSubjects() {
    return BoardSubject.values();
}
```

- @ModelAttribute 어노테이션을 사용해 board_form.html 페이지에 boardSubjects 라는 이름으로 BoardSubject를 넘겨주었다.

```html
<div th:each="boardSubject : ${boardSubjects}">
        <input type="text" th:value="${boardSubject}"
               th:if="${boardSubject.getSubjectName eq subject}"
               name="subject" ></input>
</div>
```

- boardSubject의 subjectName 과 요청파라미터로 넘어온 subject의 문자열끼리 비교해 일치하는  상수값을 전달하도록했다.

#### 내 생각

- 만약에 게시판의 수가 늘어나 비교연산의 수가 늘면 성능이 저하될 것 같다.
- 그냥 게시판별로 글쓰기 페이지를 만들어야할까? 



