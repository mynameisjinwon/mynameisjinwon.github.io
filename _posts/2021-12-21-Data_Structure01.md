---
title:  "자료구조를 공부해보자"
excerpt: "자료구조 공부 with Java"

categories:
  - Java
tags:
  - Java
  - DataStructure
  
date: 2021-12-21
last_modified_at: 2021-12-21

---

### 배열 Array

같은 자료형의 데이터를 효율적으로 관리하기 위해 사용하는 자료구조

인덱스를 통해 배열의 요소에 대한 빠른 접근이 가능하다.

길이가 고정되어있다. 

```java
int arr1 = new int[10];
int arr2 = {5, 4, 3 ,2, 1};
int arr3 = {1, 2, 3, 4, 5};
```



### ArrayList 클래스 

가변길이의 배열을 다룰 수 있는 기능을 제공하는 클래스 

```java
import java.util.ArrayList;

List<Integer> list1 = new ArrayList<Integer>();
ArrayList<Integer> list2 = new ArrayList<Integer>();	
//List클래스 타입의 참조변수 list1은 List클래스를 상속하는 다른 클래스타입 객체를 담을 수 있지만 ArrayList클래스 타입인 list2는 ArrayList클래스 타입 객체만 담을 수 있다.
//데이터 삽입
list1.add(1);
list1.add(2);
//데이터 삭제
list1.remove(0); // 0은 인덱스 번호
//데이터 갱신
list1.set(0, 5); // 0번 인덱스의 데이터를 5로 바꿈, list에 데이터가 없으면 오류
```

ArryaList는 객체만을 다루기 때문에 premitive 자료형이 아닌 wrapper 클래스를 사용해야한다.

*Array는 premitive 타입 뿐만 아니라 객체도 담을 수 있다.* 



### 큐 Queue

가장 많이 쓰이는 자료구조 중 하나 (멀티태스킹을 위한 프로세스 스케쥴링을 구현하는데 사용한다.)

FIFO(First In First Out)  선입선출, 가장 먼저 들어온 데이터가 가장 먼저 나간다.



**enqueue : 큐에 데이터를 삽입하는 기능**

**dequeue : 큐에 데이터를 삭제하는 기능**

Java는 Queue 클래스를 제공한다. Queue 클래스 사용을 위해서 LinkedList클래스가 필요하다.

``` java
import java.util.LinkedList;
import java.util.Queue;

Queue<Integer> que1 =new LinkedList<Integer>();
Queue<String> que2 = new LinkedList<String>();	

//enqueue 
que1.add(1);
que1.offer(2);
//dequeue
que1.poll();
que1.remove();
```

