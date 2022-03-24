---
title:  "자동으로 인코딩하는 FileReader"
excerpt: "자동으로 인코딩하는 FileReader"

categories:
  - Java
tags:
  - Java
  - File I/O
 
date: 2022-03-25
last_modified_at: 2022-03-25
---

## 자동으로 인코딩하는 FileReader

- JSP 페이지에서 파일입력을 위해 FileReader 클래스를 사용하는데 
- 인코딩 문제가 발생했다.

```jsp
<%@ page contentType="text/html;charset=utf-8"%>
<%@ page import="java.io.*"%>
<html>
    <head>
        <title>노래 가사 출력</title>
    </head>
    <body>
        <% 
            // FileReadPractice.jsp 에서 url parameter 로 파일이름에대한 files 리스트의 인덱스를 전달한다.
            // idx 에 인덱스 번호를 저장한다.
            int idx = Integer.parseInt(request.getParameter("title"));
            
            BufferedReader reader = null;
            
            // songs 디렉토리의 파일들을 리스트형태로 가져온다.
            String filePath = application.getRealPath("/WEB-INF/songs/");
            String[] files = new File(filePath).list();
            // 파라미터로 전달받은 인덱스에 해당하는 파일의 이름을 title 에 저장한다.
            String title = files[idx];
            out.println("<h3>" + title +"</h3>");
            
            // 파일 경로에 파일이름을 붙인다.
            filePath += title;

            try {
                // 입력 스트림 생성, 인코딩 값을 지정해 읽어들이기 위해 InputStreamReader 와 FileInputStream 을 사용한다.
                // FileReader 는 인코딩 방식이 정해져있다. System.getProperty("file.encoding") -> ms949 리턴
                //reader = new BufferedReader(new InputStreamReader(new FileInputStream(filePath), "utf-8")); 
                reader = new BufferedReader(new FileReader(filePath));

                // 파일을 읽어와 브라우저에 출력한다.
                while(true) {
                    String lyrics = reader.readLine();
                    if(lyrics==null) break;
                    out.println(lyrics + "<br>");
                }

            } catch(FileNotFoundException e) {
                out.println("파일이 존재하지 않습니다.");
            } catch(IOException e) {
                out.println("파일을 읽을 수 없습니다.");
            } finally {
            }
        
        %>
        <br>
        <%@ include file="Today.jsp" %>
    </body>
</html>
```

- 요청으로 넘어온 리스트의 인덱스를 받아 인덱스에 해당하는 노래 제목과 노래 가사를 출력해주는 JSP 페이지를 작성했는데
- 한글을 출력하려고 하면 인코딩방식 때문에 제대로 출력되지않는다.

```markdown
TOMBOY.txt
�궃 �뾼留덇� �뒛 踰좏뫜 �궗�옉�뿉 �뼱�깋�빐
洹몃옒�꽌 洹몃윴 嫄닿� �뒛 �뼱�졄�떎�땲源�
�엪湲� �몢�젮�썱�뜕 �슃�떖 �냽�뿉�룄
�옉�� �삁�겏�씠 �엳吏�
�궃 吏�湲� �뻾蹂듯빐 洹몃옒�꽌 遺덉븞�빐
�룺�뭾 �쟾 諛붾떎�뒗 �뒛 怨좎슂�븯�땲源�
遺덉씠 遺숈뼱 鍮⑤━ ��硫� �븞 �릺�옏�븘
�굹�뒗 �궗�옉�쓣 �쓳�썝�빐
�젇�� �슦由� �굹�씠�뀒�뒗 �옒 蹂댁씠吏� �븡怨�
李щ��븳 鍮쏆뿉 �늿�씠 硫��뼱 爰쇱졇媛��뒗�뜲
�븘�븘�븘�븘�븘
�뒳�뵂 �뼱瑜몄� �뒛 �뮮嫄몄쓬留� 移섍퀬
誘몄슫 �뒪臾쇱쓣 �꽆湲� �꼳 吏�猷⑦빐 蹂댁뿬
遺덉씠 遺숈뼱 鍮⑤━ ��硫� �븞 �릺�땲源�
�슦由� �궗�옉�쓣 �쓳�썝�빐
�젇�� �슦由� �굹�씠�뀒�뒗 �옒 蹂댁씠吏� �븡怨�
李щ��븳 鍮쏆뿉 �늿�씠 硫��뼱 爰쇱졇媛��뒗�뜲
�븘�븘�븘�븘�븘
洹몃옒 洹몃븣 �굹�뒗 �옒 紐곕옄�뿀�뼱
�슦由� �떎瑜� �젏留� �떘�븯怨�
泥좎씠 �뱾�뼱 癒쇱� �뼥�뼱�졇 踰꾨┛
�꼫�� �씠�젨 �굹�룄 �떘�븯�꽕
�젇�� �슦由� �굹�씠�뀒�뒗 �옒 蹂댁씠吏� �븡怨�
李щ��븳 鍮쏆뿉 �늿�씠 硫��뼱 爰쇱졇媛��뒗�뜲
�븘�븘�븘�븘�븘
```

- 해당 파일의 인코딩 방식은 utf-8 이고 java에서 사용하는 인코딩 방식은 MS949 이다. (System.getProperty("file.encoding")) 출력결과로 확인해보았다.)



#### 해결 방법 

- 파일 입력을 위해 PrintWriter 가 아닌 InputStreamReader 와 FileInputStream 클래스를 사용해 인코딩 방식을 지정해 사용했다.

```java
reader = new BufferedReader(new InputStreamReader(new FileInputStream(fileName), "utf-8"));
```



