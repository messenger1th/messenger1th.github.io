## 后缀数组

* #### [1044. Longest Duplicate Substring](https://leetcode.cn/problems/longest-duplicate-substring/)



## 树哈希
* [572. 另一棵树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)
```CPP
//这里采用的哈希公式是：h(X) = Xv + mb + lb * h(Xl) + rb * h(Xr), 且h(nullptr) = 1
constexpr uint64_t lb = 2333, rb = 97755331, mb = 23333;
class Solution {
public:
    int trans(TreeNode* root){
        return !root ? 1 : root->val = lb * trans(root->left) + rb * trans(root->right) + root->val + mb;
    }
    bool dfs(TreeNode* root, int k){
        return root && (root->val == k || dfs(root->left, k) || dfs(root->right, k));
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        return dfs((trans(root), root), trans(subRoot));
    }
};
```
## Rabin-Karp算法: 
> 子字符串匹配、优化散列查找

* [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

* #### [1044. Longest Duplicate Substring](https://leetcode.cn/problems/longest-duplicate-substring/)




## 快速幂和快速乘
* [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

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

* [29. 两数相除](https://leetcode-cn.com/problems/divide-two-integers/)

```cpp
long mul(int x, long n) {
	long res = 0;
    while (n > 0) {
        if (n & 1) {
            res += x;
        }
        n >> 1;
        x += x;
    }
    return res;
}
```

* [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

## Manacher

* [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

## 区间查询修改

* 数组不变，区间查询：__前缀和__、树状数组、线段树；
* 数组单点修改，区间查询：__树状数组__、线段树；
* 数组区间修改，单点查询：__差分__、线段树；
* 数组区间修改，区间查询：__线段树__。

### 前缀和 [前缀和](./prefix.md)




### 差分数组
>应用场景：区间修改, 单点查询。
#### 模板题

* [1893. 检查是否区域内所有整数都被覆盖](https://leetcode-cn.com/problems/check-if-all-the-integers-in-a-range-are-covered/)
* [1109. 航班预订统计](https://leetcode-cn.com/problems/corporate-flight-bookings/)
* [370. 区间加法](https://leetcode.cn/problems/range-addition/)

#### 变式

* [995. K 连续位的最小翻转次数](https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/)


### 树状数组

> 单点修改, 区间查询

*  [307. 区域和检索 - 数组可修改](https://leetcode-cn.com/problems/range-sum-query-mutable/)

待学习: [【宫水三叶の相信科学系列】二维最长上升子序列：朴素 DP & 二分 DP（含证明）& 树状数组 DP - 俄罗斯套娃信封问题 - 力扣（LeetCode）](https://leetcode.cn/problems/russian-doll-envelopes/solution/zui-chang-shang-sheng-zi-xu-lie-bian-xin-6s8d/)

### 分段树（线段树）segment tree

> 区间修改, 区间查询.
* 以上区间求和题目都能做, 但不是最优.



## 启发式搜索

### A Star

> 最好提前判断是否有解, 无解情况需要遍历所有可能.

1. 设置`f()`作为启发式函数, 用于计算于终点的最短距离.
2. 用优先队列存储过程, 优先取出与终点理论距离最短, 即`f()`算出来最短的那个中间值.

* [433. Minimum Genetic Mutation](https://leetcode.cn/problems/minimum-genetic-mutation/)

**待学习**:

* [打开转盘锁 - 打开转盘锁 ](https://leetcode.cn/problems/open-the-lock/solution/da-kai-zhuan-pan-suo-by-leetcode-solutio-l0xo/)

### IDA Star

>  相对于 A*算法，它的优势如下：

1. 不需要判重，不需要排序；
2. 空间需求减少。

**待学习**:

* [【宫水三叶】一题三解：「双向 BFS」& 「AStar 算法」&「IDA* 算法」 - 打开转盘锁 ](https://leetcode.cn/problems/open-the-lock/solution/gong-shui-san-xie-yi-ti-shuang-jie-shuan-wyr9/)





## 字符串哈希

* #### [1044. Longest Duplicate Substring](https://leetcode.cn/problems/longest-duplicate-substring/)
