---
layout: post
title: 剑指 offer——其他
date: 2020-05-17
categories: 算法
tags: [算法, 剑指 offer, 其他，规律]
excerpt_separator: <!--more-->
mathjax: true
---

<!--more-->
#### 43. 1~n整数中1出现的次数
题意：[面试题43. 1～n整数中1出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)  
思路：找规律。每一位digit上的1出现的次数只与:它前面的数字high、它后面的数字low、当前位的因数$10^{digit}$相关。  
分情况讨论：  
1)第i位上的数字 == 0。1～n中，这一位上1出现的次数只与高位有关。（例如2304的第3位为0，1～2304中第3位为1的情况有001x ~ 221x，一共23 * 100个， 即high * $10^{digit}$个）  
2)第i位上的数字 == 1。1～n中，这一位上1出现的次数和高低位都有关系。（例如2314中第3位为1，1～2314中第3位为1的情况有001x ~ 221x,2310 ~ 2314，一共high * $10^{digit}$ + low + 1个）  
3)第i位上的数字 > 1。1～n中，这一位上1出现的次数只与高位有关。（例如2304中的第2位为3，1～2304中第2位为1的情况有01xx~21xx,一共（high + 1) * $10^{digit}$个）
```Java
class Solution {
    public int countDigitOne(int n) {
        int count = 0;
        int high = n / 10;
        int low = 0;
        int cur = n % 10;
        int digit = 1;
        while (high != 0 || cur != 0) {
            if (cur == 0) {
                count += high * digit;
            } else if (cur == 1) {
                count += high * digit + (low + 1);
            } else {
                count += (high + 1) * digit;
            }
            low += cur * digit;
            digit *= 10;
            cur = high % 10;
            high /= 10;
        }
        return count;
    }
}
```

#### 44. 数字序列中某一位的数字
题意：[面试题44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)  
思路：先判断给定的n是否在i位的范围之内（比如n在10以内，是在1位数的范围内；超过10又在100以内，是在2位数的范围内……以此类推），然后计算n是i位数的第几个数的第几位。
```Java
class Solution {
    List<Integer> list = Arrays.asList(0, 10, 100, 1_000, 10_000,
    100_000, 1_000_000, 10_000_000, 100_000_000, 1_000_000_000);

    public int findNthDigit(int n) {
        int bit = 1;
        long len;
        while (n > 0) {
            len = list.get(bit) - list.get(bit - 1);
            if (len * bit >= n) {
                break;
            }
            n -= len * bit;
            bit ++;
        }
        int num = list.get(bit - 1) + n / bit;
        return String.valueOf(num).charAt(n % bit) - '0';
    }
}
```

#### 45. 把数组排成最小的数
题意：[面试题45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)  
思路：自定义比较器。【对于两个数字，不能简单的将较小的数字放在前面，大的数字放在后面（如2和12，最小的数应该是122，而不是212）。】正确的做法是比较两个数字组合起来的字串，将组合后字串字典序小的放前面。
```Java
class Solution {
    public String minNumber(int[] nums) {
        Integer[] tmp = new Integer[nums.length];
        for (int i = 0; i < tmp.length; i++) {
            tmp[i] = nums[i];
        }
        Arrays.sort(tmp, (a, b) -> {
            String s1 = String.valueOf(a) + String.valueOf(b);
            String s2 = String.valueOf(b) + String.valueOf(a);
            return s1.compareTo(s2);
        });
        StringBuilder sb = new StringBuilder();
        for (int i : tmp) {
            sb.append(i);
        }
        return sb.toString();
    }
}
```
