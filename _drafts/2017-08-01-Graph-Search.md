---
layout: post
title: 그래프 알고리즘 - 그래프 탐색 (Graph Search)
category: problem solving
tag: [graph search]
---
* list element with functor item
{:toc}

# (1) 깊이우선탐색 (Depth First Search, DFS)

``` c++
vector<int> adj[MAX_V];
vector<bool> visited[MAX_V]; // initially set all false

void dfs(int u) {
    visited[u] = true;
    cout << "visit vertex " << u << endl;
    for (int v: adj[u]) {
        if (!visited[v]) {
            dfs(v);
        }
    }
}
```

# (2) 너비우선탐색 (Breadth First Search,BFS)

``` c++
int V;
vector<int> adj[MAX_V];

void bfs(int source) {
    vector<int> dist(V, INF);
    dist[source] = 0;
    cout << "visit vertex 0 (Layered 0)" << endl;;
    queue<int> q;
    q.push(source);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (dist[v] == INF) {
                dist[v] = dist[u] + 1;
                cout << "visit vertex " << v << " (Layered " << dist[v] << ")" << endl;
                q.push(v);
            }
        }
    }
}
```

# (3) 연결 요소 (Connected Components)

무방향 그래프에서의 연결 요소 찾기

``` c++
int V;
vector<int> adj[MAX_V];
bool visited[MAX_V];

void dfs(int u) {
    visited[u] = true;
    cout << u << ' ';
    for (int v : adj[u])
        if (!visited[v])
            dfs(v);
}
void find_connected_components() {
    int cnt = 0;
    for (int i = 0; i < V; i++) {
        if (!visited[i]) { // visited[]: initially all false
            cout << "Connected Component " << cnt++ << ": ";
            dfs(i);
            cout << "\n";
        }
    }
}
```

# (4) 위상 정렬 (Topological Sort)

DAG(Directed Asyclic Graph)에서의 위상 정렬

``` c++
int V;
vector<int> adj[MAX_V];
vector<int> order;
bool visited[MAX_V];

void dfs(int u) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v);
        }
    }
    order.push_back(u);
}
void topological_sort() {
    for (int i = 0; i < V; i++)
        if (!visited[i])
            dfs(i);
    reverse(order.begin(), order.end());
    cout << "order: ";
    for (int i = 0; i < order.size(); i++)
        cout << order[i] << ' ';
    cout << endl;
}
```

# (5) 이분 그래프 (Bipartite Graph)

이분 그래프인지 검사하기

```c++
int V;
vector<int> adj[MAX_V];
int color[MAX_V];

bool set_color(int u, int u_color) {
    color[u] = u_color;
    for (int v : adj[u]) {
        if (color[v] == INF && !set_color(v, 1 - color[u]))
            return false;
        else if (color[v] == color[u])
            return false;
    }
    return true;
}
bool is_bipartite() {
    for (int i = 0; i < V; i++)
        color[i] = INF;
    for (int i = 0; i < V; i++)
        if (color[i] == INF && !set_color(i, 0))
            return false;
    return true;
}
```

# (6) DFS 스패닝 트리 (DFS Spanning Tree)

DFS를 응용하려 문제를 해결할 때 알아두면 좋은 개념. 특히 단절점, 단절선, 강한 연결 요소를 찾는 알고리즘들을 이해하는데 도움이 된다.

```c++
int V;
vector<int> adj[MAX_V];
int discovered[MAX_V]; // initially set all -1
bool finished[MAX_V];
int counter = 0;

void dfs_spanning_tree(int u) { // in directed graph
    discovered[u] = counter++;
    for (int v : adj[u]) {
        cout << "edge(" << u << ", " << v << ") is a ";
        if (discovered[v] == -1) {
            cout << "tree edge.\n";
            dfs_spanning_tree(v);
        }
        else if (discovered[u] < discovered[v])
            cout << "forward edge.\n";
        else if (!finished[v])
            cout << "back edge.\n";
        else
            cout << "cross edge.\n";
    }
    finished[u] = true;
}
```

# (7) 단절점 (Articulation Points = Cut Vertices)

```c++
int V;
vector<int> adj[MAX_V];
int discover[MAX_V];
bool cut_vertex[MAX_V];
int counter = 0;

int find_cut_vertices(int u, bool root = true) {
    discover[u] = counter++;
    int minback = discover[u];
    int children = 0;
    for (int v : adj[u]) {
        if (discover[v] == -1) {
            children++;
            int subtree = find_cut_vertices(v, false);
            if (!root && subtree >= discover[u]) {
                cut_vertex[u] = true;
            }
            minback = min(minback, subtree);
        } else {
            minback = min(minback, discover[v]);
        }
    }
    if (root && children >= 2) cut_vertex[u] = true;
    return minback;
}
```

# (8) 단절선 (Bridges)

```c++
int V;
vector<int> adj[MAX_V];
int discover[MAX_V];
vector<pair<int, int>> bridges;
int counter = 0;

int find_bridges(int u, int parent = -1) {
    discover[u] = counter++;
    int minBack = discover[u];
    for (int v : adj[u]) {
        if (v == parent) continue;
        if (discover[v] == -1) {
            int subtree = find_bridges(v, u);
            if (subtree > discover[u]) {
                bridges.push_back(make_pair(u, v));
            }
            minBack = min(minBack, subtree);
        } else {
            minBack = min(minBack, discover[v]); 
        }
    }
    return minBack;
}
```

#  (9) 강한 연결 요소 (Strongly Connected Components)
코사라주 알고리즘(Kosaraju's algorithm)은 두 번의 DFS (정방향 DFS 한 번, 역방향 DFS 한 번)로 방향 그래프의 강한 연결 요소들을 구분한다.

```c++
int V;
vector<int> adj[MAX_V];
vector<int> radj[MAX_V];
vector<int> sid, order;
vector<bool> visited;

void dfs(int u) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v);
        }
    }
    order.push_back(u);
}
void rdfs(int u, int id) {
    sid[u] = id;
    for (int v : radj[u]) {
        if (sid[v] == -1) {
            rdfs(v, id);
        }
    }
}
int scc() {
    visited.assign(V, false);
    for (int u = 0; u < V; u++) {
        if (!visited[u]) {
            dfs(u);
        }
    }
    reverse(order.begin(), order.end());
    int id = 0;
    sid.assign(V, -1);
    for (int u : order)  {
        if (sid[u] == -1) {
            rdfs(u, id++);
        }
    }
    return id;
}
```

