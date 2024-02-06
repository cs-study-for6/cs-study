# Graph
## 그래프란?
![graph](/image/graph.png)
자료구조로써의 그래프는 정점(Vertex)과 정점들을 연결하는 변(Edge)으로 구성이 된다. 일반적으로 정점은 원으로 표현하고 변은 화살표나 선분으로 표현한다.

해석학적인 그래프와는 구분이 필요하다.

## 그래프 용어

1. 정점(Vertex): 노드(node), 데이터 저장
2. 간선(Edge): 정점을 연결하는 선
    - 가중치 (Weighted value): 변이 가질 수 있는 특정한 수치
3. 분지수(차수, degree): 무방향 그래프에서 하나의 정점에 붙어있는 간선 개수
4. 내향 분지수(진출 차수, in-degree): 방향 그래프에서 들어오는 간선의 수
5. 외향 분지수(진입 차수, out-degree): 방향 그래프에서 나가는 간선의 수
6. 인접
    - 인접(adjacent): 정점 사이 간선이 있음
    - 부속(incident): 정점과 간선 사이 관계
7. 경로(path): 출발지에서 목적지로 가는 순서
8. 단순 경로(simple path): 경로 중 반복되는 정점이 없음
9. 사이클(cycle): 단순 경로의 출발지와 목적지가 같은 경우


## 그래프의 종류
### 무방향 그래프
![undirgraph](/image/undirgraph.png)
간선에 방향이 없는 그래프이다. 간선을 선분으로 표현하는 경우 양방향 모두 이동이 가능하며, 정점 v와 정점 w를 연결하는 간선을 (v, w)라고 하면, (v, w)와 (w, v)는 같은 간선이다.
정점 n개일 때 무방향 그래프가 가질 수 있는 최대 간선 수는 **n(n-1)/2** 개 이다.

### 방향 그래프
![dirgraph](/image/dirgraph.png)
간선에 방향이 있는 그래프이다. 간선을 화살표로 나타내는 경우 해당 방향으로만 이동할 수 있으며, 정점 v에서 w로 가는 간선을 (v, w)라고 하고, 이때는 간선 (w, v)와 다르다.
정점 n개일 때 방향 그래프가 가질 수 있는 최대 간선 수는 **n(n-1)** 개 이다.

### 완전 그래프
![complegraph](/image/complegraph.png)
모든 정점에 간선이 있고, 한 정점에서 다른 정점과 모두 연결되어 있는 그래프이다.

### 가중치 그래프
![wgraph](/image/wgraph.png)
간선에 가중치가 부여되어 있다.

## 그래프 구현
### 인접 행렬
![crgraph](/image/crgraph.png)
간선에 가중치가 부여되어 있다.
- 2차원 배열 matrix 사용
matrix[v][w] = 1 : 정점 v에서 정점 w로 가는 간선이 있음
- matrix[v][w] = 0 : 정점 v에서 정점 w로 가는 간선이 없음
- 장점 : 구현이 쉬우며 연결된 정점을 빠르게 찾을 수 있다.
- 단점 : **O(n^2)** 의 공간복잡도를 갖는다.
- 구현 코드
```java
public static void main(String[] args) {
		int[][] edges = new int[][] {
			{1, 2}, {1, 3}, {1, 4}, {2, 3}, {2, 5}, {4, 5}
		};
		
		int n = 5; //정점의 개수
		
		int[][] matrix = new int[n + 1][n + 1];
		
		for(int[] edge : edges) {
			matrix[edge[0]][edge[1]] = 1;
			matrix[edge[1]][edge[0]] = 1;
		}
		
        //출력
		for(int i = 1 ; i <= n ; i++) {
			for(int j = 1 ; j <= n ; j++) {
				System.out.print(matrix[i][j]+" ");
			}
			System.out.println();
		}
	}


// 출력 결과
0 1 1 1 0 0 
1 0 1 0 1 0 
1 1 0 0 0 0 
1 0 0 0 1 0 
0 1 0 1 0 0 
```

### 인접 리스트
![listgraph](/image/listgraph.png)
- 배열 또는 리스트를 사용한다.
- 정점의 개수만큼 헤드 노드가 있고, 각 정점에 인접한 정점들 리스트로 연결한다.
- 정점 v의 인접 정점이 w와 z라면 헤드노드 v에 w와 z가 연결 리스트로 연결되어있다.
- 구현 코드
```java
// 배열 + 리스트
public static void main(String[] args) {
		int[][] edges = new int[][] {
			{1, 2}, {1, 3}, {1, 4}, {2, 3}, {2, 5}, {4, 5}
		};
		
		int n = 5;
		
		ArrayList<Integer>[] list = new ArrayList[n + 1];

		for (int i = 0; i <= n; i++) list[i] = new ArrayList<>();
		
		for(int[] edge : edges) {
			list[edge[0]].add(edge[1]);
			list[edge[1]].add(edge[0]);
		}
		
        //출력
		for (int i = 1; i <= n; i++) {
			for(int j = 0 ; j < list[i].size();j++)
				System.out.print(list[i].get(j)+" ");
			System.out.println();
		}
	}
// 리스트 + 리스트
public static void main(String[] args) {
		int[][] edges = new int[][] {
			{1, 2}, {1, 3}, {1, 4}, {2, 3}, {2, 5}, {4, 5}
		};
		
		int n = 5;
		
		ArrayList<ArrayList<Integer>> list = new ArrayList<>();
		
		for(int i = 0 ; i <= n ; i++) list.add(new ArrayList<>());
		
		for(int[] edge : edges) {
			list.get(edge[0]).add(edge[1]);
			list.get(edge[1]).add(edge[0]);
		}
		
        //출력
		for (int i = 1; i < list.size(); i++) {
			for(int j = 0 ; j < list.get(i).size(); j++) 
				System.out.print(list.get(i).get(j)+" ");
			System.out.println();
		}
	}
// 출력 결과
2 3 4 
1 3 5 
1 2 
1 5 
2 4 
```

## 그래프 순회
- 그래프의 한 정점에서 출발하여 체계적으로 모든 정점을 한번씩 방문하는 것을 그래프 순회라고 한다. 대표적인 알고리즘으로 깊이 우선 탐색(DFS)와 너비 우선 탐색(BFS)이 있다.

### 깊이 우선 탐색(DFS, Depth-First-Search)
![dfs](/image/dfs.gif)
1. 정점 v 방문
2. v의 인접 정점 중 아직 방문하지 않은 정점 w를 찾아 DFS를 재귀적으로 수행
- 시간 복잡도
    - 인접 리스트 : O(V + E)
    - 인접 행렬 : O(V^2)
- 구현 코드
```java
/* 인접 리스트 이용 */
class Graph {
  private int V;
  private LinkedList<Integer> adj[];
 
  Graph(int v) {
      V = v;
      adj = new LinkedList[v];
      // 인접 리스트 초기화
      for (int i=0; i<v; ++i)
          adj[i] = new LinkedList();
  }
  void addEdge(int v, int w) { adj[v].add(w); }
 
  /* DFS에 의해 사용되는 함수 */
  void DFSUtil(int v, boolean visited[]) {
      // 현재 노드를 방문한 것으로 표시하고 값을 출력
      visited[v] = true;
      System.out.print(v + " ");
 
      // 방문한 노드와 인접한 모든 노드를 가져온다.
      Iterator<Integer> it = adj[v].listIterator();
      while (it.hasNext()) {
          int n = it.next();
          // 방문하지 않은 노드면 해당 노드를 시작 노드로 다시 DFSUtil 호출
          if (!visited[n])
              DFSUtil(n, visited);
      }
  }
 
  /* DFS */
  void DFS(int v) {
      boolean visited[] = new boolean[V];
 
      // v를 시작 노드로 DFSUtil 재귀 호출
      DFSUtil(v, visited);
  }
}
```

### 너비 우선 탐색(BFS, Breadth-First-Search)
![bfs](/image/bfs.gif)
1. 정점 v 방문 - 큐에 삽입
2. v의 인접 정점 중 아직 방문하지 않은 정점을 차례대로 방문
3. 큐에 있는 다음 정점을 꺼내 2번을 실행 ~반복
- 시간 복잡도
    - 인접 리스트 : O(V + E)
    - 인접 행렬 : O(V^2)
- 구현 코드
```java
class Graph {
  private int V;
  private LinkedList<Integer> adj[];
 
  Graph(int v) {
    V = v;
    adj = new LinkedList[v];
    for (int i=0; i<v; ++i)
      adj[i] = new LinkedList();
  }
 
  void addEdge(int v, int w) { adj[v].add(w); }
 
  /* BFS */
  void BFS(int s) {
    boolean visited[] = new boolean[V];
    LinkedList<Integer> queue = new LinkedList<Integer>();
 
    visited[s] = true;
    queue.add(s);
 
    while (queue.size() != 0) {
      // 방문한 노드를 큐에서 추출(dequeue)하고 값을 출력
      s = queue.poll();
      System.out.print(s + " ");
 
      // 방문한 노드와 인접한 모든 노드를 가져온다.
      Iterator<Integer> i = adj[s].listIterator();
      while (i.hasNext()) {
        int n = i.next();
        // 방문하지 않은 노드면 방문한 것으로 표시하고 큐에 삽입(enqueue)
        if (!visited[n]) {
          visited[n] = true;
          queue.add(n);
        }
      }
    }
  }
}
```



### 참고 링크
https://velog.io/@suk13574/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0Java-%EA%B7%B8%EB%9E%98%ED%94%84Graph
https://developer-mac.tistory.com/64
