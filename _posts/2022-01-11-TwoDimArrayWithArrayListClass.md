---
title:  "ArrayList로 2차원배열을 선언해보자"
excerpt: "2차원배열을 ArrayList로 선언해보자"

categories:
  - java
tags:
  - java
  - DataStructure
 
date: 2022-01-11
last_modified_at: 2022-01-11
---
## 2차원배열

알고리즘 문제를 풀다가 ArrayList를 사용해 2차원 배열을 구현하는 풀이를 보았다.

ArrayList를 사용해 2차원 배열을 구현하는 방법을 알아보았다.

#### 일반적인 2차원 배열을 선언해보자

```java
//2차원 배열
int[][] arr2d = new int[][] {
    {1,2,3},
    {4,5,6}
};
for(int i=0;i<arr2d.length;i++) {
    for(int j=0;j<arr2d[i].length;j++)
        System.out.print(arr2d[i][j] + " ");
    System.out.println();
}
```



#### ArrayList 클래스를 활용해 2차원 배열을 선언해보자

##### 1. ArrayList를 선언 할 때 <>안에 wrapper 클래스가 아닌 ArrayList클래스를 삽입(?)한다.

String 객체를 저장하는 ArrayList를 저장하는  ArrayList가 생성된다.

```java
//String 객체를 저장하는 ArrayList 를 저장하는 ArrayList
ArrayList<ArrayList<String>> strs = new ArrayList<ArrayList<String>>();
ArrayList<String> str1 = new ArrayList<String>();
str1.add("hello1");
str1.add("world1");

ArrayList<String> str2 = new ArrayList<String>();
str2.add("hello2");
str2.add("world2");

strs.add(str1);
strs.add(str2);
System.out.println(strs.toString());
```



##### 2. ArrayList를 선언할 때 <>안에 정수형 변수를 담는 배열을 삽입한다.

정수형 변수 배열을 담는 ArrayList가 생성된다.

```java
//정수 배열을 담는 ArrayList
ArrayList<Integer[]> arr = new ArrayList<>();
arr.add(new Integer[]{1, 2, 3, 4});
arr.add(new Integer[]{5, 6, 7, 8});

for(int i=0;i<arr.size();i++) {
    for(int j=0;j<arr.get(i).length;j++) {
        sb.append(arr.get(i)[j]).append(' ');
    }
    sb.append('\n');
}
System.out.println(sb.toString());
```

ArrayList에 저장되는 건 정수형 1차원 배열이기 때문에

arr.get(0)은 정수형 배열이다(?)

arr.get(0)은 배열이기 때문에 배열 인스턴스의 멤버변수 length를 갖는다.



##### 3. 정수를 저장하는 ArrayList의 객체 배열을 생성한다.

nums는 정수형 변수를 저장하는 ArrayList를 담는 객체 배열이다.

객체 배열을 선언과 동시에 초기화 하기위해선 배열의 크기를 지정해 줘야한다.

```java
//ArrayList 객체 배열 생성
ArrayList<Integer>[] nums = new ArrayList[2] ;
ArrayList<Integer> num1 = new ArrayList<Integer>();
num1.add(1);
num1.add(2);

ArrayList<Integer> num2 = new ArrayList<Integer>();
num2.add(3);
num2.add(4);

nums[0] = num1;
nums[1] = num2;
for(int i=0;i<2;i++)
    System.out.println(nums[i].toString());
```

ArrayList를 사용해 2차원 배열을 구현할 때 어떤 방식을 사용하든지 ArrayList에 어떤 자료형의 데이터가 저장되는지 생각을 하면 다루기 편하다.



```java
ArrayList<Integer[]> arr = new ArrayList<>();
```

위의 경우에 arr에는 정수형 변수를 담는 1차원 배열이 저장된다. arr.get(i)는 배열을 의미한다.



```java
ArrayList<ArrayList<String>> strs = new ArrayList<ArrayList<String>>();
```

strs는 String 인스턴스를 담는 ArrayList가 저장되는 ArrayList이다.

strs.get(i)는 String 인스턴스를 담는 ArrayList이다.



```java
ArrayList<Integer>[] nums = new ArrayList[2] ;
```

nums는 정수형 변수를 담는 ArrayList 인스턴스를 저장하는 배열이다.

nums[i]에는 정수형 1차원 배열(ArrayList)이 저장된다.



가변길이의 배열이 필요해 ArrayList를 사용하는 거니까 3번째 방법은 효율적이지 않은것 같다? 

하지만 배열의 길이를 지정할 필요가 있는 경우에는 굳이 가변길이 배열을 사용할 필요가없으니 길이를 지정해 주는 3번째 방법을 사용하는 것이 더 좋은 경우가 있을 것 같다. ArrayList의 크기를 증가시키는데도 연산이 필요하니까 



