---
title:  "그래프를 알아보자"
excerpt: "그래프를 공부해보자"

categories:
  - Java
tags:
  - Java
  - Algorithm
 
date: 2021-12-25
last_modified_at: 2021-12-26

---

# 그래프

실제 세계의 사물이나 현상을 정점과 간선을 이용해 표현한 것

#### 그래프 용어

- 노드(Node, Vertex)  : 정점

- 간선(Edge) : 노드를 연결하는 선
- 인접 정점(Adjacent Node) : 간선으로 직접 연결된 노드
- 차수(Degree) : 무방향 그래프에서 하나의 정점에 연결된 간선의 수
- 진입 차수(In-Degree) : 방향 그래프에서 외부에서 정점으로 들어오는 간선의 수
- 진출 차수(Out-Degree) : 방향 그래프에서 외부로 뻗어나가는 간선의 수
- 경로 길이(Path Length) : 경로를 구성하는데 사용된 간선의 수
- 단순 경로(Simple Path) : 중복된 정점이 없는 경로 
- 싸이클(Cycle) : 단순 경로의 시작점과 종점이 같은 경로



#### 그래프의 종류 

- 무방향 그래프 : 간선에 방향성이 없는 그래프
- 방향 그래프 : 간선에 방향성이 있는 그래프
- 가중치 그래프 : 간선에 가중치가 할당된 그래프
- 연결 그래프 : 무방향 그래프에서 모든 노드가 연결되어 있는 그래프
- 비연결 그래프 : 무방향 그래프에서 특정 노드가 연결되어 있지 않은 그래프
- 비순환 그래프 : 싸이클이 없는 그래프
- 완전 그래프 : 모든 노드가 서로 연결되어 있는 그래프



## 그래프 탐색 알고리즘

그래프 탐색 알고리즘에는 너비 우선 탐색과 깊이 우선 탐색이 있다.



### 너비 우선 탐색 Breadth First Search

시작 노드에 연결된 모든 노드들을 방문하는 탐색 알고리즘

Java에서 그래프는 HashMap과 ArrayList를 사용해서 표현한다.

HashMap의 key값은 그래프의 노드이고, value값은 해당 노드에 인접 노드들이 ArrayList형태로 저장된다.



BFS를 구현하기 위해서 방문 노드를 저장하는 큐와 다음에 방문할 노드들을 저장하는 큐가 필요하다.

#### BFS 구현하기

```java
import java.util.HashMap;
import java.util.ArrayList;
import java.util.Arrays;

public class BFS {
    public ArrayList<String> search(HashMap<String, ArrayList<String>> graph, 
                                    String startNode){
        //방문한 노드를 저장하는 visited, 방문할 노드를 저장하는 needVisit 리스트를 만든다.
        ArrayList<String> visited = new ArrayList<String>();
        ArrayList<String> needVisit = new ArrayList<String>();
        
        //처음 방문할 노드를 needVisit리스트에 삽입한다.
        String node = startNode;
        needVisit.add(node);
        
        //방문할 노드가 남아있으면 반복
        while(needVisit.size() > 0) {
            //선입선출을 위해 첫 데이터를 꺼낸다.
            node = needVisit.remove(0);
			//contains 메소드를 통해 해당 노드를 방문했는지를 확인한다.
            if(visited.contains(node)) 
                continue;
            //방문하지 않았으면 방문한다.
            visited.add(node);
            //방문한 노드의 인접 노드들을 needVisit리스트에 삽입한다.
            needVisit.addAll(graph.get(node));
        }
        return visited;
    }
}
```



### 깊이 우선 탐색 DepthFirstSearch

한 노드의 자식 노드를 끝까지 탐색하고 돌아와 다음 자식 노드를 방문하는 탐색 알고리즘



BFS와 유사하지만 needVisit리스트를 스택으로 사용한다.

#### DFS 구현하기

```java
import java.util.HashMap;
import java.util.ArrayList;
import java.util.Arrays;

public class DFS {
    public ArrayList<String> search(HashMap<String, ArrayList<String>> graph, 
                                    String startNode){
        //방문한 노드를 저장하는 visited, 방문할 노드를 저장하는 needVisit 리스트를 만든다.
        ArrayList<String> visited = new ArrayList<String>();
        ArrayList<String> needVisit = new ArrayList<String>();
        
        //처음 방문할 노드를 needVisit리스트에 삽입한다.
        String node = startNode;
        needVisit.add(node);
        
        //방문할 노드가 남아있으면 반복
        while(needVisit.size() > 0) {
            //후입선출을위해 마지막 데이터를 먼저 꺼낸다.
            node = needVisit.remove(needVisit.size()-1);
			//contains 메소드를 통해 해당 노드를 방문했는지를 확인한다.
            if(visited.contains(node)) 
                continue;
            //방문하지 않았으면 방문한다.
            visited.add(node);
            //방문한 노드의 인접 노드들을 needVisit리스트에 삽입한다.
            needVisit.addAll(graph.get(node));
        }
        return visited;
    }
        public ArrayList<String> search2(HashMap<String, ArrayList<String>> graph, 
                                    String startNode){
        //방문한 노드를 저장하는 visited, 방문할 노드를 저장하는 needVisit 리스트를 만든다.
        ArrayList<String> visited = new ArrayList<String>();
        ArrayList<String> needVisit = new ArrayList<String>();
        
        //처음 방문할 노드를 needVisit리스트에 삽입한다.
        String node = startNode;
        needVisit.add(node);
        
        //방문할 노드가 남아있으면 반복
        while(needVisit.size() > 0) {
            node = needVisit.remove(0);
			//contains 메소드를 통해 해당 노드를 방문했는지를 확인한다.
            if(visited.contains(node)) 
                continue;
            //방문하지 않았으면 방문한다.
            visited.add(node);
            //방문한 노드의 인접 노드들을 needVisit리스트에 삽입한다.
            needVisit.addAll(0, graph.get(node));
        }
        return visited;
    }
}
```



#### 시간 복잡도 

BFS, DFS의 시간복잡도는 O(V+E) ( V: 노드의 수 , E:간선의 수)

