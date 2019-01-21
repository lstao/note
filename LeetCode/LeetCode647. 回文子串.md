LeetCode647. 回文子串
====

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。

示例1
```
输入: "abc"
输出: 3
解释: 三个回文子串: "a", "b", "c".
```


思路
----
回文串可能是奇数，也可能是偶数，故对于每一个i位置，分为两种情况，从i为中心向外扩散，以及以i和i+1为中心向外扩散，每找到一个回文串就对成员变量count加1

代码
----
```
class Solution {
    private int count;
    public int countSubstrings(String s) {
        
        for(int i=0;i<s.length();i++){
            extendPalindrome(s,i,i);
            extendPalindrome(s,i,i+1);
        }
        return count;
    }
    
    private void extendPalindrome(String s,int left,int right){
        while(left>-1&&right<s.length()&&s.charAt(left)==s.charAt(right)){
            count++;left--;right++;
        }
    }
}
```




结果
------
执行用时: 7 ms, 在Palindromic Substrings的Java提交中击败了99.22% 的用户
