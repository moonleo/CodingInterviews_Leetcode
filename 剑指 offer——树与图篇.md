## 剑指 offer——树与图篇
### 树
#### 7. 重建二叉树
题意：[面试题07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)  
思路：前序遍历的顺序是“根-左-右”，中序遍历的顺序是“左-中-右”。  
那么，对于整棵树前序遍历的结果，第一个值r一定是树的根结点。如果在中序遍历的结果中找到r的位置index，那么index左边的子数组就都是根结点r的左子树的中序遍历结果，index右边的结点都是根结点右子树的中序遍历结果。  
并且，根据在中序遍历中找到的左子树长度，可以同样在前序遍历的r结点之后找到相同长度的子数组，即为左子树的前序遍历结果。其余的部分即是左子树的前序遍历结果。  
然后根据左、右子树的前序遍历和中序遍历结果再按照上述规则创建二叉树。
```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode build(int[] pre, int preL, int preR, int[] in, int inL, int inR) {
        if (preL > preR || inL > inR) {
            return null;
        }
        TreeNode root = new TreeNode(pre[preL]);
        int rootIndex = -1;
        for (int i = inL; i <= inR; i ++) {
            if (in[i] == pre[preL]) {
                rootIndex = i;
                break;
            }
        }
        int leftLen = rootIndex - inL;
        root.left = build(pre, preL+1, preL+leftLen, in, inL, rootIndex-1);
        root.right = build(pre, preL+leftLen+1, preR, in, rootIndex+1, inR);
        return root;
    }
}
```

### DFS/BFS
#### 12. 矩阵中的路径
题意：[面试题12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)  
思路：典型的dfs搜索+回溯题，每次朝一个方向进行搜索到底，不满足条件时进行回溯。
```java
class Solution {
    int[][] dir = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

    public boolean exist(char[][] board, String word) {
        char[] arr = word.toCharArray();
        boolean[][] visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i ++) {
            for (int j = 0; j < board[0].length; j ++) {
                if (searchWord(board, i, j, arr, 0, visited)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean searchWord(char[][] board, int i, int j, char[] word, int w, boolean[][] visited) {
        if (w == word.length) {
            return true;
        }
        if (i >= 0 && i < board.length && j >= 0 && j < board[0].length
            && board[i][j] == word[w]
            && !visited[i][j]) {
            visited[i][j] = true;
            for (int[] d : dir) {
                if (searchWord(board, i + d[0], j + d[1], word, w + 1, visited)) {
                    return true;
                }
            }
            visited[i][j] = false;
            return false;
        }
        return false;
    }
}
```

#### 13. 机器人的运动范围
题意：[面试题13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)  
思路：搜索，在边界条件中加入数位之和不大于k即可。其余条件与搜索一致。
```java
class Solution {
    public int movingCount(int m, int n, int k) {
        int[][] visited = new int[m][n];
        return bfs(visited, m, n, 0, 0, k);
    }

    int[][] dirs = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

    private int bfs(int[][] visited, int m, int n, int i, int j, int k) {
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{i, j});
        visited[i][j] = 1;
        int count = 0;
        int[] tmp;
        while (!queue.isEmpty()) {
            tmp = queue.poll();
            count ++;
            for (int[] dir : dirs) {
                int x = tmp[0] + dir[0];
                int y = tmp[1] + dir[1];
                if (isValid(visited, m, n, x, y, k)) {
                    queue.add(new int[]{x, y});
                    visited[x][y] = 1;
                }
            }
        }
        return count;
    }

    private boolean isValid(int[][] visited, int m, int n, int x, int y, int k) {
        boolean isInBound = (x >= 0 && x < m && y >= 0 && y < n && visited[x][y] == 0);
        if (!isInBound) {
            return false;
        }
        int sum = 0;
        char[] arr = String.valueOf(x).toCharArray();
        for (char c : arr) {
            sum += (c - '0');
        }
        arr = String.valueOf(y).toCharArray();
        for (char c : arr) {
            sum += (c - '0');
        }
        return sum <= k;
    }
}
```

#### 26. 树的子结构
题意：[面试题26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)  
思路：将A中的每一个子结点a作为根结点，判断B是否与a的结构相同。isSub判断a与B的结构是否相同，isSubStructure取A中每一个结点与B进行比较。
```Java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        return A != null && B != null &&
        (isSub(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
    }

    private boolean isSub(TreeNode A, TreeNode B) {
        if (B == null) {
            return true;
        } else if (A == null || A.val != B.val) {
            return false;
        }
        return isSub(A.left, B.left) && isSub(A.right, B.right);
    }
}
```

#### 27. 二叉树的镜像
题意：[面试题27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)  
思路：以递归的方式创建。即把原树的左孩子结点作为新树的右孩子结点，原树的右孩子结点作为新树的左孩子结点。
```Java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        return createMirror(root);
    }

    private TreeNode createMirror(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode newRoot = new TreeNode(root.val);
        newRoot.left = createMirror(root.right);
        newRoot.right = createMirror(root.left);
        return newRoot;
    }
}
```

#### 28. 对称的二叉树
题意：[面试题28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)  
思路：递归方式判断。对于每一个结点，必须满足 左孩子的左结点 == 右孩子的右结点 && 左孩子的右结点 == 右孩子的左结点，那么这棵二叉树就是对称的。
```Java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isSame(root.left, root.right);
    }

    private boolean isSame(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null || right == null) {
            return false;
        } else {
            boolean flag = (left.val == right.val);
            return flag && isSame(left.left, right.right) && isSame(left.right, right.left);
        }
    }
}
```
