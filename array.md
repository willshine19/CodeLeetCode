# array

## 1089. Duplicate Zeros

https://leetcode-cn.com/problems/duplicate-zeros/

```
Input: [1,0,2,3,0,4,5,0]
Output: null
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
```

```python
class Solution:
    def duplicateZeros(self, arr: List[int]) -> None:
        zero_count = 0
        for val in arr:
            if val == 0:
                zero_count += 1
        
        if zero_count == 0:
            return
        
        for idx in reversed(range(len(arr))):
            if arr[idx] == 0:
                zero_count -= 1
                if idx + zero_count + 1 < len(arr):
                    arr[idx + zero_count + 1] = 0
            if idx + zero_count < len(arr):
                arr[idx + zero_count] = arr[idx]
                
```

---

# sort

## 283. Move Zeroes

https://leetcode-cn.com/problems/move-zeroes/

Version 1

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        # left 第一个 0
        # right 是 left 后面第一个非0
        left = right = 0
        ln = len(nums)
        
        for left, val in enumerate(nums):
            if val != 0:
                continue
            # 找到了 left

            if left >= right:
                right = left + 1
            
            while right < ln and nums[right] == 0: 
                right += 1
            if right == ln: return
            # 找到了 right
            
            nums[left] = nums[right]
            nums[right] = 0
            right += 1
```

Version 2

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        pos = 0
        for num in nums:
            if num != 0: nums[pos] = num; pos += 1
        while pos < len(nums):
            nums[pos] = 0; pos += 1
```

## 75. Sort Colors


Version 1 O(n) 遍历两次

```java
class Solution {
    public void sortColors(int[] nums) {
        // 2015-09-15
        if (nums == null || nums.length == 0) {
            return;
        }
        
        int count0 = 0;
        int count1 = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                count0++;
            } else if (nums[i] == 1) {
                count1++;
            }
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (count0 > 0) {
                nums[i] = 0;
                count0--;
            } else if (count1 > 0) {
                nums[i] = 1;
                count1--;
            } else {
                nums[i] = 2;
            }
        }
        
        return;
    }
}
```

Version 2 遍历1次

```java
class Solution {
    public void sortColors(int[] nums) {
        // 2015-09-15 O(n)
        if(nums == null || nums.length <= 1)
            return;
        
        // pl 指向0后一个数
        int pl = 0;
        // pr 指向2前一个数
        int pr = nums.length - 1;
        int i = 0;
        while(i <= pr){
            if(nums[i] == 0){
                // 遇到0 换到前面
                swap(nums, pl, i);
                pl++;
                i++;
            }else if(nums[i] == 1){
                i++;
            }else{
                // 遇到2 换到后面
                swap(nums, pr, i);
                pr--;
            }
        }
    }
    
    private void swap(int[] a, int i, int j) {
        if (i == j) {
            return;
        }
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}
```