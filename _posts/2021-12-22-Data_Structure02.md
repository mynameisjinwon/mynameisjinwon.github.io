---
title:  "트리를 배워보자"
excerpt: "Java로 이진탐색트리를 구현해보자"

categories:
  - Java
tags:
  - Java
  - DataStructure
  
date: 2021-12-22
last_modified_at: 2021-12-22


---

# **트리** Tree

Node와 Branch를 이용해 싸이클을 이루지 않도록 구성된 자료구조

트리 중 이진트리는 검색 알고리즘에 사용된다.



#### 트리 용어

- Node : 트리에서 데이터를 저장하는 공간

- Root Node : 트리의 맨 위에있는 노드

- Level : 트리의 깊이 (루트노드는 level0)

- Parent Node : 어떤 노드의 상위 노드 

- Child Node : 어떤 노드의 하위 노드

- Leaf Node : 자식이 없는 노드 

- Sibling : 동일한 부모를 가지고 있는 노드

- Depth : 트리에서 노드가 가질 수 있는 최대 level

  


이진 트리 : 노드의 최대 가지 수가 2인 트리
이진 탐색 트리 : 루트 노드를 기준으로 왼쪽은 더 작은 값 오른쪽은 더 큰 값만을 저장하는 이진 트리




복잡한 코드를 작성 할 때는 가능한 경우를 나누어야 한다.

트리를 삭제할 때 다음과 같은 경우로 나눌 수 있다.

1. 단말 노드를 삭제하는 경우
2. 자식 노드가 하나인 노드를 삭제하는 경우
3. 자식 노드가 두 개인 노드를 삭제하는 경우



#### 이진 탐색 트리를 구현해 보자

```java
public class Node {
    public Node left;
    public Node right;
    public int value;

    public Node(int value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
    public int getValue() {
        return this.value;
    }
}
public class NodeMgmt {
    Node rootNode = null;
    
    public boolean insertNode(int value) {
        if(this.rootNode==null) { //트리가 비어있을 때
            this.rootNode = new Node(value);
        } else { 
            Node curNode = this.rootNode;
            while(true) {
                if(value < curNode.getValue()) { //부모노드 보다 작은 값이 들어오면
                    if(curNode.left!=null) { // 왼쪽 자식노드가 있으면 이동
                        curNode = curNode.left;
                    } else { // 자식 노드가 없으면
                        curNode.left = new Node(value);
                        break;
                    } 
                } else {  //부모노드 보다 큰값이 들어오면
                    if(curNode.right!=null) { // 오른쪽 자식노드가 있으면 이동
                        curNode = curNode.right;
                    } else { // 자식 노드가 없으면
                        curNode.right = new Node(value);
                        break;
                    }
                }
            }
            
        }
        return true;
    }
    //입력 받은 데이터가 있는지 검색후 있으면 해당값을 리턴해주는 메소드
    public Node search(int value) {
        if(this.rootNode==null) {
            return null;
        } else {
            Node searchNode = this.rootNode;
            while(searchNode!=null) {
                if(value == searchNode.getValue()) {
                    return searchNode;
                } else if(value < searchNode.getValue()) { // 해당 노드의 데이터보다 작을 때
                    searchNode = searchNode.left;
                } else {
                    searchNode = searchNode.right; 
                }
            }
            System.out.println("존재하지 않는 값입니다.");
            return null;
        }
    }
    public boolean delNode(int value) {
        if(this.rootNode==null) {
            System.out.println("트리가 비어있습니다.");
            return false;
        } else {
            Node curNode = this.rootNode;
            Node preNode = curNode;      
            // 리프노드를 삭제하는 경우
            if(search(value).left==null&&search(value).right==null) {
                while(curNode!=null) {
                    if(value == curNode.getValue()) {
                          if(value < preNode.getValue()) {
                              preNode.left = null;
                              return true;
                          } else {
                              preNode.right = null;
                              return true;
                          }
                    } else if(value < curNode.getValue()) {
                        preNode = curNode;
                        curNode = curNode.left;
                    } else {
                        preNode = curNode;
                        curNode = curNode.right;
                    }
                }
                // 삭제하려는 값이 없으면 while문을 탈출한다.
                return false;
                
            //자식 노드가 하나인 노드를 삭제하는 경우    
            } else if((search(value).left!=null&&search(value).right==null)
                      ||(search(value).left==null&&search(value).right!=null)) {
                while(curNode!=null) {
                    if(value==curNode.getValue()) {
                        if(curNode.getValue() < preNode.getValue()) { // 부모노드보다 크기가 작은 노드를 삭제하는 경우
                            if(curNode.left!=null) { //왼쪽 자식만 있는 경우
                                curNode = curNode.left;
                                preNode.left = curNode;
                                return true;
                            } else {
                                curNode = curNode.right;
                                preNode.left = curNode;
                                return true;
                            }
                        } else { //부모노드보다 크기가 큰 노드를 삭제하는 경우
                            if(curNode.left!=null) { //왼쪽 자식만 있는 경우
                                curNode = curNode.left;
                                preNode.right = curNode;
                                return true;
                            } else {
                                curNode = curNode.right;
                                preNode.right = curNode;
                                return true;
                            }
                        }
                    } else if(value < curNode.getValue()) {
                        preNode = curNode;
                        curNode = curNode.left;
                    } else {
                        preNode = curNode;
                        curNode = curNode.right;
                    }
                }   
                return false;
            //자식 노드가 두개인 노드를 삭제하는 경우
            //삭제하려는 노드가 부모노드보다 작으면 오른쪽 자식 트리중 가장 작은값을 올린다.
            //삭제하려는 노드가 부모노드보다 크면 오른쪽 자식 트리중 가장 작은값을 올린다.
            } else {
                Node searchNode = search(value);
                Node rightChild = searchNode.right;
                Node preChild = rightChild;
                if(value < this.rootNode.getValue()) { // 삭제하려는 노드가 루트노드의 왼쪽에 있을 때
                    while(curNode!=null) { //삭제 하려는 노드의 부모노드를 찾는다.
                        if(value==curNode.getValue())
                            break;
                        else {
                            if(value < curNode.getValue()) {
                                preNode = curNode;
                                curNode = curNode.left;
                            } else {
                                preNode = curNode;
                                curNode = curNode.right;
                            }
                        }
                    }   
                    while(rightChild.left!=null) {//가장 작은 노드로 이동
                        preChild = rightChild; // 가장 작은 노드의 부모노드
                        rightChild = rightChild.left;
                    }
                    if(rightChild.right==null) { // 가장 작은 노드가 리프노드일 때
                        preNode.left = rightChild;
                        rightChild.right = curNode.right;
                        curNode = null;
                        return true;
                    } else { // 가장 작은 노드가 오른쪽 자식 노드를 가질 때
                        preChild.left = rightChild.right;
                        preNode.left = rightChild;
                        rightChild.right = curNode.right;
                        curNode = null;
                        return true;
                    }
                } else { // 삭제하려는 노드가 루트노드의 오른쪽에 있을 때
                    while(curNode!=null) { //삭제 하려는 노드의 부모노드를 찾는다.
                        if(value==curNode.getValue())
                            break;
                        else {
                            if(value < curNode.getValue()) {
                                preNode = curNode;
                                curNode = curNode.left;
                            } else {
                                preNode = curNode;
                                curNode = curNode.right;
                            }
                        }
                    }
                    while(rightChild.left!=null) {//가장 작은 노드로 이동
                        preChild = rightChild; // 가장 작은 노드의 부모노드
                        rightChild = rightChild.left;
                    }
                    if(rightChild.right==null) { // 가장 작은 노드가 리프노드일 때
                        preNode.right = rightChild;
                        rightChild.right = curNode.right;
                        curNode = null;
                        return true;
                    } else { // 가장 작은 노드가 오른쪽 자식 노드를 가질 때
                        preChild.left = rightChild.right;
                        preNode.right = rightChild;
                        rightChild.right = curNode.right;
                        curNode = null;
                        return true;
                    }
                }
            }
        }
    }
    public void printAll() {
        Node temp = this.rootNode;
        
    }
}
```

동일한 작업을 하는 코드가 여러군데 중복되어 있어 비효율적이다.



#### 트리의 시간 복잡도 

이진탐색트리의 시간 복잡도는 O(longN) 이다. 

하지만 최악의 경우 O(n)으로 배열과 동일한 시간복잡도를 갖는다.
