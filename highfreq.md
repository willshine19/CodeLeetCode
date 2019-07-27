# High Frequency

## O(1) Check Power of 2

https://www.lintcode.com/problem/o1-check-power-of-2/description

```java
class Solution {
    /*
     * @param n: An integer
     * @return: True or false
     */
    public boolean checkPowerOf2(int n) {
        // 2015-09-06
        if (n <= 0) {
            return false;
        }
        boolean res = ((n & (n-1)) == 0) ? true : false;
        return res;
    }
};
```


## Sqrt(x)

https://www.lintcode.com/problem/sqrtx/description

```java
class Solution {
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    public int sqrt(int x) {
        // 2015-09-06 用int会溢出
        long start = 0;
        long end = x / 2 + 1;
        while (start <= end) {
            long mid = start + (end - start) / 2;
            if ((mid * mid) > x) {
                end = mid - 1;
            } else if ((mid + 1) * (mid + 1) <= x) {
                start = mid + 1;
            } else {
                return (int)mid;
            }
        }
        return 0;
    }
}
```
