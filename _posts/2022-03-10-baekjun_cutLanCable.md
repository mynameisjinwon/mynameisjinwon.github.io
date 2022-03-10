---
title:  "백준 1654번 랜선 자르기"
excerpt: "백준 1654번 랜선 자르기"

categories:
  - Baekjun
tags:
  - Java
  - Algorithm
 
date: 2022-03-10
last_modified_at: 2022-03-10


---

## 백준 1654번 랜선 자르기

#### 오류가 발생한 부분

##### 결정문제를 풀기위한 이분탐색 결정 문제가 true 가 되는 최대 값을 찾는다.

```java
public static void solution() {
    int l=1, r=Integer.MAX_VALUE, result = 0;

    while(l<=r) {
        int mid = (l+r)/2;
        if(determination(mid)) {
            result = mid;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    System.out.println(result);
}
```

- 문제에서 입력된 랜선의 최대 길이가 2^31-1 (2147483647) 이여서 int형 변수를 사용해 결정문제의 매개변수를 구했다.
- 그런데 2147483647는 int 의 최대값이라서 이분 탐색을 위한 변수 mid 를 구할 때 l+r 연산에서 오버플로우가 발생한다. 

#### 문제 해결을 위한 시도1

```java
public static void solution() {
    long l=1, r=Integer.MAX_VALUE;
    int result = 0;

    while(l<=r) {
        int mid = (int)(l+r)/2;
        if(determination(mid)) {
            result = mid;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    System.out.println(result);
}
```

- 연산에 사용되는 l과 r을 long 형 변수로 선언했다.
- l 과 r 의 최댓값이 int의 최댓값이므로 그 둘을 더한 뒤 2로 나누면 int형 변수에 담을 수 있다.
- 그래서 mid 또한 int로 선언했다.

#### 문제 해결을 위한 시도2

```java
 public static boolean determination(long L) {
     int sum=0, cnt=0;

     // L 의 길이로 A[i]길이의 랜선을 잘랐을 때 나오는 랜선의 개수를 cnt 에 저장한다.
     for(int i=0;i<N;i++) {
         cnt += A[i]/L;
     }

     // K 개 이상의 랜선을 만들 수 있다면 true 를 리턴한다.
     return K <= cnt;
 }
public static void solution() {
    // 랜선의 최대길이는 2^31 - 1 이다. ( = Integer.MAX_VALUE)
    long l=1, r=Integer.MAX_VALUE;
    int result = 0;

    while(l<=r) {
        // l, r 은 모두 int 형 변수에 저장할 수 있지만 l + r 을 할때 오버플로우가 발생하므로 long 형 변수에 저장했다.
        int mid = (int)((l+r)/2);
        if(determination(mid)) {
            result = mid;
            l = (long)mid + 1;
        } else {
            r = mid - 1;
        }
    }
    System.out.println(result);
}
```

- mid 가 2147483647 일 때 l 에 값을 저장하는 과정에서 오버플로우가 발생한다.
- mid 가 2147483647 일 경우는 l과 r 모두 2147483647일 때이다. 
- 2147483647 + 1 의 결과로 오버플로우가 발생해 l 에 -2147483648 이 저장되면 l + r 이 1이 되고 mid에 0이 저장되면서 결정문제의 답을 구하는 메소드 안에서 divide by zero 예외가 발생한다.
- 그래서 오버플로우가 발생하지 않도록 mid + 1 을 l 에 저장할 때 mid 를 long 형으로 형변환 해준다.
- 근데 형변환 저렇게 할거면 그냥 mid를 long으로 선언하는게 낫지 않을까?

##### 값을 저장하는 변수의 최댓값과 연산에 사용되는 변수의 최댓값을 잘 생각해 변수를 선언하자

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N, K;
    static int[] A;

    public static void main(String[] args) throws IOException {
        input();
        solution();
    }
    public static boolean determination(long L) {
        int sum=0, cnt=0;

        // L 의 길이로 A[i]길이의 랜선을 잘랐을 때 나오는 랜선의 개수를 cnt 에 저장한다.
        for(int i=0;i<N;i++) {
            cnt += A[i]/L;
        }

        // K 개 이상의 랜선을 만들 수 있다면 true 를 리턴한다.
        return K <= cnt;
    }
    public static void solution() {
        // 랜선의 최대길이는 2^31 - 1 이다. ( = Integer.MAX_VALUE)
        long l=1, r=802;
        int result = 0;

        while(l<=r) {
// l, r 은 모두 int 형 변수에 저장할 수 있지만 l + r 을 할때 오버플로우가 발생하므로 long 형 변수에 저장했다.
            int mid = (int)((l+r)/2);
            if(determination(mid)) {
                result = mid;
                l = (long)mid + 1;
            } else {
                r = mid - 1;
            }
        }
        System.out.println(result);
    }
    public static void input() throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());

        N = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());
        A = new int[N];

        for(int i=0;i<N;i++) {
            A[i] = Integer.parseInt(bf.readLine());
        }

    }
}
```

