# leetcode

## 全排列 Permutations

[[LintCode]Permutations](https://www.lintcode.com/problem/permutations/description)

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        // 2015-08-28
        // nums不含重复元素，每个元素只可以使用一次
        // 解集中不可以含重复解
        ArrayList<ArrayList<Integer>> rst = new ArrayList<>();
        if (nums == null || nums.size() == 0) {
            return rst;
        }
        Collections.sort(nums);
        ArrayList<Integer> list = new ArrayList<>();
        helper(nums, rst, list);
        return rst;
    }
    
    private void helper(ArrayList<Integer> nums, 
            ArrayList<ArrayList<Integer> rst, ArrayList<Integer> list) {
        if (list.size() == nums.size()) {
            rst.add(new ArrayList<Integer>(list));
            return;
        }    
        
        for (int i = 0; i < nums.size(); i++) {
            if (list.contains(nums.get(i))) {
                continue;
            }
            list.add(nums.get(i));
            helper(nums, rst, list);
            list.remove(list.size() - 1);
        }
        return;
    }
}
```
