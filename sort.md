# sort

### Quick Sort

快速排序 https://blog.csdn.net/willshine19/article/details/52565739

```python
def quick_sort(collection):
    length = len(collection)
    if length <= 1:
        return collection
    else:
        pivot = collection.pop()
        greater, lesser = [], []
        for element in collection:
            if element > pivot:
                greater.append(element)
            else:
                lesser.append(element)
        return quick_sort(lesser) + [pivot] + quick_sort(greater)

```

```java
private static void quickSort(int[] array, int start, int end) {
    if (start < end) {
        int mid = partition(array, start, end);
        quickSort(array, start, mid - 1);
        quickSort(array, mid + 1, end);
    }
}

private static int partition(int[] src, int start, int end) {
    int target = src[start];
    while (start < end) {
        while (start < end && src[end] >= target) {
            end--;
        }
        src[start] = src[end];
        while (start < end && src[start] <= target) {
            start++;
        }
        src[end] = src[start];
    }
    // 此时 start == end
    src[start] = target;
    return start;
}
```

### Merge Sort

归并排序 https://blog.csdn.net/willshine19/article/details/52663843

```python
def merge_sort(collection):
    def merge(left, right):
        result = []
        while left and right:
            result.append((left if left[0] <= right[0] else right).pop(0))
        return result + left + right
    if len(collection) <= 1:
        return collection
    mid = len(collection) // 2
    return merge(merge_sort(collection[:mid]), merge_sort(collection[mid:]))
```

### Bubble Sort

```python
def bubble_sort(collection):
    length = len(collection)
    for i in range(length-1):
        swapped = False
        for j in range(length-1-i):
            if collection[j] > collection[j+1]:
                swapped = True
                collection[j], collection[j+1] = collection[j+1], collection[j]
        if not swapped: break  # Stop iteration if the collection is sorted.
    return collection
```

### 912 Sort an Array

https://leetcode-cn.com/problems/sort-an-array/


```python
```

### 148. Sort List

https://www.lintcode.com/problem/sort-list/description

https://leetcode-cn.com/problems/sort-list/

Sort a linked list in O(n log n) time using constant space complexity.

冒泡排序 超时

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        # 2019.7.8
        dummy = ListNode(0)
        while head:
            this = dummy
            while this.next:
                if head.val <= this.next.val:
                    head_next = head.next
                    head.next = this.next
                    this.next = head
                    head = head_next
                    break
                else:
                    this = this.next
            else:
                this.next = head
                head = head.next
                this.next.next = None
        return dummy.next 
```

```java
public class Solution {
    public ListNode sortList(ListNode head) {  
        // 2015-5-24 O(nlogn) 分治法 递归 类似归并排序
        // exit condition
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode mid = findMid(head);
        ListNode listB = sortList(mid.next);
        mid.next = null;
        ListNode listA = sortList(head);
        // listB 不长于 listA
        
        return mergeList(listA, listB);
        
    }
    
    private ListNode findMid(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    private ListNode mergeList(ListNode listA, ListNode listB) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while (listA != null && listB != null) {
            if (listA.val < listB.val) {
                tail.next = listA;
                listA = listA.next;
            } else {
                tail.next = listB;
                listB = listB.next;
            }
            tail = tail.next;
        }
        if (listA != null) {
            tail.next = listA;
        }
        if (listB != null) {
            tail.next = listB;
        }
        return dummy.next;
    }
}
```

### Insertion Sort List

https://leetcode-cn.com/problems/insertion-sort-list/

```java
public class Solution {
    public ListNode insertionSortList(ListNode head) {
        // 2015-08-28 O(n^2)
        // 建立新的dummy链
        ListNode dummy = new ListNode(0);
        
        // 遍历两个链表，一定是两个while循环嵌套        
        // 遍历未排序的链
        while (head != null) {
            // 从头遍历dummy链，找合适的插入位置
            ListNode insertPos = dummy;
            while (insertPos.next != null && insertPos.next.val < head.val) {
                insertPos = insertPos.next;
            }
            
            // 找到插入位置，在insertPoc.next的位置插入head节点
            ListNode temp = head.next;
            head.next = insertPos.next;
            insertPos.next = head;
            head = temp;
        }
 
        return dummy.next;
    }
}
```

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

medium https://leetcode-cn.com/problems/sort-colors/

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

## Partition Array

https://www.lintcode.com/problem/partition-array/description

Given an array nums of integers and an int k, partition the array (i.e move the elements in "nums") such that:

All elements < k are moved to the left
All elements >= k are moved to the right

```java
public class Solution {
    public int partitionArray(int[] nums, int k) {
	    // 2015-09-23 O(n) 
	    // 注意所有元素小于 k 的情况
	    if (nums == null || nums.length < 1) {
	        return 0;
	    }
	    
	    int start = 0;
	    int end = nums.length - 1;
	    while (start < end) {
	        if (nums[start] < k) {
	            start++;
	        } else if (nums[end] >= k) {
	            end--;
	        } else {
	            swap(nums, start, end);
	            start++;
	        }
	    }
	    // 此时 start = end
	    if (nums[start] < k) {
	        return start + 1;
	    } else {
	        return start;
	    }
    }
    
    private void swap(int[] nums, int start, int end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
    }
}
```
