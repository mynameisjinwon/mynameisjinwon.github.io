---
title:  "트리를 순회해보자"
excerpt: "트리를 순회하는 메소드를 만들어보자"

categories:
  - Java
tags:
  - Java
  - DataStructure
  
date: 2021-12-24
last_modified_at: 2021-12-24  

---

# 트리 순회

트리 순회는 루트 노드를 방문하는 순서에 따라 나뉜다.

전위 순회 : 루트 노드를 가장 먼저 방문하는 방법

중위 순회 : 왼쪽 자식을 먼저 방문하고 루트 노드를 방문하는 방법

후위 순회 : 루트 노드를 가장 마지막에 방문하는 방법



#### 재귀용법을 이용해 트리 순회 메소드를 구현해 보았다.

```java
//전위 
public void rootFirstTour(Node curNode) {
    if(curNode==null) {
        return;
    }
    System.out.print(curNode.getValue()+ " ");
    //양쪽 자식이 다 있으면
    if(curNode.left!=null && curNode.right!=null) {
        rootFirstTour(curNode.left);
        rootFirstTour(curNode.right);
        //왼쪽 자식만 있으면
    } else if(curNode.left!=null && curNode.right==null) {
        rootFirstTour(curNode.left);
        //오른쪽 자식만 있으면
    } else if(curNode.left==null && curNode.right!=null) {
        rootFirstTour(curNode.right);
        //자식이 없으면
    } else {
        return;
    }
    return;
}
//중위 순회 
public void leftFirstTour(Node curNode) {
    if(curNode==null) {
        return;
    }
    //양쪽 자식이 다 있으면
    if(curNode.left!=null && curNode.right!=null) {
        leftFirstTour(curNode.left);
        System.out.print(curNode.getValue()+ " ");
        leftFirstTour(curNode.right);
        //왼쪽 자식만 있으면
    } else if(curNode.left!=null && curNode.right==null) {
        leftFirstTour(curNode.left);
        //오른쪽 자식만 있으면
    } else if(curNode.left==null && curNode.right!=null) {
        leftFirstTour(curNode.right);
        //자식이 없으면
    } else {
        System.out.print(curNode.getValue()+ " ");
        return;
    }
    return;
}
//후위 순회
public void rootLastTour(Node curNode) {
    if(curNode==null) {
        return;
    }
    //양쪽 자식이 다 있으면
    if(curNode.left!=null && curNode.right!=null) {
        rootLastTour(curNode.left);
        rootLastTour(curNode.right);
        System.out.print(curNode.getValue()+ " ");
        //왼쪽 자식만 있으면
    } else if(curNode.left!=null && curNode.right==null) {
        rootLastTour(curNode.left);
        //오른쪽 자식만 있으면
    } else if(curNode.left==null && curNode.right!=null) {
        rootLastTour(curNode.right);
        //자식이 없으면
    } else {
        System.out.print(curNode.getValue()+ " ");
        return;
    }
    return;
}
```
