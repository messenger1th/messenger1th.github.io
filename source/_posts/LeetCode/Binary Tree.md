# 树：Tree

## 遍历

* 前序遍历
* 中序遍历
* 后序遍历
* 层序遍历
>递归和迭代的写法

>后序结果可由前序改变入栈顺序，翻转结果数组得来。**_但这种方法不是真正意义上的后序遍历:左—>右->中_** 但是其实遍历本来就要用root节点做媒介，也先访问了root节点。


### 遍历题目

### 前中后序

> 递归和迭代的写法.

* [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
* [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
* [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

### 遍历应用

* [129. 求根节点到叶节点数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)

### 层序遍历

* [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)
* [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)
* [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-cameras/)
* [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

## 特殊种类
* 满二叉树
* 完全二叉树
* 二叉搜索树
* 平衡二叉树
* 平衡二叉搜索树
>善用特殊树各种性质解题

### 完全二叉树
* [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)
* [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)
### 平衡二叉树
* [110.平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
### 二叉搜素树

> inOrder有序

* [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)
* [98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
* [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)
* [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)
* [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
* [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
* [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
* [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)
* [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
* [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
* [1382. 将二叉搜索树变平衡](https://leetcode-cn.com/problems/balance-a-binary-search-tree/)
* [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)(dp)
* [653. 两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)(_考虑树双指针双栈实现_)[_Morris+双指针_](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/solution/653-morrisjia-shuang-zhi-zhen-shi-xian-k-r2rm/)
* [783. 二叉搜索树节点最小距离](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/)
## 构造

### 遍历构造

> 迭代法值得考究

* [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
* [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### 数组构造

* [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)
* [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

### 链表构造

* [109. 有序链表转换二叉搜索树](https://leetcode.cn/problems/convert-sorted-list-to-binary-search-tree/)

## 深度
* [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
* [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
## 其他操作
* [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

* [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

* [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

* [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)(回溯)

* [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

* [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

* [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

* [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

* [863. 二叉树中所有距离为 K 的结点](https://leetcode-cn.com/problems/all-nodes-distance-k-in-binary-tree/)


 >插入、删除通常需要用到返回值!!!

## **hard**
* [968. 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)
* [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

## 树形DP

题型

* [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

* 二叉树中找连通块的最大值（Acwing题目）

* [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

#### [285. 二叉搜索树中的中序后继](https://leetcode.cn/problems/inorder-successor-in-bst/)

#### [426. 将二叉搜索树转化为排序的双向链表](https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/)

#### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

#### [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

#### [572. 另一棵树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

#### [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/balance-a-binary-search-tree/)

#### [156. 上下翻转二叉树](https://leetcode.cn/problems/binary-tree-upside-down/)

#### [814. 二叉树剪枝](https://leetcode.cn/problems/binary-tree-pruning/)



> * [968. 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)  类似这题，用返回值告诉子节点状态. *Greedy*