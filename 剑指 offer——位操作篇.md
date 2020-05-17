---
layout: post
title: 剑指 offer——位操作篇
date: 2020-04-05
categories: 算法
tags: [算法, 剑指 offer, 位操作, bit]
excerpt_separator: <!--more-->
mathjax: true
---

<!--more-->
#### 15. 二进制中1的个数
题意：[面试题15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)  
思路：使用位操作，每次计算给定数字的某一个二进制位上是否为1。由于1的二进制表示中，只有末位为1，其余位均为0，所以将给定的数与1进行按位与操作，即可判断其末位上的二进制位是否为1。
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while (n != 0) {
            if ((n & 1) == 1) {
                count ++;
            }
            n = n >>> 1;
        }
        return count;
    }
}
```

#### 16. 数值的整数次方
题意：[面试题16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)  
思路：将n个x相乘，计算 $x^n$ 需要的时间复杂度为O(n)，当n特别大时，不满足时间要求。  
由于 $x^{a+b} = x^a*x^b$，按照这种方式将n写成二进制的形式，并进行拆分，那么只计算二进制位上为1时的乘积，这种算法需要O(log(n))的时间复杂度。如x=2，n=6时：
\[
2^6 = 2^{110}(将6转换成2进制) = 2^{100} * 2^{10} = 2^4 * 2^2
\]
```java
class Solution {
    public double myPow(double x, int n) {
        if (n == 0) {
            return 1;
        }
        boolean negative = n < 0;
        long newN = Math.abs((long)n);
        double res = 1;
        double tmp = x;
        while (newN != 0) {
            if ((newN & 1) == 1) {
                res *= tmp;
            }
            tmp *= tmp;
            newN >>= 1;
        }
        return negative ? 1 / res : res;
    }
}
```
