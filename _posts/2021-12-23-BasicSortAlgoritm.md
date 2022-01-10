---
title:  "기본 정렬에 대해 알아보자"
excerpt: "알고리즘 공부 with Java"

categories:
  - Java
tags:
  - Java
  - Algorithm
  
date: 2021-12-23
last_modified_at: 2021-12-23


---

# 기본 정렬 알고리즘

버블 정렬, 선택 정렬, 삽입 정렬



### 버블 정렬 BubbleSort

배열의 인접한 데이터끼리 비교해 더 큰값을 뒤로 보내는 정렬

```java
for(i=0~n-1) {
	for(j=0~n-1-i) {
		if(arr[j] > arr[j+1])
			swap(arr[j], arr[j+1])
	}
}
```



#### 버블 정렬 구현하기

```java
import java.util.ArrayList;
import java.util.Collections;

public class BubbleSort {
    public ArrayList<Integer> bubbleSort(ArrayList<Integer> dataList) {
		int size = dataList.size()-1;
        for(int i=0;i<size;i++) {
            for(int j=0;j<size-1-i;j++) {
				if(dataList.get(j) > dataList.get(j+1)) {
                    Collections.swap(dataList, j, j+1);
                }
            }
        }
        return dataList;
    }
}
```

___

### 선택 정렬 SelectionSort

배열의 앞쪽부터 최소값을 찾아 앞으로 가져오는 정렬

```java
for(i=0~n-1) {
	for(j=i~n-1) {
		if(findMin()) {
			swap(i, min);
		}
	}
}
```



#### 선택 정렬 구현하기

```java
import java.util.ArrayList;
import java.util.Collections;

public class SelectionSort {
	public ArrayList<Integer> sort(ArrayList<Integer> dataList) {
		int size = dataList.size()-1;
        int min;
        for(int i=0;i<size;i++) {
            min = i;
            for(int j=i+1;j<n-1;j++) {
                if(dataList.get(min) > dataList.get(j)) {
                    min = j;
                }
            }
            Collections.swap(dataList, min, i);
        }
        return dataList;
    }
}
```

___

### 삽입 정렬 InsertionSort

데이터를 적당한 위치에 삽입해 정렬하는 알고리즘

```java
for(i=0~n-1) {
    for(j=i+1~1) {
        if(arr[j-1] > arr[j]) {
            swap(arr[j-1], arr[j])
        }
    }
}
```



#### 삽입 정렬 구현하기

```java
import java.util.ArrayList;
import java.util.Collections;

public class InsertionSort {
	public ArrayList<Integer> sort(ArrayList<Integer> dataList) {
        int size = dataList.size()-1;
        for(int i=0;i<size;i++) {
            for(int j=i+1;j>0;j--) {
                if(dataList.get(j-1) > dataList.get(j) ) {
                    Collections.swap(dataList, j-1, j);
                }
            }
        }
        return dataList;
    }
}
```

---

#### 시간복잡도 

기본 정렬 알고리즘의 시간복잡도는 O(n^2)이다.
