LeetCode655. 输出二叉树
====
在一个 m*n 的二维字符串数组中输出二叉树，并遵守以下规则：

1.行数 m 应当等于给定二叉树的高度。<br>
2.列数 n 应当总是奇数。<br>
3.根节点的值（以字符串格式给出）应当放在可放置的第一行正中间。根节点所在的行与列会将剩余空间划分为两部分  <br>&nbsp;&nbsp;（左下部分和右下部分）。你应该将左子树输出在左下部分，右子树输出在右下部分。左下和右下部分应当有相同的大小。<br>&nbsp;&nbsp;即使一个子树为空而另一个非空，你不需要为空的子树输出任何东西，但仍需要为另一个子树留出足够的空间。<br>&nbsp;&nbsp;然而，如果两个子树都为空则不需要为它们留出任何空间。<br>
4.每个未使用的空间应包含一个空的字符串""。<br>
5.使用相同的规则输出子树。<br>

示例 1:

```
输入:
     1
    /
   2
输出:
[["", "1", ""],
 ["2", "", ""]]

```

示例2：

```
输入:
     1
    / \
   2   3
    \
     4
输出:
[["", "", "", "1", "", "", ""],
 ["", "2", "", "", "", "3", ""],
 ["", "", "4", "", "", "", ""]]
```


<h1>思路</h1>
采用层次遍历，但是需要解决空间相对位置问题。将整棵树看成一个二维数组，共h层，每层有2^h-1个节点。每个节点对应的下一层输出的位置与当前遍历的层数和树的高度相关，可以得出一个距离s。算上当前的相对位置i，左右子节点分别对应的下一层位置为i-s与i+s。处理完这一层循环后，进行下一轮的递归循环即可。本题自己写的算法，消耗了太多的空间，效率不是很高。

<hr>

<h1>解答</h1>


<h2>自己的答案</h2>

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
    private ArrayList<List<String>> list=new ArrayList<>();
    private int count;
    public List<List<String>> printTree(TreeNode root) {
        int height=height(root);
        count=(1<<height)-1;
        LinkedList<TreeNode> queue=new LinkedList<TreeNode>();
        for(int i=0;i<count/2;i++){
            queue.offer(null);
        }
        queue.offer(root);
        for(int i=count/2+1;i<count;i++){
            queue.offer(null);
        }
        
        if(height!=1)
            printTree(queue,1<<(height-2));
        else
            printTree(queue,0);
        
        
        
        return list;
        
    }
    private void printTree(LinkedList<TreeNode> queue,int s){
        
        ArrayList<String> tmp=new ArrayList<>(count);
        LinkedList<TreeNode> queue2=new LinkedList<TreeNode>();
        for(int i=0;i<count;i++){
            queue2.offer(null);
        }
        for(int i=0;i<count;i++){
            TreeNode node=queue.poll();
            if(node==null){
                tmp.add("");
            }else{
                
                tmp.add(""+node.val);
                
               
                if((i-s)>-1)
                {
                    queue2.set(i-s,node.left);
                   
                }
                if((i+s)<count)
                {
                    queue2.set(i+s,node.right);
                    
                }
                
            }
            
            
        }
        list.add(tmp);
        if(s==0) return;
        printTree(queue2,s/2);
    }
    
    
    private int height(TreeNode root){
        if(root==null) return 0;
        
        return Math.max(height(root.left),height(root.right))+1;
        
    }
}
```

执行用时: 9 ms, 在Print Binary Tree的Java提交中击败了32.56% 的用户

<h2>优秀解答</h2>

执行4ms的解答

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
    public List<List<String>> printTree(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        int row = getH(root); //先获得树高再说
        double guodu = row;
        int col = (int)Math.pow(2.0, guodu) - 1;; //计算行数
        List<List<String>> ret = new ArrayList<>();
        //初始化这个二维数组都为""
        for(int i = 0; i<row;i++){
            ArrayList<String> temp = new ArrayList<String>();
            for(int j =0;j<col;j++){
                temp.add("");
            }
            ret.add(temp);
        }
        getSingleRow(ret,root,0,0,col);
        return ret;
       
    }
    
    //递归计算每一层 left:最左边  right:最右边
    public void getSingleRow(List<List<String>> ret,TreeNode root,int row,int left, int right){
        if(root!=null){
            //需要填写字母的位置
            int mid = (left + right)/2;
            //获取第row行第mid列修改其值
            ret.get(row).set(mid,String.valueOf(root.val));
            //row+1是下一行 root.left为左侧
            getSingleRow(ret, root.left, row + 1,left, mid);
            //row+1是下一行 root.right为右侧
            getSingleRow(ret, root.right, row + 1 , mid + 1, right);
        }
    }
           
    //获得树高
    public int getH(TreeNode root){
        if(root == null){
            return 0;
        }else{
            return 1 + Math.max(getH(root.left),getH(root.right));
        }
    }
}

```