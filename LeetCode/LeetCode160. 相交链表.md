LeetCode160. 相交链表
====
编写一个程序，找到两个单链表相交的起始节点。

<h1>思路</h1>
先统计两个链表的总长度，减去多余的部分，在剩余的部分开始对比（若有相交节点，则其中一段长度一定相同，多余的部分一定不会是相交的节点所在处）。因为是公用了一个节点，直接比较剩余长度相同部分的每个节点的地址是否相同即可，如果相同则返回该节点。不相同则相应指针均向后移动一位。

<h1>解法</h1>

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode node1=headA;
        ListNode node2=headB;
        int c1=0;
        int c2=0;
        while(node1!=null){
            c1++;
            node1=node1.next;
        }
        while(node2!=null){
            c2++;
            node2=node2.next;
        }
        
        
        node1=headA;
        node2=headB;
        
        if(c1>c2){
            for(int i=0;i<c1-c2;i++){
                node1=node1.next;
            }
        }else if(c1<c2){
            for(int i=0;i<c2-c1;i++){
                node2=node2.next;
            }
        }
        
        while(node1!=node2){
            node1=node1.next;
            node2=node2.next;
        }
        
        return node1;
    }
}
```

执行用时: 2 ms, 在Intersection of Two Linked Lists的Java提交中击败了78.54% 的用户