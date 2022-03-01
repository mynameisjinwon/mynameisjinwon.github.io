---
title:  "빠른 입력을 위한 BufferedReader 클래스"
excerpt: "BufferedReader 클래스를 사용해보자"

categories:
  - Java
tags:
  - Java
 
date: 2022-03-01
last_modified_at: 2022-03-01

---

## 빠른 입력을위한 BufferedReader 클래스

- 알고리즘 문제를 풀다가 시간초과가 발생했다. [두 용액](https://www.acmicpc.net/problem/2470)
- 시간 절약을 위해 이분 탐색 알고리즘을 사용했지만 시간초과가 발생해
- Scanner 보다 빠른 입력을 위해 BufferedReader 클래스를 사용했다.

#### BufferedReader 클래스

- 입력을 받는데 버퍼를 이용하기 때문에 입력에 대한 처리가 빠르다.

> ##### 버퍼를 거치는 과정이 추가됐는데 왜 더 빠를까?
>
> 버퍼를 사용하지 않는 입력은 입력이 들어오면 들어올 때마다 프로그램에 전달된다.
> 버퍼를 사용해 버퍼가 가득차거나 개행문자가 나타나면 버퍼의 내용을 한번에 전달한다.
> 하드디스크의 속도가 느리기 때문에 데이터 입출력에는 시간이 많이 걸린다. 
> 그래서 입력이 있을 때 마다 전달하는 것보다 모아서 한번에 전달하는 것이 더 효율적이고 빠르다.

#### Scanner 와의 차이점

- Scanner는 띄어쓰기, 개행을 기준으로 입력을 구분해 따로 가공할 필요가 없다.
- BufferedReader는 개행만을 기준으로 입력을 구분하고 입력된 데이터가 String으로 고정되기 때문에 가공이 필요하다.

#### 사용방법 

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class BufferedReaderTest {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		//readeLine()은 예외가 발생할 수 있기 때문에 IOException을 throw 해줘야한다.
        int num = Integer.parseInt(bf.readLine());
    }
}
```

#### 데이터 가공

- BufferedReader 는 입력을 개행문자 단위로 받기 때문에 이를 공백 단위로 분리하기 위해서는 String.split() 메소드나 StringTokenizer를 사용한다.

```java
StringTokenizer st = new StringTokenizer(bf.readLine());
for(int i=1;i<N+1;i++) {
    A[i] = Integer.parseInt(st.nextToken());
}
```



## 참고자료

- https://jhnyang.tistory.com/92