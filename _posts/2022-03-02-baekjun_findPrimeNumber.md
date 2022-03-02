---
title:  "백준 1929번 소수 구하기"
excerpt: "백준 1929번 소수 구하기"

categories:
  - Java
tags:
  - Java
  - Algorithm
 
date: 2022-03-02
last_modified_at: 2022-03-02


---

## 백준 1929번 소수 구하기

- 백준 문제를 풀다가 테스트 케이스에 대한 결과는 옳게 나왔는데 틀렸다고 해서 문제 해결에 시간이 오래걸렸다.

#### 오류가 발생한 부분

```java
for(int i=2;i<N+1;i++) {
    for(int j=i*i;j<=N;j+=i) {
        if(j<0) break;
        A[j] = 1;
    }
}
```

- 안쪽 for 문의 j를 i*i로 초기화해서 일정 값을 넘어가면 int 범위를 넘어서 오버플로우가 발생하고  j가 음수가 돼서 ArrayIndexOutOfBounds 예외가 발생했다.
- 예외 발생을 막기위해서 안쪽 if문을 추가해 j가 음수가되면 반복문을 빠져나가도록했다.
- 결과는 틀렸다.

#### 문제 해결을 위한 첫번째 시도

```java
for(int i=2;i<N+1;i++) {
    for(long j=(long)i*i;j<=N;j+=i) {
        A[(int)j] = 1;
    }
}
```

- 오버플로우가 발생하는 순간에 i*i 값은 이미 N의 값을 초과해서 조건문을 추가해 j가 음수가되면 반복문을 빠져나가게했는데 틀렸다.
- 그래서 내 생각이 틀린 것같아 오버플로우를 막아보자라는 생각으로 보기 안좋지만 j를 long으로 선언했다.
- 맞긴 맞았지만 시간이 오래걸렸다. (시간 초과는 발생하지 않았다.)

#### 문제 해결을 위한 두번째 시도

```java
for(int i=2;i<N+1;i++) {
    for(int j=i+i;j<=N;j+=i) {
        A[j] = 1;
    }
}
```

- j를 long으로 선언하고 i*i로 초기화 하는 과정에서 시간이 오래걸리는 건줄 알고  i+i로 수정했다. 

- 실행시간이 줄어들지 않았다.
- 에라토스테네스의 체 알고리즘을 사용해서 N이하의 소수를 모두 찾았고 다시한번 반복문을 통해 배열을 탐색하며 소수인 값들을 출력했다.
- 탐색을 한번으로 줄였다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int[] A;
    static int N, M;

    public static void main(String[] args) throws IOException {
        input();
        solution();
    }
    public static void solution() {
        StringBuilder sb = new StringBuilder();
        A = new int[1000001];
        A[1] = 1; // 1 은 소수가 아니다.

        //에라토스테네스의 체 알고리즘을 사용해 소수가 아닌 인덱스에는 1을 넣는다.
        //알고리즘을 제대로 구현했는지는 모르겠다.
        for(int i=2;i<N+1;i++) {
            // i 가 합성수일 때 continue;
            // i 의 배수들을 찾아 1로 배열의 값을 바꾸기 때문에 
            // i 번째 원소가 1이라면 탐색할 필요가없다.
            if(A[i]==1) continue;

            // i 가 소수이면서 M 보다 크면 sb에 append
            if(i >= M)
                sb.append(i).append("\n");

            // 합성수는 제외
            for(int j=i+i;j<=N;j+=i) {
                A[j] = 1;
            }
        }
        //출력
        System.out.println(sb);
    }
    public static void input() throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(bf.readLine());
        M = Integer.parseInt(st.nextToken());
        N = Integer.parseInt(st.nextToken());
    }
}
```

