LeetCode3. 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例1：
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例2：

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

```
示例3：
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

思路
====

使用LinkedList链表以及一个保存最大长度的临时变量max，遍历字符s，直到遇到重复的字符之前，不断的向链表中加入当前字符。若遇到了重复的字符，则将最大长度max更新为当前链表的长度。




解答
====
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s==null||"".equals(s)) return 0;
        List<Character> list=new LinkedList<Character>();
        int max=-1;
        for(int i=0;i<s.length();i++){
            char ch=s.charAt(i);
            if(list.contains(ch)){
                max=Math.max(max,list.size());
                Iterator it=list.iterator();
                while(it.hasNext()){
                   Character c=(Character)it.next();
                    if(c!=ch)
                        it.remove();
                    else if(c==ch){
                         it.remove();
                        break;
                    }
                 }
               
            }
            
             list.add(ch);
        }
            
        return Math.max(max,list.size());
    }
}
```
执行用时: 35 ms, 在Longest Substring Without Repeating Characters的Java提交中击败了89.48% 的用户