---
layout: post
title: 최단 경로(Shortest Path)
comments: true
category: Computer Algorithm
tag: [graph, shortest path]
---
가중치가 없는 그래프에서 두 정점 사이의 최단 거리는 BFS를 이용하면 쉽게 구할 수 있다. 만약 가중치가 있는 그래프에서 임의의 두 정점 사이의 최단 경로를 구해야 한다면 다른 방법을 찾아야 한다.  

## 단일 시작점 최단 경로

그래프에서 시작점 ***S***가 존재하고, 이 정점으로부터 모든 정점 사이의 최단 경로를 구하는 유명한 최단 경로 알고리즘으로는 다익스트라 알고리즘과 벨만-포드 알고리즘이 있다.

### 다익스트라(Dijkstra) 알고리즘

다익스트라 알고리즘은 시작 정점 ***S*** 로부터 가장 가까운 정점부터 방문해나가는 탐욕적 알고리즘이다. 가중치가 없는 그래프에서는 큐를 이용한 BFS로 이 작업을 수행하지만 가중치가 있는 그래프에서는 그렇지 못하다. 대신에 우선 순위 큐를 사용함으로써 이 작업을 수행한다. 시간복잡도는 $$O(E \space log_2{  V }  )$$이다.

``` c++
int V;
vector<pair<int, int>> adj[MAX_V];

vector<int> dijkstra(int src) {
  vector<int> dist(V, INF);
  dist[src] = 0;
  priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
  pq.push({ 0, src });
  while (!pq.empty()) {
    int here = pq.top().second;
    int cost = pq.top().first;
    pq.pop();
    if (dist[here] < cost) continue;
    for (auto& edge : adj[here]) {
      int there = edge.first;
      int ncost = edge.second + cost;
      if (dist[there] > ncost) {
        dist[there] = ncost;
        pq.push({ ncost, there });
      }
    }
  }
  return dist;
}
```

### 벨만-포드(Bellman-Ford) 알고리즘

만일 주어진 그래프에 가중치가 음수인 간선이 있는 경우 다익스트라 알고리즘은 정상적으로 동작하지 않는다. 위에서 구현한 다익스트라 알고리즘은 우선 순위 큐를 사용하기 때문에 느리긴 하지만 음수 간선이 있어도 동작은 한다. 음수 간선이 존재하는 그래프의 단일 시작점 최단 경로를 구하려면 다른 알고리즘을 사용하는 것이 좋다. 음수 간선에서도 잘 동작하는 알고리즘으로 벨만-포드 알고리즘이 유명하다. 벨만-포드 알고리즘은 다음과 같은 간선의 특성을 기반으로 시작점으로부터 다른 모든 정점으로 가는 최단 거리를 완화(relax)해나간다.

<br>

***dist[v] $$\le$$ dist[u] + w(u, v)***  (***dist[u]***: 시작점 ***S***로부터 ***u***로 가는 최단 거리)

<br>

***S***와 ***u*** 사이의 경로가 있다고 가정하면 ***dist[u]***는 최대 $$V-1$$개의 간선을 거친 경로의 거리이다. 따라서 위의 특성을 이용하여 경로상의 간선의 개수가 1개, 2개, ..., (최대) $$V-1$$개인 최단 경로를 확정해 나가면 각 정점까지의 최단 경로를 모두 찾을 수 있게 된다. 시간복잡도는 $$O(VE)$$가 된다. 

음수 간선에서도 동작한다는 특징 외에 벨만-포드 알고리즘에 좋은점이 하나 더 있다. 그래프에 음수 사이클이 존재한다면 최단 경로 문제는 의미가 없어진다. 위에서 구현한 다익스트라 알고리즘도 음수 사이클이 존재하는 그래프에서 실행시킨다면 무한 루프에 빠지게 된다. 벨만-포드 알고리즘은 최대 $$V-1$$의 완화 작업을 수행하기 때문에 무한 루프에 빠질 우려는 없다. 더욱이  $$V-1$$번의 완화 이후 더 완화될 수 있는 정점이 존재한다면 주어진 그래프는 음수 사이클을 가졌다고 판정할 수 있다.

``` c++
int V;
vector<pair<int, int>> adj[MAX_V];

vector<int> bellmenford(int src) {
  vector<int> dist(V, INF);
  dist[src] = 0;
  bool updated;
  for (int i = 0; i < V; i++) {
    updated = false;
    for (int here = 0; here < V; here++) {
      for (auto& edge : adj[here]) {
        int there = edge.first;
        int cost = edge.second;
        if (dist[here] != INF && dist[there] > dist[here] + cost) {
          dist[there] = dist[here] + cost;
          updated = true;
        }
      }
    }
    if (!updated) break;
  }
  assert(!updated);  // if updated: true, then exists negative cycle.
  return dist;
}
```

### Shortest-Path-Fast-Algorithm(SPFA)

두안 판딩(Dual Fanding)이라는 중국인에 의해 발표된 알고리즘으로 큐를 활용하여 벨만-포드 알고리즘에서 불필요한 연산들을 제거한 알고리즘이다. 시간복잡도는 $$O(VE)$$로 벨만-포드와 같지만 보통 더 빠르게 동작한다고 한다. 음수 사이클의 유무는 큐에 $$V$$번 이상 삽입되는 정점의 유무로 확인할 수 있다.

``` c++
int V, E;
vector<pair<int, int>> adj[MAX_V];

vector<int> spfa(int src) {
  queue<int> q;
  q.push(src);
  vector<bool> in_queue(V, false);
  in_queue[src] = true;
  vector<int> dist(V, INF), inserted(V, 0);
  dist[src] = 0;
  inserted[src] = 1;
  while (!q.empty()) {
    int here = q.front(); q.pop();
    in_queue[here] = false;
    for (auto& edge : adj[here]) {
      int there = edge.first;
      int cost = edge.second;
      if (dist[here] != INF && dist[there] > dist[here] + cost) {
        dist[there] = dist[here] + cost;
        if (!in_queue[there]) {
          q.push(there);
          in_queue[there] = true;
          inserted[there]++;
          // if some vertex that enters the queue for over V - 1 times,
          // graph has negative cycle.
          assert(inserted[there] < V);
        }
      }
    }
  }
  return dist;
}
```

### 0-1 BFS

그래프의 가중치가 0 또는 1인 경우에 대해서 생각해보자. 이 경우 다익스트라 알고리즘을 사용하면 $$O(E\space log_2V)$$에 해결할 수 있다. 하지만 자료 구조 덱(deque)를 이용하면 $$O(V+E)$$만에 문제를 해결할 수 있다. 현재 정점 $$u$$를 방문중일 때, 인접한 정점 $$v$$로 가는 간선의 가중치가 0이라면  $$v$$를 덱에 앞에다 넣고, 1이라면 뒤에다 넣으면 된다. 조금 정리를 해보면 단일 시작점 최단 경로를 구할 때, 간선의 가중치에 따라 사용하는 자료 구조는 다음과 같다.

- 모든 간선의 가중치가 0: 큐(queue)
- 모든 간선의 가중치가 0 또는 1: 덱(deque)
- 간선의 가중치가 제각각: 우선순위 큐(priority queue)



## 모든 쌍 최단 경로

그래프의 임의의 두 정점쌍에 대한 최단 경로를 모두 구하고자 할 때는 다익스트라나 벨만 포드 알고리즘을 $$V$$번 수행하여 구할 수도 있지만, 플로이드-워셜이라는 보다 효율적인 알고리즘을 사용할 수 있다.

### 플로이드-워셜(Floyd-Warshell) 알고리즘

플로이드-워셜 알고리즘은 동적 계획법(dynamic programming)을 이용하여 두 정점 사이의 최단 경로를 계산한다.  $$dist[k][u][v]$$를 ***u***에서 **{ 1, 2, ..., k }** 내의 정점들을 경유점을 하여(경유해도 되고, 안해도 됨) ***v***에 도달할 수 있는 최단 거리라고 하면 다음과 같은 관계식을 얻을 수 있다.

<br>

$$ dist[k][u][v] =min\begin{cases}dist[k-1][u][k] + dist[k-1][k][v]  \\ dist[k-1[u][v] \end{cases} $$ 

<br>

이와 같은 관계식을 토대로 아래와 같이 코드로 구현할 수 있다. 눈여겨볼만한 부분은 $$dist[k][?][?]$$를 구할 때, $$dist[k-1][?][?]$$만 필요하므로 슬라이딩 윈도우 기법으로 메모리 사용량을 줄인 부분이다. 초기 $$dist[u][v]$$는 ***u***에서 ***v***로 가는 간선의 가중치를 두고, 간선이 없으면 매우 큰 값(INF)을 넣어두면 된다(다만, INF + INF시 오버플로가 발생하면 안된다). 시간복잡도는 $$O(V^3)$$이 된다.

``` c++
int V;
/* initially set dist[u][v] = w (if there is edge of weight w from u to v)
             set dist[u][v] = INF (if there is no edge from u to v) */
int dist[MAX_V][MAX_V];

void floyd() {
  for (int u = 0; u < V; u++)
    dist[u][u] = 0;
  for (int k = 0; k < V; k++) {
    for (int u = 0; u < V; u++) {
      for (int v = 0; v < V; v++) {
        dist[u][v] = min(dist[u][v], dist[u][k] + dist[k][v]);
      }
    }
  }
}
```

## 연습 문제

**BOJ 1916 최소비용 구하기 ([문제 보기](https://www.acmicpc.net/problem/1916))**

**BOJ 1238 파티 ([문제 보기](https://www.acmicpc.net/problem/1238))**

**BOJ 1261 알고스팟 ([문제 보기](https://www.acmicpc.net/problem/1261))**

**BOJ 6593 상범 빌딩 ([문제 보기](https://www.acmicpc.net/problem/6593))**

**BOJ 1486 등산 ([문제 보기](https://www.acmicpc.net/problem/1486))**

**BOJ 11657 타임머신 ([문제 보기](https://www.acmicpc.net/problem/11657))**

**BOJ 1865 웜홀 ([문제 보기](https://www.acmicpc.net/problem/1865))**

**BOJ 2606 바이러스 ([문제 보기](https://www.acmicpc.net/problem/1865))**

**BOJ 1613 역사 ([문제 보기](https://www.acmicpc.net/problem/1613))**

**BOJ 1219 오민식의 고민 ([문제 보기](https://www.acmicpc.net/problem/1219))**

## 참고 자료

- 프로그래밍 대회에서 배우는 알고리즘 문제해결전략, 구종만
- Competive Programming 3, Steven Halim
- http://codeforces.com/blog/entry/22276