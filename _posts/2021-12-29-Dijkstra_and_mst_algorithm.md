---
title:  "그래프 탐색 알고리즘을 알아보자"
excerpt: "그래프 탐색 알고리즘을 공부해보자" 

categories:
  - Algorithm
tags:
  - Java
  - Algorithm
 
date: 2021-12-29
last_modified_at: 2021-12-29


---

# 그래프 탐색 알고리즘

최단 경로 알고리즘 : 그래프의 간선의 가중치가 가장 낮은 경로를 찾는 알고리즘

알고리즘의 종류

1. 다익스트라 알고리즘 : 한 정점에서 모든 정점까지의 최단경로를 찾는 알고리즘
2. 크루스칼 알고리즘 : 가중치가 낮은 간선들을 선택하며 최소 신장 트리를 찾는 알고리즘
3. 프림 알고리즘 : 한정점에서 부터 시작해 연결된 간선들 중 가중치가 낮은 간선들을 선택하며 신장트리를 찾는 알고리즘 



### 다익스트라 알고리즘 

1. 첫 정점에 연결된 정점과 정점까지의 거리를 배열에 저장하고 시작 정점과 인접한 정점과 거리를 우선순위 큐에 삽입한다.
2.  우선순위 큐에서 정점들을 가져와 시작 정점에서 각 정점으로 가는 거리와 현재 정점에서 각 정점으로 가는 거리를 비교한다.
3. 거리가 더 짧을 경우 배열의 데이터를 갱신한다. 

#### 다익스트라 알고리즘을 구현해보자

```java
import java.util.PriorityQueue;
import java.util.ArrayList;

public class Dijkstra {
    public HashMap<String, Integer> dijkstraFunc(HashMap<String, ArrayList<Edge>> graph,
                                                String start) {
		//우선순위큐에서 꺼낸 정점을 저장할 변수
        Edge curNode = null;
        //시작 정점으로 부터 각 정점들과의 거리를 저장할 변수
        HashMap<String, Integer> distances = new HasMap<String, Integer>();
		//graph의 키값은 그래프의 모든 정점
        //각 정점들에 해당하는 값을 최대값으로 초기화한다.
        for(String key : graph.keySet()) {
            distances.put(key, Integer.MAX_VALUE);
        }
		//시작 정점의 값을 0으로 초기화
        ditances.put(start, 0);
        
		//다른 정점을 경유해서 가는 경우의 거리를 구하기위해 우선순위큐를 사용한다.
        PriorityQueue<Edge> pQueue = new PriorityQueue<Edge>();
        //처음에는 시작 정점과 0을 삽입한다.
        pQueue.add(new Edge(distances.get(start)));
        
        //우선순위큐에 정점이 있으면 반복한다.
        while(pQueue.size()>0) {
            curNode = pQueue.poll();
			//현재 정점에 연결된 모든 정점들을 탐색
            for(Edge edge : graph.get(curNode.vertext)) {
				//더 짧은 경로가 있으면 갱신한다.
                if(distances.get(edge.vertex)>curNode.weigth+edge.weight) {
                	distances.put(edge.vertex, curNode.weight+edge.weight);
                	pQueue(new Edge(distances.get(edge.vertex), edge.vertex));
                }
            }
        }
        return distances;
    }
}
```



#### 다익스트라 알고리즘의 시간 복잡도

1. 모든 간선들을 검사하는 과정 O(E) (E: 간선의 수) 

2. 우선순위큐에 노드정보를 삽입/삭제하는 과정 O(ElogE) 

   O(E) + O(ElogE) = O(E + ElogE) = O(ElogE)



### 크루스칼 알고리즘

최소신장트리를 찾는 알고리즘 

신장트리 : 모든 노드들이 연결된 싸이클이 없는 트리

최소신장트리 : 가중치의 합이 최소인 신장트리



크루스칼 알고리즘은 탐욕 알고리즘을 기초로한다.

#### 최소신장트리를 구하는 과정

1. 모든 간선들을 가중치의 오름차순으로 정렬한다.
2. 가중치가 작은 간선들을 차례로 선택한다.
3. 싸이클이 생기는지 확인한다.
4. 싸이클이 생기면 다음으로 작은 간선을 선택한다.



#### Union-Find 알고리즘

중복되는 요소가 없는 부분집합을 만드는 알고리즘

1.  초기화 : n개의 원소가 개별집합으로 이루어지도록 한다.
2. Union : 두 개별집합을 하나로 합친다.
3. Find : 싸이클이 생기는 지 확인하기위해 각 집합의 루트노드를 확인한다. (부모가 같으면 같은 집합)



Union-Find의 성능 향상을 위한 기법

- union by rank : 각 트리의 level을 rank로 하고 두 부분집합을 합칠 때 rank가 높은 부분집합의 root가 새로운 집합의 root가 된다.

- path compression : Find 실행시 거쳐간 노드들을 모두 부모노드와 직접연결한다. 이 과정을 거치면 다음에 Find를 실행했을 때 루트노드를 한번에 알 수 있다.



#### 크루스칼 알고리즘을 구현해보자

```java
import java.util.Collections;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Arrays;

public class KruskalPath {
    //각 정점들의 부모노드와 랭크값을 저장하는 변수
    HashMap<String, String> parent = new HashMap<String, String>();
    HashMap<String, Integer> rank = new HashMap<String, Integer>();
    
    //Find()
    public String find(String node) {
		//부모노드로 자신을 가리키지 않으면 (부모가 있으면)
        if(parent.get(node)!=node) {
            //자신의 부모의 부모를 부모로 가리키게한다.
            //재귀용법으로 구현해 루트노드에서 자기 자신을 리턴한다.
            //결국 find를 실행하고 나면 모든 노드가 루트노드를 가리키게된다.
            //path compression을 구현
            parent.put(node, find(parent.get(node)));
        }
        //루트 노드는 자기 자신을 리턴한다.
        return parent.get(node);
    }
    //Union
    public void union(String nodeV, String nodeU) {
        //현재 노드의 부모를 리턴받아 저장한다.
        String root1 = find(nodeV);
        String root2 = find(nodeU);
        
        //부모노드의 rank값이 더 높은 쪽에 다른 하나를 연결한다.
        if(rank.get(root1)>rank.get(root2)) {
            parent.put(root2, root1);
        } else {
            parent.put(root1, root2);
            //rank 값이 서로 같으면 새로 루트노드가 된 정점의 rank값을 1 증가시킨다.
            if(rank.get(root1)==rank.get(root2)) {
                rank.put(root2, rank.get(root2)+1);
            }
        }
    }
    //초기화 
    //모든 노드는 부모로 자기자신을 가리키고 rank값은 0으로 초기화한다.
    public void makeSet(String node) {
        this.parent.put(node, node);
        this.rank.put(node, 0);
    }
    public ArrayList<Edge> kruskalFunc(ArrayList<String> vertices, ArrayList<Edge> edges) {
        //최소 신장트리를 저장할 변수
        ArrayList<Edge> mst = new ArrayList<Edge>();
        Edge curEdge;
        
        //초기화
        //모든 노드는 부모로 자기자신을 가리키고 rank값은 0으로 초기화한다.
        for(int i=0;i<vertices.size();i++) {
            makeSet(vertices.get(i));
        }
        //Edge의 weight 기준으로 오름차순 정렬
        Collections.sort(edges);
        System.out.println(edges);
        
        //가중치가 낮은 간선부터 진행한다.
        for(int i=0;i<edges.size();i++){ 
            curEdge = edges.get(i);
            //루트노드가 서로 다르면 union을 진행하고 mst에 저장한다.
            if(find(curEdge.nodeV)!=find(curEdge.nodeU)){
                union(curEdge.nodeV, curEdge.nodeU);
                mst.add(curEdge);
            }
        }
        return mst;
    }
}
```



#### 크루스칼 알고리즘의 시간복잡도

1. 초기화 : 노드의 총 개수만큼 반복 O(n)
2. 정렬 : 정렬알고리즘의 시간복잡도와 같다. O(ElogE) 간선의 가중치를 기준으로 정렬하므로 간선의 수와 비례
3. union : O(E)

크루스칼 알고리즘의 시간복잡도는 각 과정 중 최대값인 O(ElogE) 이다.



#### Edge 클래스

```java
public class Edge implements Comparable<Edge> {
    public int weight;
    public String node1;
    public String node2;
    
    public Edge(int weight, String nodeV, String nodeU) {
        this.weight = weight;
        this.nodeV = nodeV;
        this.nodeU = nodeU;
    }
    public String toString() {
        return "(" + weight +", "+ this.nodeV + ", " + nodeU + ")";
    }
    //가중치값을 기준으로 정렬하기위해 compareTo()메소드를 재정의한다.
    @Override
    public int compareTo(Edge edge) {
        return this.weight - edge.weight;
    }
}
```

