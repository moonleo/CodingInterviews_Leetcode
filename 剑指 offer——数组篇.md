---
layout: post
title: 剑指 offer——数组篇
date: 2020-04-05
categories: 算法
tags: [算法, 剑指 offer, 数组]
excerpt_separator: <!--more-->
---

<!--more-->
#### 3. 数组中重复的数字
题意：详见[面试题03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)  
思路1：使用Hash表。遍历整个数组，在访问到某个数字时，先在Hash表中查询是否已包含此数字，如果包含，返回此数字即可；如果不包含，则将数字加到Hash表中，继续遍历下一个数字。
```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int i : set) {
            if (set.contains(i)) {
                return i;
            } else {
                set.add(i);
            }
        }
        return -1;
    }
}
```
思路2:由于数组的大小为n，数字范围为[0,n-1]。我们可以在遍历中将数字换到数组中下标对应的位置，即将0换到数组index为0的位置。如果对应的位置已经有数字了，表示当前遍历的数字是重复的，那么返回当前的数字。
```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for (int i = 0; i < nums.length; i ++) {
            if (nums[i] != i && nums[i] == nums[nums[i]]) {
                return nums[i];
            }
            while (nums[i] != i && nums[i] != nums[nums[i]]) {
                swap(nums, i, nums[i]);
            }
        }
        return -1;
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

#### 4. 二维数组中的查找
题意：详见[面试题04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)  
思路：（1）从左到右、从上到下都是递增关系  
（2）搜索过程如果是从左上向右下（或从右下向左上），那么在当前值比目标值小时，将要搜索两个方向（向右和向下的元素都比当前值要大），所以此法将可能遍历整个二维数组，时间复杂度O(n*m)  
（3）可以从右上角or左下角开始搜索，每次与target比较后，只有一个方向可以搜索，时间复杂度O(m+n)
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        int h = matrix.length;
        if (h == 0) {
            return false;
        }
        int w = matrix[0].length;
        if (w == 0) {
            return false;
        }
        int i = 0, j = w - 1;
        while (i < h && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                j --;
            } else {
                i ++;
            }
        }
        return false;
    }
}
```
#### 11. 旋转数组的最小数字
题意：[面试题11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)  
思路：使用二分的思想，每次将搜素范围缩小。
假设[left, right]为当前的搜索范围。取中间的位置mid，与right位置的数字比较：  
1）如果mid位置的数字 > right位置的数字，那么最小数字一定在搜索范围的后半段，所以需要将left置为mid+1；  
2）如果mid位置的数字 < right位置的数字，那么最小数字一定在搜索范围的前半段（包括mid位置），所以需要将right置为mid；  
3）如果mid位置的数字 = right位置的数字，此时无法判断最小数字的位置，所以只能逐步缩小搜索范围，将right--。

```java
class Solution {
    public int minArray(int[] numbers) {
        int left = 0;
        int right = numbers.length - 1;
        int mid;
        while (left < right) {
            mid = left + ((right - left) >> 1);
            if (numbers[mid] > numbers[right]) {
                left = mid + 1;
            } else if (numbers[mid] < numbers[right]){
                right = mid;
            } else {
                right --;
            }
        }
        return numbers[left];
    }
}
```
#### 17. 打印从1到最大的n位数
题意：[面试题17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)  
思路：最大的n位数加上1一定是1+n个0，所以题目给出了n，就能计算出10的n次方的值，并以此值作为打印的上边界。
```java
class Solution {
    public int[] printNumbers(int n) {
        int ten = 1;
        int i = 1;
        while (i <= n) {
            ten *= 10;
            i ++;
        }
        int[] res = new int[ten - 1];
        for (i = 0; i < res.length; i ++) {
            res[i] = i + 1;
        }
        return res;
    }
}
```

#### 21. 调整数组顺序使奇数位于偶数前面
题意：[面试题21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)  
思路：使用类似快排的思想，在数组前后分别置一个指针left、right，left从前向后寻找偶数，right从后向前寻找奇数，然后交换两个位置的数字并移动两指针，直到两指针碰到一起结束。
```Java
class Solution {
    public int[] exchange(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while (left < right) {
            while (left < right && (nums[left] & 1) == 1) {
                left ++;
            }
            while (left < right && (nums[right] & 1) == 0) {
                right --;
            }
            swap(nums, left++, right--);
        }
        return nums;
    }

    private void swap(int[] nums, int left, int right) {
        int tmp = nums[left];
        nums[left] = nums[right];
        nums[right] = tmp;
    }
}
```

#### 29. 顺时针打印矩阵
题意：[面试题29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)  
思路：双指针法。两个指针分别位于矩阵的左上角和右下角。每次打印以两个指针为边界的矩形范围（打印是要考虑特殊情况：只有一个元素、只有一行、只有一列...）。打印一圈之后缩小范围，左上角指针向右下角移动，右下角矩阵向左上角移动。
```Java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return new int[0];
        }
        int width = matrix[0].length;
        int height = matrix.length;
        int[] res = new int[width * height];
        int leftX = 0, leftY = 0;
        int rightX = height - 1, rightY = width - 1;
        int start = 0;
        while (start < res.length) {
            start = print(matrix, leftX, leftY, rightX, rightY, res, start);
            leftX ++;
            leftY ++;
            rightX --;
            rightY --;
        }
        return res;
    }

    private int print(int[][] matrix, int leftX, int leftY, int rightX, int rightY, int[] res, int start) {
        if (leftX == rightX && leftY == rightY) {
            res[start ++] = matrix[leftX][leftY];
        } else if (leftX == rightX) {
            for (int i = leftY; i <= rightY; i ++) {
                res[start ++] = matrix[leftX][i];
            }
        } else if (leftY == rightY) {
            for (int i = leftX; i <= rightX; i ++) {
                res[start ++] = matrix[i][leftY];
            }
        } else {
            int i = leftX, j = leftY;
            while (j < rightY) {
                res[start ++] = matrix[i][j ++];
            }
            while (i < rightX) {
                res[start ++] = matrix[i ++][j];
            }
            while (j > leftY) {
                res[start ++] = matrix[i][j --];
            }
            while (i > leftX) {
                res[start ++] = matrix[i --][j];
            }
        }
        return start;
    }
}
```

#### 39. 数组中出现次数超过一次的数字
题意：[面试题39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)  
思路：假设数组中超过一半的数字为x，如果同时删除一个x和另一个非x，那么最后数组中剩下的仍然是x.
遍历数组，使用一个候选值candidate和一个计数count记录当前比较多的数字，如果遍历到的数字等于candidate，那么count加一。
如果遍历到的数字不为candidate，那么count减一，当count减到0的时候，将candidate赋值为此时遍历到的数字。
最后candidate记录的数字就是出现次数超过一半的数字。
```Java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = nums[0];
        int count = 1;
        for (int i = 1; i < nums.length; i ++) {
            if (nums[i] == candidate) {
                count ++;
            } else {
                if (count == 0) {
                    candidate = nums[i];
                } else {
                    count --;
                }
            }
        }
        return candidate;
    }
}
```

#### 40. 最小的k个数
题意：[面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)  
思路：使用大根堆。大根堆中结点个数维持在k个。遍历的过程中，如果当前元素比根结点的值小，那么将当前元素插入大根堆中，并将堆顶元素弹出。遍历完之后大根堆中保存的就是最小的k个数。
Java中大根堆可以使用优先队列来实现。
```Java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        PriorityQueue<Integer> queue = new PriorityQueue<>((i1, i2) -> {
            return i2 - i1;
        });
        for (int i : arr) {
            if (queue.size() >= k) {
                if (!queue.isEmpty() && i < queue.peek()) {
                    queue.poll();
                    queue.add(i);
                }
            } else {
                queue.add(i);
            }
        }
        int[] res = new int[queue.size()];
        for (int i = 0; i < res.length; i ++) {
            res[i] = queue.poll();
        }
        return res;
    }
}
```
