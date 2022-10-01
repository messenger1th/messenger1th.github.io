# Union-Find

也称Disjoint-set structure。

## 基于树

数组实现树 + 按rank合并 + 路径压缩

```cpp
class UnionFind{
public:
    UnionFind(int n): setNum(n), rank(n, 1) {
        parent.reserve(n);
        for (int i = 0; i < n; ++i) {
            parent.emplace_back(i);
        }
    }

    int find(int x) {
        if (x != parent[x]) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    void unite(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            --setNum;
            if (rank[rootX] == rank[rootY]) {
                parent[rootX] = rootY;
                ++rank[rootY];
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
            }
        }
    }

    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }

    int getsetNum() {
        return setNum;
    }
private:
    vector<int> parent;
    vector<int> rank;
    int setNum;
};
```

时间复杂度

1. `find()`: $O(1)$。比对数小，接近线性。但多次操作后，经路径压缩变成 $O(1)$;
2. `unite(int x, int y)`: $O(n)$

空间复杂度：$O(n)$



## 基于链表

```cpp
class Node{
public:
    Node* nodeSet;
    Node* prev;
    Node* next;
    Node() {
        this->prev = this->next = this->nodeSet = this;
    }
};


class UnionFind{
public:
    UnionFind(int n):nodes(n), SetNum(n) {}

    bool isSameSet(int x, int y) {
        return nodes[x].nodeSet == nodes[y].nodeSet;
    }

    void unite(int x, int y) {
        if (!isSameSet(x, y)) {
            Node* xHead = nodes[x].nodeSet;
            Node* yHead = nodes[y].nodeSet;
            Node* xTail = xHead->prev;
            Node* yTail = yHead->prev;

            xTail->next = yHead;
            yHead->prev = xTail;
            
            xHead->prev = yTail;
            yTail->next = xHead;

            Node* p = yHead;
            while (p != xHead) {
                p->nodeSet = xHead;
                p = p->next;
            }
            --SetNum;
        }
    }


private:
    vector<Node> nodes;
    int SetNum;
};
```

时间复杂度：

1. `isSameSet()`: $O(1)$
2. `unite(int x, int y)`: $O(n)$

空间复杂度：$O(n)$



## 题目

* [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

* [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

* [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

* [685. 冗余连接 II](https://leetcode-cn.com/problems/redundant-connection/)

* [778. 水位上升的泳池中游泳](https://leetcode-cn.com/problems/swim-in-rising-water/)

* [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)、

  

