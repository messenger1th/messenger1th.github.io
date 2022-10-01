# 动态规划

> 与暴力回溯相比，对于计算个数的题目更快。
> 
> 也可记忆化搜索
* [120. 三角形最小路径和](https://leetcode.cn/problems/triangle/)




## 序列和数组
> 序列不要求连续, 子数组要求连续;
>




#### LCS: Longest Common Sequence

* [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)   [1035. 不相交的线](https://leetcode-cn.com/problems/uncrossed-lines/)

#### Others

* [91. 解码方法](https://leetcode.cn/problems/decode-ways/)
* [583. 两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)
* [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

#### 进阶

* [97. 交错字符串](https://leetcode.cn/problems/interleaving-string/)
* [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
* [44. 通配符匹配](https://leetcode.cn/problems/wildcard-matching/)
* [10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)
* [132. 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/)

* #### [446. 等差数列划分 II - 子序列](https://leetcode-cn.com/problems/arithmetic-slices-ii-subsequence/) 



### 最大子数组系列

> 通常要求子数组, 需要一些**trick**来模拟抛弃数组的操作.

* [53. 最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)

* [1186. 删除一次得到子数组最大和](https://leetcode-cn.com/problems/maximum-subarray-sum-with-one-deletion/)

* [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

* #### [918. 环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/)


#### 类似\变式

* [413. 等差数列划分](https://leetcode.cn/problems/arithmetic-slices/)

* [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

#### 进阶: 矩阵上的数组

* #### [面试题 17.24. 最大子矩阵](https://leetcode.cn/problems/max-submatrix-lcci/)

* #### [363. 矩形区域不超过 K 的最大数值和](https://leetcode.cn/problems/max-sum-of-rectangle-no-larger-than-k/)






## 区间dp
> 通常来说，解决区间dp的步骤是
>
> * 找到依赖关系， **这一步决定了dp的维度和循环数**
> * 初始化
> * 枚举长度length  $\to$ n，从小区间j转移到大区间
>
> 区间dp是自底向上的，有些题目是可以从自顶向下转换成区间dp的。
> 
> 记忆化递归是自顶向下的。

* [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

* [375. 猜数字大小 II](https://leetcode-cn.com/problems/guess-number-higher-or-lower-ii/)

* [486. 预测赢家](https://leetcode-cn.com/problems/predict-the-winner/)

* [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)

* [2104. 子数组范围和](https://leetcode-cn.com/problems/sum-of-subarray-ranges/)

* [664. 奇怪的打印机](https://leetcode-cn.com/problems/strange-printer/)

* [87. 扰乱字符串](https://leetcode-cn.com/problems/scramble-string/)

* [546. 移除盒子](https://leetcode-cn.com/problems/remove-boxes/)  3维



## 状态压缩DP

> 由于状态压缩是用一个state来表示状态，因此对n有限制。
>
> 通常可以用暴力回溯解决，但回溯时间复杂度为：$O(n!)$。

* [526. 优美的排列](https://leetcode-cn.com/problems/beautiful-arrangement/)

* [1723. 完成所有工作的最短时间](https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs/)




## 数位dp

* [357. 统计各位数字都不同的数字个数](https://leetcode.cn/problems/count-numbers-with-unique-digits/)
* [233. 数字 1 的个数](https://leetcode.cn/problems/number-of-digit-one/)
* [600. 不含连续1的非负整数](https://leetcode-cn.com/problems/non-negative-integers-without-consecutive-ones/)
* [902. 最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)



## 路径问题

* [931. 下降路径最小和](https://leetcode.cn/problems/minimum-falling-path-sum/)

* [1289. 下降路径最小和  II](https://leetcode.cn/problems/minimum-falling-path-sum-ii/)

* #### [576. 出界的路径数](https://leetcode.cn/problems/out-of-boundary-paths/)

* #### [1575. 统计所有可行路径](https://leetcode.cn/problems/count-all-possible-routes/)

* #### [1301. 最大得分的路径数目](https://leetcode.cn/problems/number-of-paths-with-max-score/) (这个题目中前向和返回两种方式的区别?)



