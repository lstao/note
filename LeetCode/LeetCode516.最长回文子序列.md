LeetCode516.最长回文子序列
====
给定一个字符串s，找到其中最长的回文子序列。可以假设s的最大长度为1000。

示例 1:

输入:
```
"bbbab"
```

输出

```
4
```

1.什么是回文串
---------
正读和反读都一样的字符串


2.回文子序列和回文子串的区别
------
字符字串指的是字符串中连续的n个字符；如palindrome中，pa，alind，drome等都属于它的字串

而字符子序列指的是字符串中不一定连续但先后顺序一致的n个字符；如palindrome中，plind，lime属于它的子序列，而mod，rope则不是，因为它们与字符串的字符顺序不一致。

3.动态规划求解
-----
```
对于任意字符串，
如果头尾字符相同，那么字符串的最长子序列等于去掉首尾的字符串的最长子序列加上首尾；
如果首尾字符不同，则最长子序列等于去掉头的字符串的最长子序列和去掉尾的字符串的最长子序列的较大者。
因此动态规划的状态转移方程为：
设字符串为str，长度为n，p[i][j]表示第i到第j个字符间的子序列的个数（i<=j），则：
状态初始条件：dp[i][i]=1 （i=0：n-1）
状态转移方程：dp[i][j]=dp[i+1][j-1] + 2            if（str[i]==str[j]）
            dp[i][j]=max(dp[i+1][j],dp[i][j-1])  if （str[i]!=str[j]）
以下代码中的两层循环变量i,j的顺序可以改变，但必须满足i<=j的条件。j指针不能跑到i指针的前面。
计算dp[i][j]时需要计算dp[i+1][*]或dp[*][j-1]，因此i应该从大到小，即递减；j应该从小到大，即递增。
```

4.代码
----
```
class Solution {
    public int longestPalindromeSubseq(String s) {
        int[][] dp=new int[s.length()][s.length()];
        
        for(int i=s.length()-1;i>=0;i--){
            dp[i][i]=1;
            for(int j=i+1;j<s.length();j++){
                if(s.charAt(i)==s.charAt(j)){
                    dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=Math.max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        
        return dp[0][s.length()-1];
    }
}
```

执行用时: 63 ms, 在Longest Palindromic Subsequence的Java提交中击败了50.21% 的用户



