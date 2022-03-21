---
title:  "Servlet 클래스 명령 프롬프트에서 컴파일하기"
excerpt: "Servlet 클래스 명령 프롬프트에서 컴파일하기"

categories:
  - Java
tags:
  - Java
  - Servlet
 
date: 2022-03-18
last_modified_at: 2022-03-18
---

## Servlet 클래스 명령 프롬프트에서 컴파일하기

#### Servlet 클래스를 컴파일 하는데 오류가 발생했다.

> HundredServlet.java:1: error: package javax.servlet.http does not exist
> import javax.servlet.http.*;
> HundredServlet.java:2: error: package javax.servlet does not exist
> import javax.servlet.*;

- javax.servlet, javax.servlet.http 패키지가 jdk의 표준 라이브러리가 아니기 때문이다. Java EE (Java Enterprise Edition) 에 속한다. (여기에 JSP, Servlet, JDBC 등이 있다.)
- 컴파일 할 때 -cp 옵션을 이용해 해당 패키지가 존재하는 경로를 classpath로 지정해주거나 jdk 디렉토리의 서브 디렉토리 jre/lib/ext 에 servlet-api.jar 파일을 복사하면 해결된다고 책에는 써있었다.
- 근데 안된다. jdk 폴더에는 jre 폴더가 존재하지 않는다. 자바 9 부터 바뀐 내용이라고한다. [참조](https://okky.kr/article/639864?note=1816018)
- -cp 옵션으로 클래스 패스를 지정해도 안된다.



#### 해결방법

- 스택 오버플로우에서 같은 내용의 오류를 가진사람의 질문을 발견했다. [링크](https://stackoverflow.com/questions/9193228/compile-error-package-javax-servlet-does-not-exist/9193282#9193282?newreg=562a167d5d684190b1f6f1c40ecbf93f)

> If you're still facing the same complation error, and you're *actually* using Tomcat 10 or newer, then you should be migrating the imports in your source code from `javax.*` to `jakarta.*`.
>
> ```java
> import jakarta.servlet.*;
> import jakarta.servlet.http.*;
> ```
>
> In case you want to keep using `javax.*` for whatever reason, then you should be downgrading to Tomcat 9 or older as that was the latest version still using the old `javax.*` namespace.

- 톰캣 10 이상의 버전을 사용하는 경우 javax가 아닌 jakarta import 구문을 바꿔야한다.
- javax 라는 패키지 이름을 사용하기 위해서는 톰캣 9 이하의 버전을 사용해야한다.



#### javax 패키지가 jakarta가 된 이유

- Java EE는 2017년 오라클에서 이클립스 재단으로 이관되었다.
- 2018년 Java EE 는 Jakarta EE 로 명칭을 바꿨다.
- Java EE 프로젝트는 이관되었지만 자바 상표권은 오라클에 있기 때문에 자바 네임스페이스 사용에 제약이 있어 Jakarta EE(Jakarta, Enterprise Edition)에서는 Java 를 Jakarta 로 변경되었다.



#### cmd 에서 classpath 추가하기

##### 현재 classpath 출력

- 지정된 classpath가 없으면 %classpath% 가 출력된다.

```
echo %classpath%
```



##### classpath 지정하기

- 현재 디렉토리(.)를 classpath로 지정(할 필요는 없다.)

```
set classpath=.
```

- 현재 디렉토리와 상위 디렉토리를 classpath로 지정
- 세미콜론(;)으로 패스를 구분한다.

```
set classpath=.;..
```

- Java EE 라이브러리가 저장된 톰캣의 lib 디렉토리를 클래스패스로 지정

```
set classpath=.;C:\Users\g_seismic\Documents\apache-tomcat-9.0.60\lib\servlet-api.jar
```



- cmd 창에서 컴파일하는 경우가 많지는 않지만 servlet 실습에서는 자주 쓰일 것 같아 시스템변수로 설정해두었다.



___

#### 참조 

[https://stackoverflow.com/questions/9193228/compile-error-package-javax-servlet-does-not-exist/9193282#9193282?newreg=562a167d5d684190b1f6f1c40ecbf93f](https://stackoverflow.com/questions/9193228/compile-error-package-javax-servlet-does-not-exist/9193282#9193282?newreg=562a167d5d684190b1f6f1c40ecbf93f)

[Java EE에서 Jakarta EE로의 전환](https://www.samsungsds.com/kr/insights/java_jakarta.html)

