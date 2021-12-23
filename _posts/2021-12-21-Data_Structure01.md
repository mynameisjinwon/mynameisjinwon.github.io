---
title:  "자료구조를 공부해보자"
excerpt: "자료구조 공부 with Java"

categories:
  - Java
tags:
  - Java
  - DataStructure
  
date: 2021-12-21
last_modified_at: 2021-12-21

---
# 배열 Array

같은 자료형의 데이터를 효율적으로 관리하기 위해 사용하는 자료구조

인덱스를 통해 배열의 요소에 대한 빠른 접근이 가능하다.

길이가 고정되어있다. 

```java
int[] arr1 = new int[10];
int arr2[] = {5, 4, 3 ,2, 1};
int[] arr3 = {1, 2, 3, 4, 5};
```



### ArrayList 클래스 

가변길이의 배열을 다룰 수 있는 기능을 제공하는 클래스 

```java
import java.util.ArrayList;

List<Integer> list1 = new ArrayList<Integer>();
ArrayList<Integer> list2 = new ArrayList<Integer>();	
//List클래스 타입의 참조변수 list1은 List클래스를 상속하는 다른 클래스타입 객체를 담을 수 있지만 ArrayList클래스 타입인 list2는 ArrayList클래스 타입 객체만 담을 수 있다.
//데이터 삽입
list1.add(1);
list1.add(2);
//데이터 삭제
list1.remove(0); // 0은 인덱스 번호
//데이터 갱신
list1.set(0, 5); // 0번 인덱스의 데이터를 5로 바꿈, list에 데이터가 없으면 오류
//size() 메소드
System.out.println(list1.size()); // 배열에 있는 데이터의 개수를 리턴 
```

ArryaList는 객체만을 다루기 때문에 premitive 자료형이 아닌 wrapper 클래스를 사용해야한다.

*Array는 premitive 타입 뿐만 아니라 객체도 담을 수 있다.* 



# 큐 Queue

가장 많이 쓰이는 자료구조 중 하나 (멀티태스킹을 위한 프로세스 스케쥴링을 구현하는데 사용한다.)

FIFO(First In First Out)  선입선출, 가장 먼저 들어온 데이터가 가장 먼저 나간다.



**enqueue : 큐에 데이터를 삽입하는 기능**

**dequeue : 큐에 데이터를 삭제하는 기능**

Java는 Queue 클래스를 제공한다. Queue 클래스 사용을 위해서 LinkedList클래스가 필요하다.

``` java
import java.util.LinkedList;
import java.util.Queue;

Queue<Integer> que1 =new LinkedList<Integer>();
Queue<String> que2 = new LinkedList<String>();	

//enqueue 
que1.add(1);
que1.offer(2);
//dequeue
que1.poll();
que1.remove();
```



**ArrayList클래스를 활용해 queue 를 구현해보자**

```java
import java.util.ArrayList;

public class MyQueue<T> {
    private ArrayList<T> que1 = new ArrayList<T>();
    
    public void enqueue(T data) {
        this.que1.add(data);
    }
    public T dequeue() {
        if(que1.isEmpty()) {// queue가 비어있으면 // == que1.isEmpty();
            return null;
        } else {
            return que1.remove(0);
        }
    }
    public void printAll() {
        int idx = que1.size();
        System.out.print('[');
        for(int i=0;i<idx;i++) {
           System.out.print(que1.get(i) + " ");
        }
        System.out.print(']');
    }
}
```



# 스택 Stack

큐와 함께 가장 많이 사용되는 자료구조 

LIFO(Last In First Out) 후입선출, 가장 먼저 들어온 데이터가 가장 마지막에 나간다.

함수의 동작에 스택이 사용된다. 

**push()** : 데이터를 삽입하는 기능

**pop()** : 데이터를 꺼내는 기능

Java는 Stack 클래스를 제공한다.

```java
import java.util.Stack();

Stack<Integer> stck = new Stack<Integer>();
//데이터 삽입
stck.push(1);
//데이터 삭제
stck.pop();
```

스택의 장점 : 구조가 단순해서 구현이 쉽다. ( 배열을 사용해 구현한다.)

단점 : 배열을 사용하기 때문에 스택의 크기를 미리 정해야한다. -> 저장공간의 낭비가 생길 수 있다.



**ArrayList클래스를 활용해 stack 을 구현해보자**

```java
class MyStack<T> {
    private ArrayList<T> stck = new ArrayList<T>();
    
    public void push(T data) {
        stck.add(data);
    }
    public T pop() {
        if(stck.isEmpty()) { //스택이 비어있으면
            return null;
        } else {
            int idx = stck.size()-1;
            return stck.remove(idx);
        }
    }
    public void printAll() {
        int idx = stck.size();
        System.out.print('[');
        for(int i=0;i<idx;i++) {
            System.out.print(stck.get(i) + " ");
        }
        System.out.println(']');
    }
}
```



# 연결리스트 LinkedList

데이터가 추가될 때 자동으로 배열의 크기가 증가할 수 있게 하기위해 만들어진 자료구조

노드 : 하나의 데이터 (데이터 + 주소) 

연결리스트 노드의 주소필드는 포인터와 유사하다.

다음 노드의 주소가 저장된다. 

**장점** : 데이터 공간을 미리 할당하지 않아도 된다. (미리 할당받아 생기는 낭비되는 공간이 없다.)

**단점** : 연결을 위한 별도 공간이 필요해 저장공간의 효율이 떨어진다.

​		   중간 데이터 삭제시 연결을 재구성하는 별도 과정이 필요하다.



**링크드 리스트를 구현해 보자**

``` java
public class SingleLinkedList<T> {
    private Node<T> head = null;
    
    public class Node<T> {
        private T data;
        public Node<T> next = null;
        
        public Node(T data) {
            this.data = data;
            this.next = null;
        }
        public void setData(T data) {
            this.data =data;
        }
        public T getData() {
            return this.data;
        }
    }
    //데이터 추가
    public void saveData(T data) {
        if(this.head==null) { //리스트가 비어있으면
            this.head = new Node<T>(data);
        } else {
            Node<T> temp = this.head;
            while(temp.next!=null) { // 마지막 노드로 이동
                temp = temp.next;
            }
            temp.next = new Node<T>(data);
        }
    }
    //데이터 삭제 
    public void removeData(T data) {
        if(this.head==null) { // 리스트가 비어있으면
            return ;
        } else {
            Node<T> temp = this.head;
            if(temp.getData()==data) { //헤드를 삭제하는 경우
                head = temp.next; //다음 노드를 헤드로 바꾼다.
                temp = null;                
            } else { //중간 노드를 삭제하는 경우
                while(temp.next!=null) {
                    if(temp.next.getData()==data) {  
                        temp.next = temp.next.next; //삭제하는 노드의 직전노드의 next가 그 다음다음노드를 가리키게 한다.
                        temp.next = null;
                        return;
                    } else {
                        temp = temp.next;                        
                    }
                }
                System.out.println("삭제할 값이 없습니다.");
                return;
            }
        } 
    }
    public void printAll() {
        if(this.head==null) {
            System.out.println("리스트가 비었습니다.");
        } else {
            int cnt = 1;
            Node<T> temp = this.head;
            while(temp!=null) {
            System.out.print("["+(cnt++)+" : "+temp.getData()+"]");
            temp = temp.next;            
            }
            System.out.println();
        }
    }
}
```



# 해쉬 테이블 HashTable

키에 데이터를 맵핑할 수 있는 자료구조

해쉬 함수를 통해 키에 해당하는 주소값을 계산 후 해당 주소에 접근한다.



**해쉬함수** : 임의의 데이터를 고정된 길이의 값으로 리턴 해주는 함수

**해쉬테이블** : 미리 확보된 메모리공간

**슬롯** : 데이터가 저장되는 메모리공간

**해쉬** : 해쉬함수를 통해 리턴된 주소값 해쉬값, 해쉬주소라고도 한다.



배열의 경우 값의 존재 여부를 확인하기 위해서는 배열의 모든 인덱스에 접근해야한다.

해쉬테이블의 경우 키를 통해 데이터에 빠르게 접근이 가능하다.

**장점 :** 데이터 저장 / 읽기 속도가 빠르다.

**단점 :** 해쉬테이블 저장을 위한 별도 공간이 필요하다. 

 	 	 여러 키에 해당하는 주소가 동일할 경우 충돌을 해결하기 위한 별도 자료구조가 필요하다.

검색이 많이 필요한 경우, 저장, 삭제, 읽기가 빈번한경우에 사용한다.



**충돌(Collision) :** 하나의 키에 둘 이상의 데이터가 대응하는 경우 

충돌해결 알고리즘에는 개방해싱기법과 폐쇠해싱기법이있다.

**Chaining** : 

충돌이 일어나면 연결리스트 자료구조를 사용해 데이터를 연결시킨다.

해쉬테이블 저장공간 이외의 공간을 사용하는 개방해싱 기법에 속한다.

**LinearProbing** : 

충돌이 일어나면 해쉬테이블의 가까운 빈공간에 데이터를 저장한다.

해쉬테이블 저장공간 안에서 문제를 해결하는 폐쇠해싱 기법에 속한다.



**chaining 기법을 구현해보자**

``` java
public class MyHash {
    private Slot[] hashTable;
    
    public MyHash(Integer size) {
        this.hashTable = new Slot[size];
    }
    
    public class Slot {
        private String key;
        private String value;
        private Slot next = null;
        
        public Slot(String key, String value) {
            this.key = key;
            this.value = value;
        }
        public void setValue(String value) {
            this.value = value;
        }
        public String getValue() {
            return this.value;
        }
        public String getKey() {
            return this.key;
        }
    }
    
    public int hashFunc(String key) { // 해쉬를 만드는 해쉬함수
        return (int)(key.charAt(0)) % hashTable.length;
    }
    public void saveData(String key, String value) {
        int k = hashFunc(key);
        if(this.hashTable[k]==null) { // 해당 키의 슬롯이 비어있으면
            this.hashTable[k] = new Slot(key, value);
        } else {
            if(this.hashTable[k].getKey()==key) { //현재 키값과 동일하면 업데이트
                this.hashTable[k].setValue(value);
            } else {
                Slot curSlot = this.hashTable[k];
                Slot preSlot = curSlot;
                while(curSlot!=null) {
                    if(curSlot.getKey()==key) { //키값이 동일한 슬롯을 찾으면
                        curSlot.setValue(value);
                    } else {
                        preSlot = curSlot;
                        curSlot = curSlot.next;
                    }
                } // 연결리스트에 해당 키값이 존재하지 않으면 새로운 슬롯 생성후 연결
                preSlot.next = new Slot(key, value);
            }
        }
    }
    public String getData(String key) {
        int k = hashFunc(key);
        if(this.hashTable[k]==null) {
            System.out.println("빈 슬롯입니다.");
            return null;
        } else { 
            Slot temp = this.hashTable[k];
            while(temp!=null) {
                if(temp.getKey()==key) { //입력된 키값과 동일한 키를 갖는 슬롯을 찾으면
                    return temp.getValue();
                } else {
                    temp = temp.next;
                }
            }
            return null;
        }
    }
}
```

해쉬 테이블은 충돌이 발생하면 성능이 떨어진다.

충돌을 개선하기위해선 해쉬테이블 저장공간을 확대하거나 해쉬함수를 재정의해서 중복을 최소화해야한다.

**해쉬 테이블 시간복잡도 ** 

충돌이 없는 경우 O(1), 충돌이 발생한 경우 O(n)의 시간복잡도를 갖는다.

해쉬테이블의 경우는 일반적인경우(O(1))를 기대하고 사용한다.
