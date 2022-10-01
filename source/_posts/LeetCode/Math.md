# Math

* [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)(数字根%9同余)

* #### [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)

* #### [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

* #### [9. 回文数](https://leetcode.cn/problems/palindrome-number/)

* #### [89. 格雷编码](https://leetcode.cn/problems/gray-code/)

* #### [172. Factorial Trailing Zeroes](https://leetcode.cn/problems/factorial-trailing-zeroes/)

* #### [166. Fraction to Recurring Decimal](https://leetcode.cn/problems/fraction-to-recurring-decimal/)



## 进制相关

> 逆序求和, 按进制进位, 结果转置即可

* [67. 二进制求和](https://leetcode.cn/problems/add-binary/)
* [415. 字符串相加](https://leetcode.cn/problems/add-strings/)
* #### [43. 字符串相乘](https://leetcode.cn/problems/multiply-strings/)

## 约瑟夫环 

> * 数组模拟
> * 环形链表模拟
> * 数学方法
> * 扩展：找最后k个剩余的人

* [1823. 找出游戏的获胜者](https://leetcode-cn.com/problems/find-the-winner-of-the-circular-game/)
* [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)


## 最大公因数和最小公倍数
>欧几里得算法（辗转相除法）

```cpp
//最大公因数
int GCD(int a, int b){
	if(b == 0) return a;
	else return GCD(b, a%b);
}
//最小公倍数
int LCM(int a,int b){
    int gcd = GCD(a,b);
    int lcm = a/gcd*b;
    return lcm;
}
```
## 约数

> 试除法

```cpp
vector<int> getFactors(int n) {
    vector<int> res;
    for (int i = 1; i <= n / i; ++i) {
        if (n % i == 0) {
           res.emplace_back(i);
           if (n / i != i) {
               res.emplace_back(n / i);
           }
        }
    }
    return res;
}
```

## 取模
* [168. Excel表列名称](https://leetcode-cn.com/problems/excel-sheet-column-title/)





#### [357. 统计各位数字都不同的数字个数](https://leetcode.cn/problems/count-numbers-with-unique-digits/)
