---
title:  "Variable expected 에러"
excerpt: "Variable expected 에러"

categories:
  - java
tags:
  - java
  - DataStructure
  - Error
 
date: 2022-01-13
last_modified_at: 2022-01-13
---
## Variable expected 

ArrayList로 2차원 배열을 구현해 알고리즘 문제를 푸는데 에러가 발생했다.

```java
static ArrayList<ArrayList<String>> arr = new ArrayList<ArrayList<String>>();

public static void input() {
    N = sc.nextInt();

    for(int i=0;i<N;i++) {
        String str = sc.next();
        int len = str.length();
        arr.get(len) = new ArrayList<String>(Arrays.asList(str)); // variable expected
    }
}
```

문자열을 입력받아 문자열의 길이에 해당하는 행에 입력받은 문자열을 저장하기위해

```java
arr.get(len).add(str);
```

이런 코드를 짰는데 out of bounds exception이 발생했다. 

add() 메소드를 통해서 비어있는 행에 접근을 하려해서 그런것이 아닐까?

그래서 arr.get(len) = new ArrayList<String>~~ 을 통해 비어있는 행에 ArrayList를 할당하려고 했지만

Variable expected 에러가 발생했다.

해당 에러는 메소드의 호출결과로 리턴되는 값에 직접 할당하려고 해서 발생한다.

메소드의 호출 결과로 리턴되는 값에는 새로운 값을 할당할 수 없다.

[링크].(https://stackoverflow.com/questions/47741603/variable-expected-java-vector)

