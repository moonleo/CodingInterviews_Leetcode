---
layout: post
title: 剑指 offer——字符串篇
date: 2020-04-05
categories: 算法
tags: [算法, 剑指 offer, 字符串]
excerpt_separator: <!--more-->
mathjax: true
---

<!--more-->
#### 05. 替换空格
题意：[面试题05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)  
思路：题目要求将空格字符‘ ’替换为‘20%’，即将一个字符替换为三个。我们可以先遍历一遍字符串，统计出字符串中空格的个数count，根据这个数字可以计算出：
\[
替换之后字串的长度 = 原字串长度 + 2 * count
\]
然后从后向前，依次将原字符串的非空格内容复制到新的字符数组中，遇到空格，则依次添加‘%’、‘0’、‘2’。
```java
class Solution {
    public String replaceSpace(String s) {
        char[] arr = s.toCharArray();
        int count = 0;
        for (int i = 0; i < arr.length; i ++) {
            if (arr[i] == ' ') {
                count ++;
            }
        }
        char[] res = new char[arr.length + count * 2];
        int index = res.length - 1;
        int i = arr.length - 1;
        while (i >= 0) {
            if (arr[i] == ' ') {
                res[index --] = '0';
                res[index --] = '2';
                res[index --] = '%';
            } else {
                res[index --] = arr[i];
            }
            i --;
        }
        return String.valueOf(res);
    }
}
```

### 20. 表示数值的字符串
题意：[面试题20. 表示数值的字符串]（https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/）  
思路：参照题解[有限状态机DFA](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/que-ding-you-xian-zi-dong-ji-dfa-by-justyou/)
![状态转移图](https://pic.leetcode-cn.com/21bb5c376b6f9d17f85a2cb5149cf663e15581a3cc4e7092d994bd8df5c0bd92-IMG_3732.jpg)
![状态转移矩阵](https://pic.leetcode-cn.com/b2f30ff15f23955ba82c297a892eab2a4fa53ba2bb9760fca83bb8b85fd3d41b-IMG_3733.jpg)
```Java
class Solution {
    int[][] transTable = {
        {1, 2, 7, -1, -1, 0,},
        {-1, 2, 7, -1, -1, -1},
        {-1, 2, 3, 4, -1, 9},
        {-1, 3, -1, 4, -1, 9},
        {6, 5, -1, -1, -1, -1},
        {-1, 5, -1, -1, -1, 9},
        {-1, 5, -1, -1, -1, -1},
        {-1, 8, -1, -1, -1, -1},
        {-1, 8, -1, 4, -1, 9},
        {-1, -1, -1, -1, -1, 9}
    };

    Map<String, Integer> indexMap = new HashMap<String, Integer>() {
        {
            put("sign", 0);
            put("number", 1);
            put(".", 2);
            put("exp", 3);
            put("other", 4);
            put("blank", 5);
        }
    };

    Set<Integer> set = new HashSet<>(Arrays.asList(2, 3, 5, 8, 9));

    public boolean isNumber(String s) {
        int state = 0;
        for (char c : s.toCharArray()) {
            state = transTable[state][nextState(c)];
            if (state == -1) {
                return false;
            }
        }
        return set.contains(state);
    }

    private int nextState(char c) {
        String name;
        if (c >= '0' && c <= '9') {
            name = "number";
        } else if (c == '+' || c == '-') {
            name = "sign";
        } else if (c == '.') {
            name = ".";
        } else if (c == 'E' || c == 'e') {
            name = "exp";
        } else if (c == ' ') {
            name = "blank";
        } else {
            name = "other";
        }
        return indexMap.get(name);
    }
}
```

#### 38. 字符串的排列
题意：[面试题38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)  
思路：递归构建。每次固定一个位置上的值，然后让其后面位置上的元素进行全排列。固定某一个位置的元素可以使用交换的方式。
```Java
class Solution {
    public String[] permutation(String s) {
        char[] arr = s.toCharArray();
        Set<String> res = new HashSet<>();
        permutation(arr, 0, res);
        return res.toArray(new String[0]);
    }

    private void permutation(char[] arr, int start, Set<String> res) {
        if (start == arr.length) {
            res.add(String.valueOf(arr));
            return;
        }
        for (int i = start; i < arr.length; i ++) {
            swap(arr, i, start);
            permutation(arr, start + 1, res);
            swap(arr, i, start);
        }
    }

    private void swap(char[] arr, int i, int j) {
        char tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```
对于有重复元素的字符串，还可以使用以下做法。某一个位置上固定的元素只需要固定一次就行。
```Java
class Solution {
    public String[] permutation(String s) {
        char[] arr = s.toCharArray();
        Set<String> res = new HashSet<>();
        permutation(arr, 0, res);
        return res.toArray(new String[0]);
    }

    private void permutation(char[] arr, int start, Set<String> res) {
        if (start == arr.length) {
            res.add(String.valueOf(arr));
            return;
        }
        for (int i = start; i < arr.length; i ++) {
            if (i > start && arr[i] == arr[start]) {
                continue;
            }
            swap(arr, i, start);
            permutation(arr, start + 1, res);
            swap(arr, i, start);
        }
    }

    private void swap(char[] arr, int i, int j) {
        char tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```
#### 48. 最长不含重复字符的子字符串
题意：[面试题48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)  
思路：滑动窗口。使用两个指针指向滑动窗口的左右边界，先移动右边界，直到出现重复字符，再移动左边界，直到消除重复字符。重复以上过程直到右边界到达字串的最后位置。
```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        char[] arr = s.toCharArray();
        int left = -1;
        int right = 0;
        int max = 0;
        while (right < arr.length) {
            if (set.contains(arr[right])) {
                while (left < right) {
                    set.remove(arr[++left]);
                    if (arr[left] == arr[right]) {
                        break;
                    }
                }
            }
            set.add(arr[right]);
            max = Math.max(right - left, max);
            right ++;
        }
        return max;
    }
}
```

#### 50. 第一个只出现一次的字符
题意：[面试题50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)  
思路：使用hash表记录字串中每个字符出现的次数。然后再遍历一遍字符，找到第一个出现次数为1的字符返回即可。
```Java
class Solution {
    public char firstUniqChar(String s) {
        if (s == null || s.length() == 0) {
            return ' ';
        }
        char[] arr = s.toCharArray();
        int[] chars = new int[26];
        for (char c : arr) {
            chars[c - 'a']++;
        }
        for (char c : arr) {
            if (chars[c - 'a'] == 1) {
                return c;
            }
        }
        return ' ';
    }
}
```
