---
title:  "java.lang.ClassNotFoundException: javax.servlet.http.HttpServlet"
excerpt: "자바 웹프로그래밍 Next Step 책을 보며 MVC 프레임워크를 구현하던 중에 오류가 발생했다"

categories:
  - Servlet
tags:
  - Servlet
 
date: 2025-01-17
last_modified_at: 2025-01-17
---
## java.lang.ClassNotFoundException: javax.servlet.http.HttpServlet

- 자바 웹프로그래밍 Next Step 책을 보며 MVC 프레임워크를 구현하던 중에 오류가 발생했다.

```java
@WebServlet("/user/create")
public class CreateUserServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final Logger log = LoggerFactory.getLogger(CreateUserServlet.class);

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        User user = new User(req.getParameter("userId"), req.getParameter("password"), req.getParameter("name"),
                req.getParameter("email"));
        log.debug("user : {}", user);
        DataBase.addUser(user);
        resp.sendRedirect("/user/list");
    }
}
```

- @WebServlet 어노테이션으로 /user/create url 과 CreateUserServlet 클래스를 매핑했다.
- 그런데 클라이언트에서 localhost:8080/user/create 로 요청을 보내면 500 에러가 발생했다.
- 서버에서 ClassNotFoundException이 발생했다.
- 검색을 해보니 사용중인 Servlet 버전이 맞지 않아 발생한 오류였다.

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

```xml
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

```

- 최신버전으로 바꿔주니 해결이 되었다. 야호~

[참고](https://stackoverflow.com/questions/77335852/java-lang-classnotfoundexception-javax-servlet-http-httpservlet-in-springmvc)
