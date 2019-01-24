LeetCode61. 旋转链表
====

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例1

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```



<h1>思路</h1>

本题链表的旋转主要思路是针对选择的个数，计算后序需要将多少k个元素提前到链表头部，所以需要先遍历一次遍历获取总个数，然后就是链表的拼接工作

<h1>解法</h1>

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        ListNode node=head;
        int count=0;
        while(node!=null){
            count++;
            node=node.next;
        }
        if(count==0||count==1||(k=k%count)==0)
            return head;
        node=head;
        
        
      
        ListNode second=head;
        for(int i=0;i<count-k-1;i++){
            second=second.next;
        }
        head=second.next;
        
        ListNode first=second.next;
        second.next=null;
        
        while(first.next!=null){
            first=first.next;
        }
        first.next=node;
        
        return head;
    }
}
```
执行用时: 10 ms, 在Rotate List的Java提交中击败了90.96% 的用户