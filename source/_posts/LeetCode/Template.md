# Frequent Template

## Shortest Path

### Dijkstra

```cpp
class Solution {
public:
    vector<int> getPath(vector<int>& path, int src, int tar) { //nodes from 1 -> n;
        vector<int> nodeSeq;
        int cur = tar;
        while (path[cur] != -1) {
            nodeSeq.emplace_back(cur);
            cur = path[cur];
        }
        nodeSeq.emplace_back(src);
        reverse(nodeSeq.begin(), nodeSeq.end());
        return nodeSeq;
    }
    int networkDelayTime(vector<vector<int>>& times, int n, int src) { //nodes from 1 -> n;
        const int Inf = INT_MAX / 2;
        vector<vector<int>> graph(n, vector<int> (n, Inf));
        for (const auto& edge: times) {
            int from = edge[0] - 1, to = edge[1] - 1, cost = edge[2];
            graph[from][to] = cost;
        }
        for (int i = 0; i < n; ++i) {
            graph[i][i] = 0;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        vector<int> dist(n, Inf);
        vector<bool> visited(n, false);
        vector<int> path(n, -1);
        pq.emplace(0, src - 1);
        dist[src - 1] = 0;
        while (!pq.empty()) {
            pair<int, int> p = pq.top(); pq.pop();
            int cur = p.second;
            if (visited[cur]) {//add visited array to avoid useless traversal
                continue;
            }
            visited[cur] = true;
            for (int i = 0; i < n; ++i) {
                if (dist[i] > dist[cur] + graph[cur][i]) {
                    dist[i] = dist[cur] + graph[cur][i];
                    pq.emplace(dist[i], i);
                    path[i] = cur;
                } 
            }
        }
        vector<int> pathNodes = getPath(path, src - 1);
        for (auto node: pathNodes)
            cout << node + 1 << ' ';

        int maxDistance = *max_element(dist.begin(), dist.end());
        return maxDistance == Inf ? -1 : maxDistance;
    }
};
```

### Floyd

```cpp
vector<int> getPath();

int Floyd(vector<vector<int>>& times, int n, int src) { //nodes from 1 -> n;
    const int Inf = INT_MAX / 2;
    vector<vector<int>> graph(n, vector<int> (n, Inf));
    for (int i = 0; i < n; ++i) {
        graph[i][i] = 0;
    }
    for (const auto& edge: times) {
        int from = edge[0] - 1, to = edge[1] - 1, cost = edge[2];
        graph[from][to] = cost;
    }

    for (int k = 0; k < n; ++k) {
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j]);
            }
        }
    }

    int maxDistance = *max_element(graph[src - 1].begin(), graph[src - 1].end());
    return maxDistance == Inf ? -1 : maxDistance;
}
```

### SPFA

```cpp
int SPFA(vector<vector<int>>& edges, int n, int src) { //nodes from 1 -> n;
    const int Inf = INT_MAX / 2;
    vector<vector<int>> graph(n, vector<int> (n, Inf));
    for (int i = 0; i < n; ++i) {
        graph[i][i] = 0;
    }
    for (const auto& edge: edges) {
        int from = edge[0] - 1, to = edge[1] - 1, cost = edge[2];
        graph[from][to] = cost; 
    }

    vector<int> dist(n, Inf);
    vector<bool> inQ(n, false);
    dist[src - 1] = 0;
    inQ[src - 1] = true;
    queue<int> Q;
    Q.emplace(src - 1);
    while (!Q.empty()) {
        int cur = Q.front(); Q.pop();
        inQ[cur] = false;
        for (int i = 0; i < n; ++i) {
            if (dist[i] > dist[cur] + graph[cur][i]) {
                dist[i] = dist[cur] + graph[cur][i];
                if (!inQ[i]) {
                    Q.emplace(i);
                    inQ[i] = true;
                }
            }
        }
    }
    int maxDistance = *max_element(dist.begin(), dist.end());
    return maxDistance == Inf ? -1 : maxDistance;
}
```

```cpp
```



## Minimum Spanning Tree

### Prim

```cpp
int Prim(vector<vector<int>>& edges, int n) {
    const int Inf = INT_MAX / 2;
    vector<vector<int>> graph(n, vector<int> (n, Inf));
    for (int i = 0; i < n; ++i) {
        graph[i][i] = 0;
    }
    for (const auto& edge: edges) {
        int p1 = edge[0] - 1, p2 = edge[1] - 1, cost = edge[2];
        graph[p1][p2] = graph[p2][p1] = cost;
    }
    vector<bool> visited(n, false);
    vector<int> dist(n, Inf);
    dist[0] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.emplace(0, 0);
    int res = 0;
    int connectedNum = 0;
    while(!pq.empty()) {
        pair<int, int> p = pq.top(); pq.pop();
        int cur = p.second;
        if (visited[cur]) {
            continue;
        }
        ++connectedNum;
        res += p.first;
        visited[cur] = true;
        for (int i = 0; i < n; ++i) {
            if (dist[i] > graph[cur][i]) {
                dist[i] = graph[cur][i];
                pq.emplace(dist[i], i);
            }
        }
    }
    return n == connectedNum ? res : - 1;
}
```

### Kruskal

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    int find(int i) {
        if (i != parent[i]) {
            parent[i] = find(parent[i]);
        }
        return parent[i];
    }

    bool unite(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] <= rank[rootY]) {
                if (rank[rootX] == rank[rootY]) {
                    ++rank[rootY];
                }
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
            }
            return true;
        }
        return false;
    }

private:
    vector<int> parent;
    vector<int> rank;
};

int Kruskal(vector<vector<int>>& edges, int n) {
    sort(edges.begin(), edges.end(), [](const vector<int>& edge1, const vector<int>& edge2) {
        return edge1[2] < edge2[2];
    });

    int connectedNum = 1;
    int res = 0;
    UnionFind uf(n);
    for (const auto& edge: edges) {
        int p1 = edge[0] - 1, p2 = edge[1] - 1, cost = edge[2];
        if (uf.unite(p1, p2)) {
            res += cost;
            ++connectedNum;
        }
    }
    return connectedNum == n ? res : -1;
}
```



```cpp
int SPFA(vector<vector<int>>& edges, int n, int src) { //nodes from 1 -> n;
    const int Inf = INT_MAX / 2;
    vector<vector<int>> graph(n, vector<int> (n, Inf));
    for (int i = 0; i < n; ++i) {
        graph[i][i] = 0;
    }
    for (const auto& edge: edges) {
        int from = edge[0] - 1, to = edge[1] - 1, cost = edge[2];
        graph[from][to] = cost; 
    }

    vector<int> dist(n, Inf);
    vector<bool> inQ(n, false);
    dist[src - 1] = 0;
    inQ[src - 1] = true;
    queue<int> Q;
    Q.emplace(src - 1);
    while (!Q.empty()) {
        int cur = Q.front(); Q.pop();
        inQ[cur] = false;
        for (int i = 0; i < n; ++i) {
            if (dist[i] > dist[cur] + graph[cur][i]) {
                dist[i] = dist[cur] + graph[cur][i];
                if (!inQ[i]) {
                    Q.emplace(i);
                    inQ[i] = true;
                }
            }
        }
    }
    int maxDistance = *max_element(dist.begin(), dist.end());
    return maxDistance == Inf ? -1 : maxDistance;
}
```





## Segment Tree

### 区间加法

 > `tag[]`表示加法懒标记

```cpp
//add to the interval block
#include<bits/stdc++.h>

using namespace std;
int n, m;
typedef long long ll;
const int MAXN = 1e5 + 10;
ll arr[MAXN], sum[MAXN<<2], tag[MAXN<<2];

inline int lc(int p) {
    return p << 1;
}

inline int rc(int p) {
    return p << 1 | 1;
}

void pushUp(int p) {
    sum[p] = sum[lc(p)] + sum[rc(p)];
}

void build(int p, int l, int r) {
    if (l == r) {
        sum[p] = arr[l];
        return;
    }
    int mid = ((r - l) >> 1) + l;
    build(lc(p), l, mid);
    build(rc(p), mid + 1, r);
    pushUp(p);
}

void lazy(int p, ll val, int l, int r) {
    tag[p] += val;
    sum[p] += val * (r - l + 1);
}

void pushDown(int p, int l, int r) {
    int mid = ((r - l) >> 1) + l;
    lazy(lc(p), tag[p], l, mid);
    lazy(rc(p), tag[p], mid + 1, r);
    tag[p] = 0;
}

void update(int p, ll val, int updateL, int updateR, int l, int r) {
    if (updateL <= l && r <= updateR) {
        lazy(p, val, l, r);
        return;
    }
    int mid = ((r - l) >> 1) + l;
    if (tag[p] != 0) {
        pushDown(p, l, r);
    }
    if (updateL <= mid) update(lc(p), val, updateL, updateR, l, mid);
    if (updateR > mid) update(rc(p), val, updateL, updateR, mid + 1, r);
    pushUp(p);
}

ll query(int p, int queryL, int queryR, int l, int r) {
    if (queryL <= l && r <= queryR) {
        return sum[p];
    }
    int mid = ((r - l) >> 1) + l;
    if (tag[p] != 0) {
        pushDown(p, l, r);
    }
    ll res = 0;
    if (queryL <= mid) res += query(lc(p), queryL, queryR, l, mid);
    if (queryR > mid) res += query(rc(p), queryL, queryR, mid + 1, r);
    return res;
}
int main() {
    int l, r;
    ll val;
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) {
        scanf("%lld", &arr[i]);
    }
    build(1, 1, n);
    while (m--) {
        int choice; cin >> choice;
        if (choice == 1) {
            scanf("%d%d%lld", &l, &r, &val);
            update(1, val, l, r, 1, n);
        } else if (choice == 2) {
            scanf("%d%d", &l, &r);
            printf("%lld\n",query(1, l, r, 1, n));
        }
    }
    return 0;
}
```

### 区间乘法和加法

> `mul[]`和`add[]`

```cpp
#include<bits/stdc++.h>

using namespace std;
int n, m, mod;
typedef long long ll;
const int MAXN = 1e5 + 10;
ll arr[MAXN], sum[MAXN<<2], add[MAXN<<2], mul[MAXN<<2];

inline int lc(int p) {
	return p << 1;
}

inline int rc(int p) {
	return p << 1 | 1;
}

void pushUp(int p) {
	sum[p] = (sum[lc(p)] + sum[rc(p)]) % mod; 
}

void build(int p, int l, int r) {
	mul[p] = 1;
	if (l == r) {
		sum[p] = arr[l];
		return;
	}
	int mid = ((r - l) >> 1) + l;
	build(lc(p), l, mid);
	build(rc(p), mid + 1, r);
	pushUp(p);
}


void pushDown(int p, int l, int r) {
	int mid = ((r - l) >> 1) + l;
	sum[lc(p)] = (sum[lc(p)] * mul[p] + add[p] * (mid - l + 1)) % mod;
	sum[rc(p)] = (sum[rc(p)] * mul[p] + add[p] * (r - mid)) % mod;
	mul[lc(p)] = (mul[p] * mul[lc(p)]) % mod;
	mul[rc(p)] = (mul[p] * mul[rc(p)]) % mod;
	add[lc(p)] = (add[p] + add[lc(p)] * mul[p]) % mod;
 	add[rc(p)] = (add[p] + add[rc(p)] * mul[p]) % mod;
 	
	add[p] = 0;
	mul[p] = 1;
}

void updateMul(int p, ll k, int updateL, int updateR, int l, int r) {
	if (updateL <= l && r <= updateR) {
		add[p] = (add[p] * k) % mod;
		mul[p] = (mul[p] * k) % mod;
		sum[p] = (sum[p] * k) % mod;
		return;
	}
	int mid = ((r - l) >> 1) + l;
	if (add[p] != 0 || mul[p] != 1)  {
		pushDown(p, l, r);
	}
	if (updateL <= mid) updateMul(lc(p), k, updateL, updateR, l, mid);
	if (updateR > mid) updateMul(rc(p), k, updateL, updateR, mid + 1, r);
	pushUp(p); 
}

void updateAdd(int p, ll val, int updateL, int updateR, int l, int r) {
	if (updateL <= l && r <= updateR) {
		add[p] = (add[p] + val) % mod;
		sum[p] = (sum[p] + val * (r - l + 1)) % mod; 
		return;
	}
	int mid = ((r - l) >> 1) + l;
	if (add[p] != 0 || mul[p] != 1) {
		pushDown(p, l, r);
	}
	if (updateL <= mid) updateAdd(lc(p), val, updateL, updateR, l, mid);
	if (updateR > mid) updateAdd(rc(p), val, updateL, updateR, mid + 1, r);
	pushUp(p);
}

ll query(int p, int queryL, int queryR, int l, int r) {
	if (queryL <= l && r <= queryR) {
		return sum[p];
	}
	int mid = ((r - l) >> 1) + l;
	if (add[p] != 0 || mul[p] != 1) {
		pushDown(p, l, r);
	}
	ll res = 0;
	if (queryL <= mid) res += query(lc(p), queryL, queryR, l, mid) % mod;
	if (queryR > mid) res += query(rc(p), queryL, queryR, mid + 1, r) % mod;
	return res % mod;
}

int main() {
	int l, r;
	ll val;
	cin >> n >> m >> mod;
	for (int i = 1; i <= n; ++i) {
		scanf("%lld", &arr[i]);
	}
	build(1, 1, n);
	while (m--) {
		int choice; cin >> choice;
		if (choice == 1) {
			scanf("%d%d%lld", &l, &r, &val);
			updateMul(1, val, l, r, 1, n);
		} else if (choice == 2) {
			scanf("%d%d%lld", &l, &r, &val);
			updateAdd(1, val, l, r, 1, n);
		} else if (choice == 3) {
			scanf("%d%d", &l, &r);
			printf("%lld\n",query(1, l, r, 1, n));
		}
	}
	return 0;
}
```

## Binary Indexed Tree

```cpp
class BIT {
public:
    BIT(int n) {
        tree.resize(n + 1, 0);
    }
    static int lowBit(int x) {
        return x  & (-x);
    }

    void add(int x, int val) { 
        while (x < tree.size()) {
            tree[x] += val;
            x += lowBit(x);
        }
    }

    int query(int x) { // sum from tree[1] to tree[x].
        int res = 0;
        while (x > 0) {
            res += tree[x];
            x -= lowBit(x);
        }
        return res;
    }
    
    int query(int l, int r) {  // [l, r];
        return query(r) - query(l - 1);
    }
private:
    vector<int> tree; //tree index from 1 -> n
};
```

## Quick Power/Multiply

**Quick Power**

```cpp
double binaryExponentiation(double x, long long n) {
    if (n == 0) {
		return 1.0;
    }
    double half = binaryExponentiation(x, n >> 1);
    return n & 1? half * half * x : half * half;
}

double iteration(double x, long long n) {
	double res = 1.0;
    double xContribution = x;
    while (n > 0) {
		if (n & 1) {
            res *= xContribution;
        }
        xContribution *= xContribution;
        n >>= 1;
    }
    return res;
}
```

**Quick Multiply**

```cpp
long long quickMul(long long x, long long y) {
    long long res = 0;
    while (y != 0) {
        if (y & 1) {
            res += x;
        }
        x += x;
        y >>= 1;
    }
    return res;
}
```

