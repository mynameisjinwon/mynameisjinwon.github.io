---
title:  "백준 2343번 기타 레슨"
excerpt: "백준 2343번 기타 레슨"

categories:
  - Baekjun
tags:
  - Java
  - Algorithm
 
date: 2022-03-10
last_modified_at: 2022-03-10


---

## 백준 2343번 기타 레슨

#### 오류가 발생한 부분

```java
for(int i=0;i<A.length;i++) { //min은 minute
    if((sum + A[i]) > min) {
        cnt++;
        sum = A[i];
    } else {
        sum += A[i];
    }
}
```

- sum 에 배열 A의 i 번째 원소를 더했을 때 블루레이의 길이인 min 을 초과하면 블루레이의 개수를 뜻하는 cnt 값을 증가시키고 sum에 A[i]를 저장했다.
- min 값이 A[i] 보다 작을 때도 sum + A[i] 만을 확인하기 때문에 배열 A 에 min 값보다 큰 원소가 들어있어도 아무 문제없이 동작하는 것이 문제다.

#### 문제 해결

```java
for(int i=0;i<A.length;i++) {
    if(A[i] > min) return false;
    if((sum + A[i]) > min) {
        cnt++;
        sum = A[i];
    } else {
        sum += A[i];
    }
}
```

- 배열의 원소보다 작은 min 값이 전달되면 바로 false를 리턴하도록 변경했다.

##### 	정답이 될 수 있는 값의 최댓값을 무시해서 틀렸다.



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N, M;
    static int[] A;

    public static void main(String[] args) throws IOException {
        input();
        solution();
    }
    public static boolean determination(long min) {
        int sum=0, cnt=1;

        // 강의 시간의 합이 min 을 초과하지 않게 sum 에 담는다.
        // sum 이 min 값을 초과하면 cnt (블루레이의 개수) 값을 증가시킨다.
        for(int i=0;i<A.length;i++) {
            if(A[i] > min) return false;
            if((sum + A[i]) > min) {
                cnt++;
                sum = A[i];
            } else {
                sum += A[i];
            }
        }
        // 가장 긴 강의 시간보다 min 값이 작거나 같아야한다. min (minute 의 약자임)
        // min 보다 긴 강의 시간이 있으면 블루레이에 담을 수 없기 때문
        return M >= cnt;
    }
    public static void solution() {
        long l=0, r=10000000000L, result = 0;

        while(l<=r) {
            long mid = (l+r)/2;
            if(determination(mid)) {
                result = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        System.out.println(result);
    }
    public static void input() throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        A = new int[N];
        st = new StringTokenizer(bf.readLine());
        for(int i=0;i<N;i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }
    }
}
```

