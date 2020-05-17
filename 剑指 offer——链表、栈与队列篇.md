---
layout: post
title: 剑指 offer——链表、栈与队列篇
date: 2020-04-05
categories: 算法
tags: [算法, 剑指 offer, 链表, 栈, 队列]
excerpt_separator: <!--more-->
---
<!--more-->
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

#### 30. 包含min函数的栈
题意：[面试题30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)  
思路：使用两个栈，一个栈data用来保存数据，另一个栈min用来存data中最小值的信息。  
1）入栈时，若当前入栈的元素x小于min栈中栈顶元素，那么将当前元素x同时压入data栈和min栈。  
2）出栈时，若出栈元素x等于min的栈顶元素，那么将x也从min栈中弹出。
```Java
class MinStack {
    Stack<Integer> data;
    Stack<Integer> min;
    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<>();
        min = new Stack<>();
    }

    public void push(int x) {
        data.push(x);
        if (min.isEmpty() || x <= min.peek()) {
            min.push(x);
        }
    }

    public void pop() {
        if (data.isEmpty()) {
            return;   
        }
        int num = data.pop();
        if (num == min.peek()) {
            min.pop();
        }
    }

    public int top() {
        if (data.isEmpty()) {
            return -1;
        }
        return data.peek();
    }

    public int min() {
        if (min.isEmpty()) {
            return -1;
        }
        return min.peek();
    }
}
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
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

#### 35. 复杂链表的复制
题意：[面试题35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)  
思路：链表除了next指针，还包含random指针。使用一个Map记录下已经创建的新结点，并将旧结点与新结点建立映射关系。在遍历过程中对于已经创建过的结点直接从Map中取即可。
```Java
class Solution {
    Map<Node, Node> map = new HashMap<>();

    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        if (map.get(head) != null) {
            return map.get(head);
        }
        Node newNode = new Node(head.val);
        map.put(head, newNode);
        newNode.next = copyRandomList(head.next);
        newNode.random = copyRandomList(head.random);
        return newNode;
    }
}
```

#### 41. 数据流中的中位数
题意：[面试题41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)  
思路：构造两个堆，一个大根堆，一个小根堆。使大根堆中记录数据流中较小部分的元素，小根堆中记录数据流中较大部分的元素。  
使得小根堆中元素的值都大于大根堆中元素的值。即使小根堆的根结点值比大根堆的根结点值要大。  
并且保证，在数据流的个数为偶数时，两个堆中的数据个数一样（此时中位数为两个堆堆顶元素的平均值）。数据流个数为奇数时，大根堆个数比小根堆多一个（此时中位数为大根堆的堆顶元素）。
```Java
class MedianFinder {
    PriorityQueue<Integer> min;
    PriorityQueue<Integer> max;

    /** initialize your data structure here. */
    public MedianFinder() {
        min = new PriorityQueue();
        max = new PriorityQueue(Collections.reverseOrder());
    }

    public void addNum(int num) {
        if (max.size() == min.size()) {
            min.add(num);
            max.add(min.poll());
        } else {
            max.add(num);
            min.add(max.poll());
        }
    }

    public double findMedian() {
        return max.size() == min.size() ? (max.peek() + min.peek()) / 2.0 : max.peek();
    }
}
```
