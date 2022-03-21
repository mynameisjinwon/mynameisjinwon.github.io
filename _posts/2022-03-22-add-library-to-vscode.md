---
title:  "vscode에 외부 라이브러리를 추가해 보자"
excerpt: "vscode에 외부 라이브러리를 추가해 보자"

categories:
  - Java
tags:
  - Java
  - VSCode
 
date: 2022-03-22
last_modified_at: 2022-03-22
---

## vscode에 외부 라이브러리를 추가해 보자



- intellij 말고 VS Code 를 사용해서 servlet 을 공부하고있다.
- 메모장보다는 이쁘고 intellij 보다는 가벼워서 선택을 했는데
- 외부 라이브러리 추가를 하지 않으면 에러가 발생해서 불편하다.



![import_error_on_vscode](https://user-images.githubusercontent.com/32839363/159291367-69cd140c-876a-409c-b785-5a244732fc31.png)



#### 외부 라이브러리 추가하기

- VS Code 상에 좌측 하단에 Java Projects 항목을 선택하면 현재 프로젝트에 대한 정보가 나온다.

![import_error_on_vscode2](https://user-images.githubusercontent.com/32839363/159292068-727dcbd4-38e8-4413-857a-81a31279184e.png)

- 그 중 Referenced Libraries 의 우측에 있는 + 를 누르면 탐색창이 뜨면서
- 라이브러리 파일을 선택할 수 있다.
- 원하는 .jar 파일을 선택해 추가하면된다.



#### 추가된 모습과 결과

![import_error_on_vscode3](https://user-images.githubusercontent.com/32839363/159292402-f717dac3-54d2-4e3d-8769-994b45e48319.png)

![import_error_on_vscode4](https://user-images.githubusercontent.com/32839363/159292411-331d32ac-52e4-4910-877a-c7eff18e25d5.png)

- 에러가 사라져서 기분이 좋다.



#### 참조 

[Add JAR files to Java Project using Visual Studio Code](https://youtu.be/3Qm54znQX2E) 

