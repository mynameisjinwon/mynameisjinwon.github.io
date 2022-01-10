---
title:  "백트래킹 알고리즘을 알아보자"
excerpt: "백트래킹 알고리즘을 공부해보자" 

categories:
  - Algorithm
tags:
  - Java
  - Algorithm
 
date: 2021-12-29
last_modified_at: 2021-12-29



---

# 백트래킹

퇴각검색이라고도 한다.

제약 조건 만족문제에서 해를 찾기위한 전략이다.

고려할 수 있는 모든 경우의 수(후보군)를 상태공간트리로 표현하고 이 트리를 깊이 우선 탐색 기법으로 탐색한다. 특정 경로로 탐색을하다 해를 찾을 수 없으면 멈추고 해당경로를 버리고(Pruning) 처음으로 돌아와 다른 경로를 탐색한다. 

- 조건검사 Promising : 해당 경로가 조건에 맞는지 검사하는 기법

- 가지치기 Pruning : 해를 찾을 수 없는 경로를 포기해 탐색 시간을 절약하는 기법



#### N Queen 문제

대표적인 백트래킹 문제

N*N 크기의 체스판에서 N개의 퀸을 서로 공격할 수 없는 위치에 배치하는 경우를 찾는 문제

- Pruning : 탐색하는 과정에서 앞선 행에 배치한 퀸으로 인해 다음행에 퀸을 배치할 수 없는 경우에 더 이상 퀸을 배치하지 않고 이전 행의 퀸의 자리를 바꾼다.

- Promising : 한 행에는 하나의 퀸만 배치할 수 있어 별도로 체크가 필요하지 않다. 수직, 대각선은 체크가 필요하다.

- 수직체크 : 앞선 행의 퀸이 배치된 x좌표와 같은 위치에는 더 이상 퀸을 배치 할 수없다.
- 대각선체크 : 현재 탐색중인 좌표의 x, y값과 앞선 행의 퀸이 위치한 x, y값의 차의 절대값이 서로 같으면 대각선 위치에 있다는 뜻이므로 해당 위치에는 퀸을 배치할 수 없다. 



#### 백트래킹 기법을 사용해 N Queen 문제의 해를 찾아보자

```java
import java.util.ArrayList;

public class NQueen {
    //조건에 맞는지 검사
    public boolean isAvailable(ArrayList<Integer> candidate, Integer currentCol) {
        //candidate에 저장된 데이터의 수는 지금까지 탐색한 행의 수와같다.
        //candidate.size() 가 2이면 1행과 2행에서 후보군을 찾아 저장했다는 뜻
        Integer currentRow = candidate.size();
        for(int i=0;i<currentRow;i++) {
            //promising
            // candidate.get(i) == currentCol -> 같은 열
            //Math.abs(candidate.get(i)-currentCol==currentRow-i -> 대각선에 위치
            if((candidate.get(i)==currentCol)||
               (Math.abs(candidate.get(i)-currentCol)==currentRow-i))  {
                return false;
            }
        }
        return true;
    }
    public void dfsFunc(Integer N, Integer currentRow, 
                        ArrayList<Integer> currentCandidate) {
        //탐색이 끝났으면 현재까지 찾은 해를 출력한다.
        if(currentRow == N) {
            System.out.println(currentCandidate);
            return;
        }
        //같은행의 다른 열 탐색
        for(int i=0;i<N;i++) {
            //조건 탐색
            if(this.isAvailable(currentCandidate, i)==true) {
                //조건에 일치하면 해당 위치의 x좌표를(열정보) 리스트에 저장한다. 
                currentCandidate.add(i);
                //다음 행으로 이동해 같은 작업을 반복한다.
                this.dfsFunc(N, currentRow+1, currentCandidate);
                //가지치기
                // 
                currentCandidate.remove(currentCandidate.size()-1);
            }
        }
    }
}
```
