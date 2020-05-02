## 剑指 offer——链表、栈与队列篇
#### 6. 从尾到头打印链表
题意：[面试题06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)  
思路：首先遍历一遍链表得到链表的长度，以此长度初始化数组。然后再从头到尾遍历一遍链表，并将遍历得到的数字从后往前插入数组。
```java
class Solution {
    public int[] reversePrint(ListNode head) {
        int len = 0;
        ListNode p = head;
        while (p != null) {
            p = p.next;
            len ++;
        }
        int[] res = new int[len];
        p = head;
        while (p != null) {
            res[--len] = p.val;
            p = p.next;
        }
        return res;
    }
}
```

#### 9. 用两个栈实现队列
题意：[面试题09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)  
思路：“出队”操作，将一个栈的数据全部倒入到另一个空栈中，之后另一个栈的操作顺序即为队列的出栈顺序。
```java
class CQueue {
    Stack<Integer> in;
    Stack<Integer> out;

    public CQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }

    public void appendTail(int value) {
        in.push(value);
    }

    public int deleteHead() {
        if (out.isEmpty()) {
            while (!in.isEmpty()) {
                out.push(in.pop());
            }
        }
        return out.isEmpty() ? -1 : out.pop();
    }
}
```

#### 18. 删除链表的节点
题意：[面试题18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)  
思路1：要删除单链表中的某一个节点node，首先需要找到node的前一个节点pre，然后把pre的next指针指向node的下一个节点即可。
```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        if (head.val == val) {
            return head.next;
        }
        ListNode p = head;
        while (p.next != null) {
            if (p.next.val == val) {
                p.next = p.next.next;
                break;
            }
            p = p.next;
        }
        return head;
    }
}
```
思路2：上述思路1是在<b>不能修改</b>链表节点值的情况下的操作。如果可以修改链表的值，或者题目中没有给出链表头节点，只给出了要被删除的节点。  
这时我们可以使用后面的节点值覆盖前面节点的值来完成删除节点操作。参照[面试题 02.03. 删除中间节点](https://leetcode-cn.com/problems/delete-middle-node-lcci/)
```java
class Solution {
    public void deleteNode(ListNode node) {
        ListNode p = node;
        ListNode q = node.next;
        while (q.next != null) {
            p.val = q.val;
            p = p.next;
            q = q.next;
        }
        p.val = q.val;
        p.next = null;
    }
}
```

#### 22. 链表中倒数第k个节点
题意：[面试题22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)  
思路：快慢双指针法。让快指针先走k步，然后快慢指针一起走，当快指针走到链表结尾的时候，慢指针就指向倒数第k个节点
```Java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode p = head;
        while (p != null && k > 0) {
            p = p.next;
            k --;
        }
        if (p == null && k > 0) {
            return null;
        }
        ListNode q = head;
        while (p != null) {
             p = p.next;
             q = q.next;
        }
        return q;
    }
}
```

#### 24. 反转链表
题意：[面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)  
思路：递归。先反转当前节点的后面节点，reverseList(head.next)，这个函数返回的是反转之后的链表头，即最后一个节点。进行这步操作之后，当前节点下一个节点指向的是反转之后链表的尾部节点，即head.next，这时将head.next的下一个节点指向当前节点即可完成反转。
```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        if (head.next == null) {
            return head;
        }
        ListNode next = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return next;
    }
}
```

#### 25. 合并两个排序的链表
题意：[面试题25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)  
思路：双指针法。使用两个指针分别指向l1和l2的头结点，如果l1.val < l2.val，那么将l1指向的结点加入新的链表中，否则将l2指向的结点加入新的链表。
```Java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode r = head;
        while (l1 != null && l2 != null) {
            if (l1.val > l2.val) {
                r.next = l2;
                l2 = l2.next;
            } else {
                r.next = l1;
                l1 = l1.next;
            }
            r = r.next;
        }
        r.next = (l1 == null) ? l2 : l1;
        return head.next;
    }
}
```

#### 31. 栈的压入、弹出序列
题意：[面试题31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)  
思路：建一个栈来模拟题目中的压入、弹出操作。由于弹出序列中的第一个数字，一定是出现在栈顶时弹出的，如
```
pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
```
中，弹出序列中的第一个元素4出栈时，压栈序列中一定是将4以及之前的元素压入栈内了。这时模拟弹出栈顶的元素4，然后接着比较弹出序列的下一个元素是否还与栈顶相同。  
即每次将弹出序列的元素，与栈顶元素比较，相同则弹出，不同则继续入栈元素，最后判断栈是否为空即可判断是否为合法的弹出序列。
```Java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int pushIndex = 0;
        int popIndex = 0;
        while (pushIndex < pushed.length) {
            stack.push(pushed[pushIndex]);
            while (popIndex < popped.length
                && !stack.isEmpty()
                && popped[popIndex] == stack.peek()) {
                stack.pop();
                popIndex ++;
            }
            pushIndex ++;
        }
        return stack.isEmpty();
    }
}
```
