LeetCode39. 组合总和
=====
自己写的解法，29ms
#
```
class Solution {
    private List<List<Integer>> res=new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        path(new ArrayList<Integer>(),target,0,candidates);
        return res;
    }
    
    private void path(ArrayList list,int target ,int start,int[] candidates){
        if(target==0){
            res.add(new ArrayList(list));
            return;
        }
        
        if(target<0){
            
            return;
        }
        
        for(int i=start;i<candidates.length;i++){
            list.add(candidates[i]);
            path(list,target-candidates[i],i,candidates);
            list.remove(list.size()-1);
        }
        return;
    }
}
```

排名第一9ms的解法
#
```
class Solution {
    private void search(int[] candidates, int target, int[] count, List<List<Integer>> res, int n) {
        if(target == 0) {
            List<Integer> list = new ArrayList<Integer>();
            for(int i = 0; i <= n; i++) {
                for(int j = 0; j < count[i]; j++) {
                    list.add(candidates[i]);
                }
            }
            res.add(list);
        }
        
        if(n == candidates.length || target < candidates[n])
            return;
        
        search(candidates, target, count, res, n + 1);
        count[n]++;        
        search(candidates, target - candidates[n], count, res, n);
        count[n]--;
    }
    
```
