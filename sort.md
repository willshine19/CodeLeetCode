# sort

快速排序 https://blog.csdn.net/willshine19/article/details/52565739

归并排序 https://blog.csdn.net/willshine19/article/details/52663843

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

https://www.lintcode.com/problem/insertion-sort-list/description

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

