LeetCode226.翻转二叉树
====
翻转一棵二叉树。

示例：

输入
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
输出

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

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
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return root;
        TreeNode tmp=root.left;
        root.left=root.right;
        root.right=tmp;
        
        invertTree(root.left);
        
        invertTree(root.right);
        return root;
    }
    
   
    
}
```
执行用时: 0 ms, 在Invert Binary Tree的Java提交中击败了100.00% 的用户