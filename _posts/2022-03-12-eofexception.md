---
title:  "EOFException"
excerpt: "EOFException"

categories:
  - Java
tags:
  - Java
  - Exception
 
date: 2022-03-12
last_modified_at: 2022-03-12


---

## EOFException

- ObjectInputStream 클래스를 통해 직렬화된 인스턴스를 역직렬화 하는 과정에서 EOFException이 발생했다.
- ObjectInputStream 의 readObject() 메소드는 더 이상 읽어들일 데이터가 존재하지 않으면 null을 리턴하는 것이 아니라 EOFException 을 발생시킨다.
- 해결 방법 : EOFException 예외에 대한 예외 처리를 하거나 마지막 인스턴스의 뒤에 null을 추가해준다.







[https://junhyunny.blogspot.com/2019/03/exception-eofexception.html](https://junhyunny.blogspot.com/2019/03/exception-eofexception.html)
