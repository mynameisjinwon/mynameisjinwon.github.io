---
title:  "StringBuilder"
excerpt: "StringBuilder를 알아보자" 

categories:
  - Java
tags:
  - Java
 
date: 2021-12-30
last_modified_at: 2021-12-30

---

# String

자바에서 문자열은 객체로 존재한다. 

같은 문자열을 서로다른 String 참조변수에 저장하면 두개의 문자열 객체를 생성하지 않고

하나의 객체를 생성한 뒤 다른 하나는 이전에 생성한 객체를 참조한다.

객체 생성을 제한해 성능을 향상시키기 위해서 이러한 방식을 사용한다.

```java
String str1 = "hello world";
String str2 = "hello world";

if(str1==str2) 
    System.out.println("both are same");

```

만약 str1을 사용해 저장된 문자열을 변경할 수 있으면, str2는 문자열이 변경된 줄 알 수 없다. 

이러한 문제의 발생을 막기 위해 자바는 문자열의 변경을 허락하지 않는다.



### StringBuilder 

String 클래스는 변경 불가능한 문자열을 표현하는데 사용하지만

StringBuilder 클래스는 변경이 가능한 문자열의 표현을 위한 클래스이다.

```java
String str1 = "hi ";
String str2 = "I'm jinwon";
String str3 =  str1 + str2; // +연산의 결과로 두 문자열을 합친 새로운 문자열 객체를 생성한다.

StringBuilder sb = new StringBuilder();
sb.append("hi ");
sb.append("I'm jinwon");
System.out.println(sb.toString());
```

String 객체에 대해 + 연산을 수행하면 StringBuilder 객체가 생성되고 해당 문자열에 대해 append()메소드를 실행한 뒤 toString()메소드를 사용해 새로운 문자열 객체를 만든다. 그렇기 때문에 문자열을 대상으로하는 + 연산은 성능저하를 일으킬 수 있다.

그래서 변경이 필요한 문자열을 다루는 경우에는 StringBuilder 클래스를 사용하는 것이 더 효율적이다.



#### StringBuilder에 저장되는 건 무엇?

StringBuilder 객체가 저장된다?



