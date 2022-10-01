# 回溯

> 常常搭配剪枝来提速
>
> 回溯的时间复杂度通常是指数级别的

### 应用范围
* 组合和排列
* 切割
* 子集
* 棋盘
* 路线

## 组合、排列和集合
>代码不同体现在`startIndex`、去重方式、获取结果等方面.

>注意树层和树枝剪枝的区别


#### 组合

* [77. 组合](https://leetcode-cn.com/problems/combinations/)
* [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/) 可复用，注意去重
* [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)
* [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/) 数组元素不唯一且不可复用，注意排序\used数组剪枝。
* [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
#### 排列
* [46. 全排列](https://leetcode-cn.com/problems/permutations/)
* [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)  注意剪枝
#### 集合
* [78. 子集](https://leetcode-cn.com/problems/subsets/)
* [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)  含重复元素，注意剪枝

#### 总结：

* 排列问题需要从`0`开始，组合从`start`开始。

* 组合问题, 如果含重复元素, 要先排序, 后去重, 方法如下.

  * `i > 0 && nums[i] == nums[i - 1] && !used[i - 1]` 
  * `i > startIndex && nums[i] == nums[i - 1] `(**推荐, 无需`used`数组**)

* 排列问题同理, 但是`used`数组必不可少.

* 组合排列问题在叶子节点处收集结果, 子集收集所有路径.



#### 序列（不能排序去重）
* [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

#### 分割
* [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)
* [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

## 路线

* [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

* [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)



## 其他

* [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/) 不同去重逻辑

* [332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

* [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

* [52. N皇后 II](https://leetcode.cn/problems/n-queens-ii/)

* [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

* [301. 删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

* [282. 给表达式添加运算符](https://leetcode-cn.com/problems/expression-add-operators/)

* [212. 单词搜索 II](https://leetcode.cn/problems/word-search-ii/)
