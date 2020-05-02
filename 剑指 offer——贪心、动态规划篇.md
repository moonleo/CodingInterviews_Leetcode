## 剑指 offer——贪心、动态规划篇
#### 10-I. 斐波拉契数列
题意：[面试题10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)  
思路：最基础的动态规划题。数据量比较大的时候不能使用递归，会报StackOverFlow Exception，最优的方式是迭代计算。
```java
class Solution {
    public int fib(int n) {
        if (n <= 1) {
            return n;
        }
        int a = 0;
        int b = 1;
        int index = 2;
        int tmp;
        while (index <= n) {
            tmp = (a + b) % 1000000007;
            a = b;
            b = tmp;
            index ++;
        }
        return b;
    }
}
```

#### 10-II. 青蛙跳台阶
题意：[面试题10- II. 青蛙跳台阶问题
](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)  
思路：同斐波拉契数列问题，最基础的动态规划问题。
```java
class Solution {
    public int numWays(int n) {
        if (n == 0 || n == 1) {
            return 1;
        }
        int a = 1;
        int b = 1;
        int c = 0;
        int i = 2;
        while (i <= n) {
            c = (a + b) % 1000000007;
            a = b;
            b = c;
            i++;
        }
        return c;
    }
}
```
#### 14-I. 剪绳子
题意：[面试题14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)  
思路1：dp[i]表示长度为i的绳子，能够得到的最大乘积数。dp[i+1]的计算方式为，每次可以剪掉1～i的长度j，其余的绳子可以剪（即dp[i+1-j]），也可以不剪（即i+1-j）,即  
dp[i+1] = Math.max(dp[i+1-j] * j, (i+1-j) * j), 其中j $\in$ [1, i]
```java
class Solution {
    public int cuttingRope(int n) {
        if (n == 2) {
            return 1;
        }
        int[] dp = new int[n + 1];
        dp[2] = 1;
        for (int i = 3; i < n + 1; i ++) {
            for (int j = 1; j < i;j ++) {
                dp[i] = Math.max(dp[i-j] * j, (i-j)*j);
            }
        }
        return dp[n];
    }
}
```
思路2：将绳子长度先尽可能的划分为3的倍数，如果最后剩余1，则拿出一个已经分配的3来凑成4。  
例如：5 = 3 + 2    
6 = 3 + 3 -> 3 * 3 = 9  
7 = 3 + 3 + 1 = 3 + 4 -> 3 * 4 = 12  
……
```java
class Solution {
    public int cuttingRope(int n) {
        if (n <= 3) {
            return n - 1;
        }
        int remind = n;
        int multi = 1;
        while (remind > 4) {
            multi = multi * 3;
            remind -= 3;
        }
        return multi * remind;
    }
}
```

### 14-II. 剪绳子 II
题意：[试题14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)  
思路：同剪绳子I
```java
class Solution {
    public int cuttingRope(int n) {
        if (n <= 3) {
            return n - 1;
        }
        int remind = n;
        long multi = 1;
        while (remind > 4) {
            multi = (multi * 3) % 1000000007;
            remind -= 3;
        }
        return (int)((multi * remind) % 1000000007);
    }
}
```

### 19. 正则表达式匹配
题意：[面试题19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)  
思路：用dp[i][j]表示s字符串前i个字符串是否与p的前j个字符串匹配，那么所要求的结果就是dp[s.length()][p.length()]的值。  
递推式：对于某一个位置dp[i][j]，是否匹配可以分以下两种情况：  
(1)当s[i] == p[j]时，如果s[0...i-1]与p[0...j-1]匹配，那么s[0...i]与p[0...j]匹配，反之不匹配。即dp[i][j] = dp[i-1][j-1]；  
(2)当s[i] != p[j]时。如果p[j] != ‘\*’,那么s[0...i]与p[0...j]一定不匹配。如果p[j] == '\*',代表p[j-1]位置的字符可以出现0次或多次  
(a)当p[j-1]位置的字符不出现时，判断s[0...i]与p[0...j-2]是否匹配即可，即dp[i][j] = dp[i][j-2]  
(b)当p[j-1]位置的字符出现时，需要判断s[i]位置上的字符是否与p[j-1] (注意可以是'.')相同，相同则dp[i][j] = dp[i-1][j]
```Java
class Solution {
    public boolean isMatch(String s, String p) {
        int sLen = s.length();
        int pLen = p.length();
        char[] sArr = s.toCharArray();
        char[] pArr = p.toCharArray();
        boolean[][] dp = new boolean[sLen + 1][pLen + 1];
        dp[0][0] = true;
        for (int i = 0; i < sLen + 1; i ++) {
            for (int j = 1; j < pLen + 1; j ++) {
                if (i > 0 && (sArr[i - 1] == pArr[j - 1] || pArr[j - 1] == '.')) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (pArr[j - 1] == '*'){
                    if (i > 0 && j > 1
                    && (sArr[i - 1] == pArr[j - 2] || pArr[j - 2] == '.')) {
                        dp[i][j] |= dp[i - 1][j];
                    }
                    if (j > 1){
                        dp[i][j] |= dp[i][j - 2];
                    }
                }
            }
        }
        return dp[sLen][pLen];
    }
}
```
