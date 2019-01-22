LeetCode24. 两两交换链表中的节点
====

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

示例
```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

----
解法：定义一个pre指针，初始执行dummy空节点，存储head起点位置，以pre.next!=null与pre.next.next!=null 为起点进行两两交换。最后返回dummy的next即可

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        while(pre.next!=null&&pre.next.next!=null){
            ListNode start = pre.next;
            ListNode then = start.next;
            start.next = then.next;
            then.next = pre.next;
            pre.next = then;
            pre = start;
        }
        return dummy.next;
        

        
    }
}

```