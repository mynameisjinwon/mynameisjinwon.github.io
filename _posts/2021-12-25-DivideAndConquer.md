---
title:  "고급 정렬알고리즘을 알아보자"
excerpt: "고급 정렬 알고리즘을 공부해보자" 

categories:
  - Algorithm
tags:
  - Java
  - Algorithm
 
date: 2021-12-25
last_modified_at: 2021-12-25


---

# 고급정렬 알고리즘



## 병합정렬 MergeSort

분할정복에 속하는 정렬 알고리즘

분할 정복(Divide and Conquer) : 문제를 나눌 수 없을 때까지 나누고 각각을 풀면서 다시 합병해 문제의 답을 얻는 알고리즘

분할 정복 알고리즘은 일반적으로 재귀용법을 사용해 구현한다.



**병합 정렬 수행 과정** 

1. 리스트를 반씩 나누어 리스트에 데이터가 하나만 있을 때까지 나눈다.
2. 각각의 부분 리스트를 합병하면서 정렬한다.



#### 병합정렬 구현하기

```java
import java.util.ArrayList;
import java.util.Arrays;

public class MergeSort {
    public ArrayList<Integer> split(ArrayList<Integer> dataList) {
		//dataList의 요소가 하나 이하일 때
        if(dataList.size()<=1)
            return dataList;
		//배열의 중간을 의미하는 변수
        int medium = dataList.size() / 2;
        
        //리스트를 반으로 나눠 저장할 리스트 생성
        ArrayList<Integer> leftList = new ArrayList<Integer>();
        ArrayList<Integer> rightList = new ArrayList<Integer>();
        
  		leftList = split(new ArrayList<Integer>(dataList.subList(0, medium)));      
        rightList = split(new ArrayList<Integer>(dataList.subList(medium, dataList.size())));
        
        return merge(leftList, rightList);
    }
    public ArrayList<Integer> merge(ArrayList<Integer> leftList, 
                                    ArrayList<Integer> rightList) {
		//왼쪽, 오른쪽 리스트의 인덱스역할을 할 변수 생성
        int leftPoint=0, rightPoint=0;
        //정렬된 리스트를 담을 리스트 생성
        ArrayList<Integer> mergeList = new ArrayList<Integer>();
        
        //각 리스트의 범위안에서 
        while(leftPoint < leftList.size() && rightPoint < rightList.size()) {
			//더 작은 값을 먼저 새로운 리스트에 저장
            if(leftList.get(leftPoint)<rightList.get(rightPoint)) {
                mergeList.add(leftList.get(leftPoint++));
            } else
                mergeList.add(rightList.get(rightPoint++));
        }
        //while문을 탈출하는 경우
        //leftPoint가 leftList.size()보다 클 때 -> leftList의 모든 값이 mergeList에 저장됨
        //아직 저장되지 않은 rightList의 값을 mergeList에 저장해야한다.
        while(rightPoint<rightList.size()){
            mergeList.add(rightList.get(rightPoint++));
        }
        //rightpoint가 rightList.size()보다 클 때 -> rightList의 모든 값이 mergeList에 저장됨
        //아직 저장되지 않은 leftList의 값을 mergeList에 저장해야한다.
        while(leftPoint<leftList.size()){
            mergeList.add(leftList.get(leftPoint++));
        }
        return mergeList;
    }
}
```



#### 병합정렬의 시간복잡도

리스트를 더이상 나눌 수 없을 때 까지 나누는데 걸리는 시간 O(logN)

나눈 리스트를 정렬하는데 걸리는 시간 O(N) 

O(logN) * O(N) = O(N*logN)





## 퀵정렬 QuickSort

분할정복에 속하는 정렬 알고리즘

합병 정렬과 다르게 리스트를 일정한 크기로 분할 하지 않는다.



**퀵 정렬 수행 과정**

1. 기준점(피벗)을 정하고 더 작은값은 왼쪽으로 큰값은 오른쪽으로 이동시킨다.
2. 피벗을 제외하고 왼쪽, 오른쪽 리스트를 대상으로 같은 작업을 반복한다.



#### 퀵 정렬 구현하기

```java
import java.util.ArrayList;
import java.util.Arrays;

public class QuickSort {
    public ArrayList<Integer> sort(ArrayList<Integer> dataList) {
        if(dataList.size()<=1) 
            return dataList;
        //정렬이 필요한 리스트의 첫번째 데이터를 피벗으로 설정한다.
        Integer pivot = dataList.get(0);
        ArrayList<Integer> leftList = new ArrayList<Integer>();
        ArrayList<Integer> rightList = new ArrayList<Integer>();
        ArrayList<Integer> mergedList = new ArrayList<Integer>();
        
        //리스트를 탐색하며 데이터를 피벗과 비교한다.
        for(int i=1;i<dataList.size();i++) {
			//피벗보다 작은 값을 왼쪽 리스트에 저장한다.
            if(pivot > dataList.get(i)) {
                leftList.add(dataList.get(i));
			//피벗보다 큰 값을 오른쪽 리스트에 저장한다.
            } else {
                rightList.add(dataList.get(i));
            }
        }
        
        //정렬된 리스트를 하나의 리스트에 담는다.
        //addAll() 메소드를 사용하면 리스트를 한번에 저장할 수 있다.
        mergedList.addAll(this.sort(leftList));
        mergedList.add(pivot);
        mergedList.addAll(this.sort(rightList));
        
        //정렬된 리스트를 리턴한다.
        return mergedList;
    }
}
```



#### 퀵 정렬의 시간복잡도

퀵 정렬의 시간복잡도는 병합 정렬의 것과 동일한 O(N*logN)이다. 

하지만 이미 정렬된 데이터에서 피벗값이 가장 큰 값 혹은 작은값일 겨우 모든 데이터를 비교하게되어 O(N^2)이된다.
