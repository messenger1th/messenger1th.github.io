# 滑动窗口 Sliding Window

> 滑动窗口一定是连续的，因此子数组类题目才能用。也有在数组两头的，一头变长一头减短。
>
> 移动窗口时变动的值最好是与两头的数据直接相关的，像是[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)中取的是最大值，因此需要花$O(k)$的时间去获取最大值，总体为$O（n*k)$时间复杂度，对于Hard题是TEL的。
>
> 一般滑动窗口都是对容器内元素的__个数或者种类__进行限制，例如窗口内不能出现[重复元素](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)，元素种类只能有[$k$种](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)等。

## 固定大小滑动窗口

> 这种是最简单的，只需要对窗口进出元素进行简单运算即可

* [643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)
* [1052. 爱生气的书店老板](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)
* [1423. 可获得的最大点数](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards/)
* [1151. 最少交换次数来组合所有的 1](https://leetcode-cn.com/problems/minimum-swaps-to-group-all-1s-together/)

## 对窗口内元素的个数、种类、和或乘积等进行限制

> __最大最长__，因此每次增长时，窗口内可能不满足条件，要缩小窗口到满足条件才能收集$res$.

* [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
* [1695. 删除子数组的最大得分](https://leetcode-cn.com/problems/maximum-erasure-value/)
* [1208. 尽可能使字符串相等](https://leetcode-cn.com/problems/get-equal-substrings-within-budget/)
* [1493. 删掉一个元素以后全为 1 的最长子数组](https://leetcode-cn.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
* [487. 最大连续1的个数 II](https://leetcode-cn.com/problems/max-consecutive-ones-ii/)
* [1004. 最大连续1的个数 III](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)
* [904. 水果成篮](https://leetcode-cn.com/problems/fruit-into-baskets/)
* [159. 至多包含两个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)
* [340. 至多包含 K 个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)
* [2024. 考试的最大困扰度](https://leetcode-cn.com/problems/maximize-the-confusion-of-an-exam/)
* [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)
* [713. 乘积小于K的子数组](https://leetcode-cn.com/problems/subarray-product-less-than-k/)

## 子串“匹配”

> 通常用一个计数器$differentNum$来判断是否匹配，不用每次都遍历子串来$check$
>
> 字符限定时，可以用静态数组来充当哈希表。

* [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
* [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
* [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

## 求满足条件的最小值

> 窗口内是要求满足条件的，但要求最小值，因此在缩小窗口时收集$res$.

* [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
* [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

#### [995. K 连续位的最小翻转次数](https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/)

#### [30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)
