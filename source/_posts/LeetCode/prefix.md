# 前缀和
应用场景：不变区间, 区间求和.

## 一维: 数组

* [325. 和等于 k 的最长子数组长度](https://leetcode.cn/problems/maximum-size-subarray-sum-equals-k/)
* [523. 连续的子数组和](https://leetcode.cn/problems/continuous-subarray-sum/)
* [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

写下来不难发现, 这几个题目都是取左闭右开的区间, 即 `(prev, i]`. 

1. 对于统计长度的题目, 需要初始化哈希表`prefix[0] = -1`;

2. 对于统计个数的题目, 需要初始为`prefix[0] = 1`

此外, 也可以是特殊的区间和, 比如: 权值为1-次数.

* [1248. 统计「优美子数组」](https://leetcode.cn/problems/count-number-of-nice-subarrays/)
* [525. 连续数组](https://leetcode.cn/problems/contiguous-array/)
* [1371. 每个元音包含偶数次的最长子字符串](https://leetcode.cn/problems/find-the-longest-substring-containing-vowels-in-even-counts/)

并且, 可以搭配一些定理来使用, 如: **同余定理**

* [974. 和可被 K 整除的子数组](https://leetcode.cn/problems/subarray-sums-divisible-by-k/)



## 二维: 矩阵

* [304. 二维区域和检索 - 矩阵不可变](https://leetcode.cn/problems/range-sum-query-2d-immutable/)
* [1314. 矩阵区域和](https://leetcode.cn/problems/matrix-block-sum/)

以上两个题目都是二维前缀和, 除此之外, 可以将二维和压缩为一维, 当成数组前缀和来做

* [85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)
* [1074. 元素和为目标值的子矩阵数量](https://leetcode.cn/problems/number-of-submatrices-that-sum-to-target/)
* [363. 矩形区域不超过 K 的最大数值和](https://leetcode.cn/problems/max-sum-of-rectangle-no-larger-than-k/)
* [面试题 17.24. 最大子矩阵](https://leetcode.cn/problems/max-submatrix-lcci/)



## 推广: 前缀异或-前缀乘积

* [1310. 子数组异或查询](https://leetcode-cn.com/problems/xor-queries-of-a-subarray/)

除了上述题目的左开右闭区间, `(prev, i]`, 还有左闭右开`[prev, i)`, 具体取决于题意.

此时, 就不需要进行`prefix[0] = **`的预处理.

* [1442. 形成两个异或相等数组的三元组数目](https://leetcode.cn/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/)

* [1352. 最后 K 个数的乘积](https://leetcode.cn/problems/product-of-the-last-k-numbers/)

  



