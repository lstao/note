LeetCode25. k个一组翻转链表
====

<h1>思路</h1>

定义一个头节点dummy，其next值为head。然后定义一个last初始为dummy，这个last每次指向的是当前要翻转的链表的头位置。先遍历出总的节点大小，然后从头节点开始，每次取k个节点开始翻转。以第一组链表为例，last为dummy，pre指针指向默认第一个已插入（每次都默认last后的第一个位置已翻转完成，后序的节点依次往last后插入，pre指针在一组链表中均指向第一个插入好的位置）的节点位置，然后将剩余的k-1个节点依次接入到last指针后。

进行完这一趟翻转后，此时pre指针指向的是已经翻转好的一组链表中，再将last赋予pre的值作为下一组待翻转链表的头节点，num减去k的大小代表反转一组结束，进行下一轮循环，直到剩余节点不小于k。


<h1>自己的解法</h1>

```
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if(k==1)
            return head;
        ListNode dummy=new ListNode(0);
        dummy.next=head;
        
        ListNode pre=dummy;
        ListNode last=dummy;
        int num=0;
        while((pre=pre.next)!=null){
            num++;
        }
        
        while(num>=k){
            ListNode cur=last.next.next;
            pre=last.next;
            for(int i=0;i<k-1;i++){      
                ListNode t=last.next;
                last.next=cur;
                pre.next=cur.next;
                cur.next=t;
                cur=pre.next;
            }
                       
            last=pre;
            num-=k;            
            
        }
        
        
        
        
        
        return dummy.next;
        
    }
}
```

执行用时: 8 ms, 在Reverse Nodes in k-Group的Java提交中击败了46.24% 的用户


<h1>执行4ms的解法</h1>

```                                                                                                                                                                                             
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
       ListNode currentNode = head;
        if (currentNode == null || k < 0){
            return head;
        }
        int count = 0;
        while (currentNode != null && count < k){ // find the k+1 node
            currentNode = currentNode.next;
            count++;
        }
        if (count == k){ // if k+1 node is found
            currentNode = reverseKGroup(currentNode, k); // reverse list with k+1 node as head
            while (count-- > 0){ // reverse current k-group:
                ListNode temp = head.next;
                head.next = currentNode;
                currentNode = head;
                head = temp;
            }
            head = currentNode;
        }
        return head;  
    }
}

```