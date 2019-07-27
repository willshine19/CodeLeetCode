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

## Subarray Sum

https://www.lintcode.com/problem/subarray-sum/description

Given an integer array, find a subarray where the sum of numbers is zero.

Version 1 

```java
public class Solution {
    public ArrayList<Integer> subarraySum(int[] nums) {
        // 2015-09-06 暴力 O(n^2)
        ArrayList<Integer> rst = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum == 0) {
                    rst.add(i);
                    rst.add(j);
                    return rst;
                }
            }
        }
        return rst;
    }
}
```

Verison 2 用哈希表降低时间复杂度

```java
public class Solution {
    public ArrayList<Integer> subarraySum(int[] nums) {
        // write your code here
        int len = nums.length;
       
        ArrayList<Integer> ans = new ArrayList<Integer>();
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
       
        map.put(0, -1);
       
        int sum = 0;
        for (int i = 0; i < len; i++) {
            sum += nums[i];
           
            if (map.containsKey(sum)) {
                ans.add(map.get(sum) + 1);
                ans.add(i);
                return ans;
            }
            
            map.put(sum, i);
        }
       
        return ans;
    }
}
```

## Subarray Sum Closest

Medium https://www.lintcode.com/problem/subarray-sum-closest/description

Given an integer array, find a subarray with sum closest to zero.

https://blog.csdn.net/willshine19/article/details/48494129

## Fast Power

Medium https://www.lintcode.com/problem/fast-power/description

Calculate the `a ^ n % b` where `a`, `b` and `n` are all 32bit non-negative integers.

https://blog.csdn.net/willshine19/article/details/48493253

## Sort Letters by Case

Medium https://www.lintcode.com/problem/sort-letters-by-case/

Given a string which contains only letters. Sort it by lower case first and upper case second.

https://blog.csdn.net/willshine19/article/details/48622531

## Majority Number III

Medium https://www.lintcode.com/problem/majority-number-iii/description

Given an array of integers and a number k, the majority number is the number that occurs more than 1/k of the size of the array.

https://blog.csdn.net/willshine19/article/details/48649743


## 136. Single Number

https://leetcode-cn.com/problems/single-number/

```java
public class Solution {
	public int singleNumber(int[] A) {
		// 2015-09-06 异或
		if (A.length == 0) {
			return 0;
		}
 
		int num = A[0];
		for(int i = 1; i < A.length; i++) {
			num = num ^ A[i];
		}
 
		return num;
	}
}
```

## 137. Single Number II

medium https://leetcode-cn.com/problems/single-number-ii/

Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

Version 1 模拟三进制 时间 O(n) 空间 O(1)

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        b1, b2 = 0, 0 # 出现一次的位，和两次的位
        for n in nums:
            b1 = (b1 ^ n) & ~ b2 # 既不在出现一次的b1，也不在出现两次的b2里面，我们就记录下来，出现了一次，再次出现则会抵消
            b2 = (b2 ^ n) & ~ b1 # 既不在出现两次的b2里面，也不再出现一次的b1里面(不止一次了)，记录出现两次，第三次则会抵消
        return b1
```

Version 2 求和 时间 O(n)

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        set_nums = set(nums)
        return (3 * sum(set_nums) - sum(nums)) // 2
```



Version 3 用 map, 时间 O(n)

```java
public class Solution {
	/**
	 * @param A : An integer array
	 * @return : An integer 
	 */
    public int singleNumberII(int[] A) {
        // 2015-09-17 O(n)
        if (A == null || A.length == 0) {
            return 0;
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < A.length; i++) {
            if (!map.containsKey(A[i])) {
                map.put(A[i], 1);
            } else if (map.get(A[i]) == 2) {
                map.remove(A[i]);
            } else {
                map.put(A[i], map.get(A[i]) + 1);
            }
        }
        Set<Integer> keyset = map.keySet();
        for (Integer rst : keyset) {
            return rst;
        }
        return 0;
    }
}
```

## 260. Single Number III

medium https://leetcode-cn.com/problems/single-number-iii/

Given an array of numbers `nums`, in which exactly two elements appear only once and all the other elements appear exactly **twice**. Find the two elements that appear only once.

Version 1 时间复杂度O(N) 空间复杂度O(1)


## 169. Majority Element

Easy https://leetcode-cn.com/problems/majority-element/

Versoion 1 时间 O(n) 空间 O(1)

```java
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: find a  majority number
     */
    public int majorityNumber(ArrayList<Integer> nums) {
        // 2015-09-07 不同的数相互抵消，剩下majority number
        int count = 0;
        int candidate = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (count == 0) {
                candidate = nums.get(i);
                count++;
                continue;
            }
            if (nums.get(i) == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
}
```

## 229. Majority Element II

Medium https://leetcode-cn.com/problems/majority-element-ii/

```java
public class Solution {
    public int majorityNumber(ArrayList<Integer> nums) {
        // 2015-09-07
        if (nums == null || nums.size() == 0) {
            return -1;
        }
        int candidate1 = 0;
        int candidate2 = 0;
        int count1 = 0;
        int count2 = 0;
        for (int i = 0; i < nums.size(); i++) {
            // 注意 if 的顺序，确保 candidate1 和 candidate2 不是同一个数
            if (count1 == 0) {
                candidate1 = nums.get(i);
                count1++;
            } else if (candidate1 == nums.get(i)) {
                count1++;
            } else if (count2 == 0) {
                candidate2 = nums.get(i);
                count2++;
            } else if (candidate2 == nums.get(i)) {
                count2++;
            } else {
                count1--;
                count2--;
            }
        }
        
        // 此时只剩下两个数，candidate1和candidate2
        count1 = 0;
        count2 = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (candidate1 == nums.get(i)) {
                count1++;
            }
            if (candidate2 == nums.get(i)) {
                count2++;
            }
        }
        return count1 > count2 ? candidate1 : candidate2;
    }
}
```

## 53. Maximum Subarray

Easy https://leetcode-cn.com/problems/maximum-subarray/

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Version 1 时间 O(n^2)

Version 2 时间 O(n) 空间 O(1)

```java
public class Solution {
    public int maxSubArray(ArrayList<Integer> nums) {
        // 2015-09-09 O(n)
        if (nums == null || nums.size() == 0) {
            // 异常
        }
        int sum = 0;
        int minSum = 0;
        int rst = Integer.MIN_VALUE;
        
        for (int i = 0; i < nums.size(); i++) {
            sum += nums.get(i);
            rst = Math.max(rst, sum - minSum);
            minSum = Math.min(minSum, sum);
        }
        
        return rst;
    }
}
```

## 172. Factorial Trailing Zeroes

https://leetcode-cn.com/problems/factorial-trailing-zeroes/

Given an integer `n`, return the number of trailing zeroes in `n!`.

```java
class Solution {
    public long trailingZeros(long n) {
        // 2015-09-09
        // n的阶乘的因子中有几个5
        // 因为2肯定比5多，所以不用考虑2
        long rst = 0;
        while ((n / 5) !=0) {
            n /= 5;
            rst += n;
        }
        return rst;
    }
};
```

## 121. Best Time to Buy and Sell Stock

Easy https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/

only one transaction

```java
public class Solution {
    public int maxProfit(int[] prices) {
        // 2015-09-15
        if (prices == null || prices.length == 0) {
            return 0;
        }
        
        int minPrice = Integer.MAX_VALUE;
        int rst = 0;
        for (int i = 0; i < prices.length; i++) {
            minPrice = Math.min(minPrice, prices[i]);
            rst = Math.max(rst, prices[i] - minPrice);
        }
        return rst;
    }
}
```

## 122. Best Time to Buy and Sell Stock II

Easy https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/

as many transactions as you like

```java
class Solution {
    public int maxProfit(int[] prices) {
        // 2015-09-15
        if (prices == null || prices.length == 0) {
            return 0;
        }
        
        int rst = 0;
        for (int i = 1; i < prices.length; i++) {
            int sub = prices[i] - prices[i - 1];
            if (sub > 0) {
                rst += sub;
            }
        }
        return rst;
    }
};
```

## 123. Best Time to Buy and Sell Stock III

Hard https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/

at most two transactions

```java
class Solution {
    public int maxProfit(int[] prices) {
        // 2015-09-15 dp
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        
        // dp from left
        int[] left = new int[prices.length];
        int minPrice = Integer.MAX_VALUE;
        left[0] = 0;
        minPrice = prices[0];
        for (int i = 1; i < prices.length; i++) {
            minPrice = Math.min(minPrice, prices[i]);
            left[i] = Math.max(left[i - 1], prices[i] - minPrice);
        }
        
        // dp from right 
        int[] right = new int[prices.length];
        int maxPrice = Integer.MIN_VALUE;
        right[prices.length - 1] = 0;
        maxPrice = prices[prices.length - 1];
        for (int i = prices.length - 2; i >= 0; i--) {
            maxPrice = Math.max(maxPrice, prices[i]);
            right[i] = Math.max(right[i + 1], maxPrice - prices[i]);
        }
        
        int rst = 0;
        for (int i = 0; i < prices.length; i++) {
            rst = Math.max(rst, left[i] + right[i]);
        }
        return rst;
    }
};
```

Input: [3,3,5,0,0,3,1,4]
left:  [0,0,2,2,2,3,3,4]
right: [4,4,4,4,4,3,3,0]

## 1. Two Sum

Easy https://leetcode-cn.com/problems/two-sum/

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Version 1 暴力 O(n^2)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 2019.7.28
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
        else:
            return []
```

Version 2 快排 + 二分查找  时间 o(nlogn)

Version 3 哈希表 时间 O(n) 空间 O(n)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 2019.7.28
        d = {}
        for i, num in enumerate(nums):
            if target - num in d:
                return [d[target - num], i]
            d[num] = i
        else:
            return []
```

```java
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        // 2015-09-25 O(n) 时间
        if (numbers == null || numbers.length == 0) {
            return new int[2];
        }
        int[] rst = new int[2];
        HashMap<Integer, Integer> map = new HashMap<>();
        int count = 1;
        for (int i = 0; i < numbers.length; i++) {
            map.put(numbers[i], count++);
        }
        for (Integer temp : map.keySet()) {
            if (map.containsKey(target - temp) 
                && map.get(temp) != map.get(target - temp)) {
                rst[0] = Math.min(map.get(temp), map.get(target - temp));
                rst[1] = Math.max(map.get(temp), map.get(target - temp));
                return rst;
            }
        }
        return new int[2];
    }
}
```

## 15. 3Sum

Medium https://leetcode-cn.com/problems/3sum/

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? 

Find **all** unique triplets in the array which gives the sum of zero.

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> threeSum(int[] numbers) {
        // 2015-10-14 DFS 
        ArrayList<ArrayList<Integer>> rst = new ArrayList<>();
        if (numbers == null || numbers.length == 0) {
            return rst;
        }
        Arrays.sort(numbers);
        ArrayList<Integer> list = new ArrayList<>();
        helper(numbers, rst, list, 0);
        return rst;
    }
    
    private void helper(int[] numbers, ArrayList<ArrayList<Integer>> rst, 
            ArrayList<Integer> list, int pos) {
        if (list.size() == 3) {
            if (list.get(0) + list.get(1) + list.get(2) == 0 
                    && !rst.contains(list)) {
                rst.add(new ArrayList<Integer>(list));
            }
            return;
        }
        for (int i = pos; i < numbers.length; i++) {
            list.add(numbers[i]);
            helper(numbers, rst, list, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```