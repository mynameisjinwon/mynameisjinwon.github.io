---
title:  "JSP 페이지에서 PrintWriter 객체를 생성해보자"
excerpt: "JSP 페이지에서 PrintWriter 객체를 생성해보자"

categories:
  - Java
tags:
  - Java
  - JSP
 
date: 2022-03-24
last_modified_at: 2022-03-24
---

## JSP 페이지에서 PrintWriter 객체를 생성해보자

- JSP페이지를 아래와 같이 작성하면

```jsp
<%@ page contentType="text/html;charset=utf-8" %>
<%@ page import="java.io.*"%>
<html>
    <head>
        <title>test</title>
    </head>
    <body>
        <h1>response.getWriter() 메소드를 사용해보자!</h1>
        결과가 어떻게 나올지 궁금하다 <br>
        <%
            PrintWriter myOut = response.getWriter();

            for(int i=0;i<100;i++) {
                myOut.printf("%d <br>", i);
            }
        %>
    </body>
</html>
```

- 서블릿 클래스의 jspServlet() 메소드는 이렇게 생성이된다.

```java
//~생략
      out.write("\r\n");
      out.write("\r\n");
      out.write("<html>\r\n");
      out.write("    <head>\r\n");
      out.write("        <title>test</title>\r\n");
      out.write("    </head>\r\n");
      out.write("    <body>\r\n");
      out.write("        <h1>response.getWriter() 메소드를 사용해보즈아~!</h1>\r\n");
      out.write("        결과가 어떻게 나올지 궁금하네여~ <br>\r\n");
      out.write("        결과가 어떻게 나올지 궁금하네여~ <br>\r\n");
      out.write("        결과가 어떻게 나올지 궁금하네여~<br>\r\n");
      out.write("        결과가 어떻게 나올지 궁금하네여~<br>\r\n");
      out.write("        결과가 어떻게 나올지 궁금하네여~<br>\r\n");
      out.write("        ");

            PrintWriter myOut = response.getWriter();

            for(int i=0;i<100;i++) {
                myOut.printf("%d <br>", i);
            }
        
      out.write("\r\n");
      out.write("    </body>\r\n");
      out.write("</html>");
//~생략
```



#### 실행 결과

```html
0 <br>1 <br>2 <br>3 <br>4 <br>5 <br>6 <br>7 <br>8 <br>9 <br>10 <br>11 <br>12 <br>13 <br>14 <br>15 <br>16 <br>17 <br>18 <br>19 <br>20 <br>21 <br>22 <br>23 <br>24 <br>25 <br>26 <br>27 <br>28 <br>29 <br>30 <br>31 <br>32 <br>33 <br>34 <br>35 <br>36 <br>37 <br>38 <br>39 <br>40 <br>41 <br>42 <br>43 <br>44 <br>45 <br>46 <br>47 <br>48 <br>49 <br>50 <br>51 <br>52 <br>53 <br>54 <br>55 <br>56 <br>57 <br>58 <br>59 <br>60 <br>61 <br>62 <br>63 <br>64 <br>65 <br>66 <br>67 <br>68 <br>69 <br>70 <br>71 <br>72 <br>73 <br>74 <br>75 <br>76 <br>77 <br>78 <br>79 <br>80 <br>81 <br>82 <br>83 <br>84 <br>85 <br>86 <br>87 <br>88 <br>89 <br>90 <br>91 <br>92 <br>93 <br>94 <br>95 <br>96 <br>97 <br>98 <br>99 <br>

<html>
    <head>
        <title>test</title>
    </head>
    <body>
        <h1>response.getWriter() 메소드를 사용해보자!</h1>
        결과가 어떻게 나올지 궁금하다 <br>
        
    </body>
</html>
```

- 출력 결과가 뒤죽박죽일 것을 예상하고 정수를 100개를 출력했는데
- 100개의 정수가 먼저 출력되고 html 코드는 그 뒤에 출력이되었다.

#### 이유

- JSP 페이지가 Servlet으로 변환되면 HTML 코드들을 JspWriter 객체를 통해 출력한다.
- JspWriter 는 버퍼가 가득 차지 않으면 출력하지 않기 때문에 (더 이상 데이터가 없거나(?))
- 버퍼가 다 채워지기 전에 PrintWriter 객체로 정수들이 출력되었다.

#### 제대로 출력되게 하려면?

- JSP 페이지를 작성할 때 page 지시자를 이용해 buffer 속성의 값을 "none"으로 지정한다.
- buffer 속성은 버퍼 사용 여부를 지정한다.

```jsp
<%@ page buffer="none" %>
```

#### 실행 결과

```html
<html>
    <head>
        <title>test</title>
    </head>
    <body>
        <h1>response.getWriter() 메소드를 사용해보자!</h1>
        결과가 어떻게 나올지 궁금하다 <br>
        0 <br>1 <br>2 <br>3 <br>4 <br>5 <br>6 <br>7 <br>8 <br>9 <br>10 <br>11 <br>12 <br>13 <br>14 <br>15 <br>16 <br>17 <br>18 <br>19 <br>20 <br>21 <br>22 <br>23 <br>24 <br>25 <br>26 <br>27 <br>28 <br>29 <br>30 <br>31 <br>32 <br>33 <br>34 <br>35 <br>36 <br>37 <br>38 <br>39 <br>40 <br>41 <br>42 <br>43 <br>44 <br>45 <br>46 <br>47 <br>48 <br>49 <br>50 <br>51 <br>52 <br>53 <br>54 <br>55 <br>56 <br>57 <br>58 <br>59 <br>60 <br>61 <br>62 <br>63 <br>64 <br>65 <br>66 <br>67 <br>68 <br>69 <br>70 <br>71 <br>72 <br>73 <br>74 <br>75 <br>76 <br>77 <br>78 <br>79 <br>80 <br>81 <br>82 <br>83 <br>84 <br>85 <br>86 <br>87 <br>88 <br>89 <br>90 <br>91 <br>92 <br>93 <br>94 <br>95 <br>96 <br>97 <br>98 <br>99 <br>
    </body>
</html>
```

- 입출력 스트림에 대해 더 공부해야겠다.

