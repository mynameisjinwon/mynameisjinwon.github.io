---
title:  "intellij 인코딩 설정하는 방법"
excerpt: "intellij 인코딩 설정을 해보자"

categories:
  - Java
tags:
  - Java
  - intellij
 
date: 2022-02-26
last_modified_at: 2022-02-26
---

## intellij 인코딩 설정 

1. `File` 탭 - `Setting`

   - `File Encoding` 검색 
   - `File Encoding` 창에서 `Project Encoding` - **UTF-8**로 설정
   - `Properties Files` - Default encoding for properties files - **UTF-8**로 설정 
   - Apply

   

2. `Help` 탭 - `Find Action`

   - edit custom vm option 검색 후 클릭
   - 파일 마지막에 **-Dfile.encoding=UTF-8** 추가

   

3. intellij 재실행



##### Java를 통해 HTTP 통신을 할 경우에는 반드시 인코딩을 UTF-8로 설정해야한다.
