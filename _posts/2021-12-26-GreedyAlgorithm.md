---
title:  "그리디 알고리즘을 알아보자"
excerpt: "그리디 알고리즘을 공부해보자"

categories:
  - Java
tags:
  - Java
  - Algorithm
 
date: 2021-12-26
last_modified_at: 2021-12-26
---

# 탐욕 알고리즘 Greedy Algorithm

여러개 중 하나를 선택할 때 매 순간 최적이라고 생각되는 것을 선택하는 알고리즘

최적의 해에 가까운 값을 구하기 위해 사용된다.

ex) 동전 거슬러주기, 부분 배낭 문제



탐욕 알고리즘이 반드시 최적의 해를 구할 수 있는 것은 아니다.

최적의 해에 가까운 값을 구하는 방법의 하나이다.



#### 부분 배낭 문제 :

무게 제한이 k일 때 배낭에 최대 가치를 가지도록 물건을 넣는 문제

Fractional Knapsack Problem : 물건을 쪼개서 배낭에 넣을 수 있다.

0/1 Knapsack Problem : 물건을 쪼갤 수 없다.

##### 해결 전략 : 

1. 무게당 가치를 구하고 내림차순으로 정렬한다.
2. 무게당 가치가 높은 물건을 배낭에 먼저 넣으면서 가치가 최대가 되도록 한다. 



```java
class Stuff {
    public Integer weight;
    public Integer value;
    public Stuff(Integer weight, Integer value) {
        this.weight = weight;
        this.value = value;
    }
}
public class GreedyAlgorithm {
    public void knapsackFunc(Stuff[] stuffs, double capacity) {
        //물건의 무게가 capacity를 초과할 때 물건을 얼만큼 쪼갤지 비율을 저장하는 변수
        double fraction = 0.0;
        double totalValue = 0;
        
        //Compartor 인터페이스의 compare메소드를 재정의하면서 
        //Stuff 객체 배열의 데이터를 무게당 가치의 오름차순으로 정렬하도록한다.
        Arrays.sort(stuffs, new Comparator<Stuff>() {
            @Override 
            public int compare(Stuff s1, Stuff s2) {
                return (s2.value / s2.weight) - (s1.value / s1.weight);
            }
        });
        
        //객체 배열을 탐색
        for(Stuff s : stuffs) {
            //capcity가 물건의 무게보다 크면 
            if(capacity > s.weight) {
                //해당 물건의 무게만큼 capcity를 감소시키고 
                //해당 물건의 가치를 총 가치에 추가한다.
                capacity -= (double)s.weight;
                totalValue += (double)s.value;
                System.out.println("무게 : " + s.weight + " 가치 : " + s.value);
            //물건의 무게가 capacity를 초과하면
            } else {
                //물건의 무게로 capacity를 나눠 물건을 얼만큼 쪼갤지 비율을 구한다.
                fraction = capacity / (double)s.weight;
                totalValue += fraction * (double)s.value;
                System.out.println("무게 : " + s.weight + " 가치 : " + s.value + "비율 : "+ fraction);
                //물건을 쪼개어 배낭에 담으면 더 이상 물건을 담을 수 없으므로 반복문을 탈출한다.
                break;
            }
        }
        System.out.println("담을 수 있는 총 가치 : "+totalValue);
    }
}
```


___

#### 객체의 크기를 비교하는 방법

Comparable, Comparator 인터페이스의 메소드를 Override해 비교 기준을 정한다.

#### Comparable 인터페이스의 compareTo() 메소드

```java
//간선의 가중치를 비교하기위한 재정의
//Comparable 인터페이스를 구현한다.
public class Edge implements Comparable<Edge> {
    public Integer distance;
    public String vertex;
    
    public Edge(Integer distance, String vertex) {
        this.distance = distance;
        this.vertex = vertex;
    }
    //Comparable 인터페이스의 compareTo()메소드를 재정의
    //Edge 객체의 distance를 기준으로 비교한다.
    @Override 
    public int compareTo(Edge e) {
        return  this.distance - e.distance ; //현재객체가 앞에있으면 오름차순 정렬
        //return e.distance - this.distance; //현재객체가 뒤에있으면 내림차순 정렬
    }
}
```

compareTo()메소드는 현재 객체와 매개변수로 전달받은 객체를 비교한다.

```java
    @Override 
    public int compareTo(Edge e) {
        //현재 객체값이 더 크면 양수, 같으면 0, 작으면 음수를 리턴한다.
        if(this.distance > e.distance) {
            return 1;
        } else if(this.distance == e.distance) {
            return 0;
        } else {
            return -1;
        }
    }
```

위의 코드를 return this.distance - e.distance; 이 한줄로 간단하게 할 수 있지만, 이렇게 코드를 구현할 경우 

자료형의 표현범위를 벗어난 underflow, overflow가 발생하지 않도록 조심할 필요가 있다.



#### Comparator 인터페이스의 compare()메소드

```java
Edge edge1 = new Edge(15, "A");
Edge edge2 = new Edge(12, "B");
Edge edge3 = new Edge(13, "C");
Edge[] edges = new Edge[]{edge1, edge2, edge3};
Arrays.sort(edges, new Comparator<Edge>() {
    @Override
    public int compare(Edge e1, Edge e2) {
        return e2.distance - e1.distance; // 내림 차순 정렬
        //return e1.ditance - e2.distance; //오름차순 정렬
    }
});
```

compareTo() 메소드는 현재 객체와 매개변수로 받은 객체를 비교하지만 compare() 메소드는 매개변수로 두 객체를 받아 서로를 비교한다.
