LeetCode129.求根到叶子节点数字之和
====
给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。

说明: 叶子节点是指没有子节点的节点。

示例1：

```
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```
<h1>思路</h1>

深度遍历



<h1>解答</h1>

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int sum=0;
    public int sumNumbers(TreeNode root) {
        if(root==null)
            return 0;
        findSum(root,0);
        return sum;
    }
    public void findSum(TreeNode root,int val){
        if(root.left==null&&root.right==null){
            sum+=val*10+root.val;
            return ;
        }
        
        if(root.left!=null){
            findSum(root.left,val*10+root.val);
        }
        if(root.right!=null){
            findSum(root.right,val*10+root.val);
        }
    }
}
```
执行用时: 1 ms, 在Sum Root to Leaf Numbers的Java提交中击败了75.07% 的用户