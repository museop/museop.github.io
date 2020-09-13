---
layout: post
title: 최소 스패닝 트리(Minimum Spanning Tree)
comments: true
category: Computer Algorithm
tag: [graph, minimum spanning tree, tree, kruskal algorithm, prim algorithm, disjoint set]
---
## 최소 스패닝 트리란?

최소 스패닝 트리(이하 MST)는 연결된 무방향 가중치 그래프 $$G$$에서 정의되는 개념으로 간선의 부분 집합 $$E' \in E$$을 선택하여 그래프를 재구성하여 만들 수 있는 그래프 $$G' $$(트리)로 다음과 같은 특징들을 만족해야 한다.

- 연결된 그래프여야 한다.
- 사이클을 없어야 한다. 즉, 루트 없는 트리 형태여야 한다.
- 간선들의 가중치 합이 최소가 되어야 한다.



MST를 구하는 대표적인 알고리즘으로 크루스칼 알고리즘과 프림 알고리즘이 있다. 두 알고리즘 모두 탐욕적 알고리즘으로 바로 이어서 설명한다.

### 크루스칼(Kruskal) 알고리즘

크루스칼 알고리즘은 그래프의 모든 간선을 가중치의 오름차순으로 정렬한 뒤, 가중치가 작은 간선부터 스패닝 트리에 추가해 나간다. 다만 간선 추가시 이미 스패닝 트리에 포함된 간선들과 사이클을 이룬다면 추가하지 않고 넘어간다. 이러한 탐욕적 선택의 정당성은 쉽게 확인할 수 있다. 가장 작은 가중치의 간선 ***e = (u, v)***를 포함하지 않는 스패닝 트리 ***T***가 있다고 가정하면, ***T***에서 ***u***와 ***v***를 연결하는 경로를 이루는 간선들의 가중치는 모두 ***e*** 보다 크거나 같다. 이 간선들 중 하나를 빼고 ***e***를 스패닝 트리에 추가해도 가중치는 증가하지 않고 최소 스패닝 트리를 이룬다. 따라서 가장 작은 가중치를 선택하는 것이 최소 스패닝 트리를 구성할 수 있다는 것을 확인할 수 있다. 이미 연결된 정점들을 하나의 정점이라고 생각하면 최적 부분 구조 또한 성립함을 알 수 있다. 

크루스칼 알고리즘을 구현할 때, 간선 ***(u, v)***를 추가했을 때, 그래프가 사이클을 이루는지 확인하는데 UnionFind 자료 구조를 사용하면 효율적으로 이를 확인할 수 있다. UnionFind 자료 구조의 연산은 사실상 상수 시간이기 때문에 알고리즘의 시간복잡도는 간선을 정렬하는데 걸리는 시간 $$O(E \space log_2{V})$$과 같다.

``` c++
struct UnionFind;

const int V;
vector<pair<int, int>> adj[MAX_V];

void kruskal() {
  vector<tuple<int, int, int>> edges;
  for (int u = 0; u < V; u++) {
    for (auto& p : adj[u]) {
      int v = p.first;
      int w = p.second;
      edges.push_back(make_tuple(w, u, v));
    }
  }
  sort(edges.begin(), edges.end());
  
  vector<pair<int, int>> selected; // edge set that forms the minimum spanning tree
  UnionFind sets(V);
  for (auto& tp : edges) {
    int cost, u, v;
    tie(cost, u, v) = tp;
    if (sets.are_same_set(u, v) == false) {
      sets.merge(u, v);
      selected.push_back(make_pair(u, v));
    }
  }
}
```

### 프림(Prim) 알고리즘

프림 알고리즘은 하나의 시작점으로 구성된 트리에서부터 시작해 트리와 연결된 간선들 중 사이클을 만들지 않는 최소 가중치 간선을 추가해나가면서 최소 스패닝 트리를 만든다. 시간복잡도는  $$O(E \space log_2{V})$$로 크루스칼 알고리즘과 같다.

``` c++
void prim() {
  int mst_cost = 0;
  priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
  vector<bool> taken(V, 0);
  taken[0] = true;
  for (auto& p : adj[0]) {
    int there = p.first;
    int ncost = p.second;
    if (!taken[there]) pq.push(make_pair(ncost, there));
  }
  while (!pq.empty()) {
    int here = pq.top().first;
    int cost = pq.top().second;
    pq.pop();
    if (!taken[here]) {
      mst_cost += cost;
      taken[here] = true;
      for (auto& p : adj[here]) {
        int there = p.first;
        int ncost = p.second;
        if (!taken[there]) pq.push(make_pair(ncost, there));
      }
    }
  }
}
```

## 연습 문제

**BOJ 1922 네트워크 연결 ([문제 보기](https://www.acmicpc.net/problem/1922))**

**BOJ 2887 행성 터널 ([문제 보기](https://www.acmicpc.net/problem/2887))**

**BOJ 1647 도시 분할 계획 ([문제 보기](https://www.acmicpc.net/problem/1647))**

## 참고 자료

- 프로그래밍 대회에서 배우는 알고리즘 문제해결전략, 구종만
- Competive Programming 3, Steven Halim









