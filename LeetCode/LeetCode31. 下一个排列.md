LeetCode31.下一个排列
====

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```


<h1>解答</h1>

```
class Solution {
       public static void nextPermutation(int[] nums) {
        if(nums==null||nums.length<=1) return;
        int i=nums.length-2;
        while(i>=0&&nums[i]>=nums[i+1]) i--;
       if(i<0){
            reverse(nums,0,nums.length-1);
           return;
       }
        int k=nums[i];
        int j=nums.length-1;
        while(nums[j]<=k) j--;
        swap(nums,i,j);
           
        reverse(nums,i+1,nums.length-1);
        
        return;
    }
    public static void reverse(int[] arr,int i,int j){
        while(i<j){
            swap(arr,i,j);
            i++;j--;
        }
        
    }
    public static void swap(int[] arr,int a1,int a2){
        arr[a1]=arr[a2]+arr[a1];
        arr[a2]=arr[a1]-arr[a2];
        arr[a1]=arr[a1]-arr[a2];
    }
}
```


执行用时: 21 ms, 在Next Permutation的Java提交中击败了54.41% 的用户