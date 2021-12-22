---
title:  "힙을 알아보자"
excerpt: "자료구조 공부 with Java"

categories:
  - Java
tags:
  - Java
  - DataStructure
  
date: 2021-12-22
last_modified_at: 2021-12-22

---

# 힙 Heap

최대값과 최소값을 빠르게 찾기위해 고안된 '완전이진트리'

> 완전 이진트리 : 노드를 삽입할 때 최하단의 왼쪽부터 차례대로 채우는 이진트리

배열의 경우 최대값, 최소값을 찾는데 걸리는 시간은 O(n) 

힙의 경우는 트리의 높이만큼 걸린다. O(logn)

우선순위 큐와 같이 최대값, 최소값을 빠르게 찾아야하는 경우에 사용된다.



힙이 되는 조건 :

각 노드들은 자신의 자식 노드보다 크거나 같다.

완전이진트리이다.

최대/최소값을 찾는것은 빠르지만 노드를 삽입, 삭제하는데 복잡한 로직이 필요하다.

데이터 삽입 : 최하단에 데이터를 삽입하고 부모노드와 크기를 비교하며 자리를 바꾼다.

데이터 삭제 : 힙의 경우 루트노드를 삭제하는 것이 일반적이다. 루트노드를 삭제하고 최하단 노드를 루트노드 자리로 옮기고 자식노드와 크기를 비교하며 자리를 바꾼다.



일반적으로 힙은 배열을 사용해 구현한다.

부모노드의 인덱스 = 자식노드의 인덱스 / 2

왼쪽 자식노드의 인덱스 = 부모노드의 인덱스 * 2

오른쪽 자식노드의 인덱스 = 부모노드의 인덱스 * 2 + 1



#### Collections 패키지 

> java.util.Collections;

swap 메소드를 제공한다. Collections.swap(List list, int a, int b); 



```java
import java.util.ArrayList;
import java.util.Collections;

public class Heap {
    public ArrayList<Integer> heapArray = null;
    
    public Heap(Integer data) {
        heapArray = new ArrayList<Integer>();
        
        heapArray.add(null);
        heapArray.add(data);
    }
    public boolean moveUp(Integer idx) {
        if(idx<=1) 
            return false;
        if(heapArray.get(idx) > heapArray.get(idx/2)) 
            return true;
        else
            return false;
    }
    public boolean moveDown(Integer idx) {
        Integer left = idx*2;
        Integer right = idx*2+1;
        //자식이 없을 때
        if(left>heapArray.size()-1) 
            return false;
        //오른쪽 자식만 없을 때
        if(right>=heapArray.size()-1) {
            //왼쪽 자식이 더 클 때 
            if(heapArray.get(idx) < heapArray.get(left)) {
                return true;
            } else 
                return false;
        //둘다 있을 때
        } else { 
            if(heapArray.get(left) > heapArray.get(right)) {
                if(heapArray.get(idx) < heapArray.get(left)) {
                    return true;
                } else 
                    return false;
            } else {
                if(heapArray.get(idx) < heapArray.get(right)) {
                    return true;
                } else 
                    return false;
            }
        }
    }
    public boolean insert(Integer data) {
        if(heapArray==null) {
            heapArray = new ArrayList<Integer>();
            
            heapArray.add(null);
            heapArray.add(data);
            return true;
        } 
        heapArray.add(data);
        int idx = heapArray.size()-1; // 마지막 노드의 인덱스 리턴
        //현재 노드의 값이 부모노드의 값보다 작으면
        while(this.moveUp(idx)) {
            Collections.swap(heapArray, idx, idx/2);
            idx /= 2;
        }
        return true ;
    }
    public Integer pop() {
        if(heapArray==null) {
            System.out.println("삭제할 데이터가 없습니다.");
            return null;
        } 
        int idx = heapArray.size()-1;
        Collections.swap(heapArray, 1, idx); // 마지막 노드를 루트 노드로 옮긴다.
        int delNode = heapArray.remove(idx); // 루트노드를 삭제한다.
        idx = 1;
        
        while(moveDown(idx)) {
            int left = idx*2;
            int right = idx*2+1;
                
            //자식이 하나일 때
            if(right>=heapArray.size()-1) {
                if(heapArray.get(idx) < heapArray.get(left) ) {
                    Collections.swap(heapArray, idx, left);
                    idx = left;
                } 
            //자식이 둘일때
            } else {
                //왼쪽 자식이 더 크고 현재 노드보다도 클 때
                if(heapArray.get(left) > heapArray.get(right) ) {
                    if(heapArray.get(idx) < heapArray.get(left)) {
                        Collections.swap(heapArray, idx, left);
                        idx = left;
                    } 
                } else {
                    if(heapArray.get(idx) < heapArray.get(right) ) {
                        Collections.swap(heapArray, idx, right);
                        idx = right;
                    } 
                }
            }
        }
        return delNode;
    }
}
```
