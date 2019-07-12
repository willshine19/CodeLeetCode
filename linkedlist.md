## Linked List

### 876. Middle of the Linked List

https://leetcode-cn.com/problems/middle-of-the-linked-list/


### 203. Remove Linked List Elements

https://leetcode-cn.com/problems/remove-linked-list-elements/

Remove all elements from a linked list of integers that have value val.


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        while head and head.val == val:
            head = head.next
        if not head:
            return None
        
        node = head.next
        last = head
        while node:
            if node.val == val:
                last.next = node.next
            else:
                last = node
            node = node.next
        return head
        
```

### 83. Remove Duplicates from Sorted List

https://www.lintcode.com/problem/remove-duplicates-from-sorted-list/description

https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/

Input:  `1->1->2->3->3->null` Output: `1->2->3->null`

version 1 不好 和上一个比

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        # 2019.7.7
        if not head or not head.next:
            return head
        
        last_node = head;
        this_node = head.next;
        
        while this_node:
            if this_node.val == last_node.val:
                last_node.next = this_node.next
            else:
                last_node = this_node
            this_node = this_node.next
        return head
```

Version 2 更好 和下一个比

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        this_node = head
        while this_node and this_node.next:
            if this_node.val == this_node.next.val:
                this_node.next = this_node.next.next
            else:
                this_node = this_node.next
        return head
```


```java
public class Solution {
    public static ListNode deleteDuplicates(ListNode head) { 
        // 2015-4-22
        ListNode node = head;
        while(node != null && node.next != null) {
            if (node.val == node.next.val) {
                node.next = node.next.next;
            } else {
                node = node.next;
            }            
        }
        return head;
    }  
}
```

### 82. Remove Duplicates from Sorted List II

https://www.lintcode.com/problem/remove-duplicates-from-sorted-list-ii/description

Given `1->2->3->3->4->4->5`, return `1->2->5`

Version 1 更好

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        # 2019.7.7
        dummy = ListNode(0)
        dummy.next = head
        this_node = dummy
        while this_node.next and this_node.next.next:
            if this_node.next.val != this_node.next.next.val:
                this_node = this_node.next
            else:
                first_dup = this_node.next
                last_dup = this_node.next.next
                while last_dup.next and last_dup.next.val == last_dup.val:
                    last_dup = last_dup.next
                this_node.next = last_dup.next
        
        return dummy.next
```

```java
public class Solution {
    public static ListNode deleteDuplicates(ListNode head) {
        // 2015-08-18 思路更清晰
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        while (head.next != null && head.next.next != null) {
            if (head.next.val != head.next.next.val) {
                head = head.next;
            } else {
                ListNode firstDup = head.next;
                ListNode lastDup = head.next.next;
                while(lastDup.next != null && lastDup.next.val == firstDup.val) {
                    lastDup = lastDup.next;
                }
                head.next = lastDup.next;
            }
        }
        
        return dummy.next;
    }
}
```

Version 2 不好

```java
public class Solution {
    public static ListNode deleteDuplicates(ListNode head) {
        // 2015-4-22 该方法比较繁琐，
        // 引入了一个标志位dup，while中含三条分支，不推荐
        if (head == null) {
            return head;
        }
        boolean dup = false;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode last = dummy;
        while (head.next != null) {
            if (head.val != head.next.val && !dup) {
                last = head;
                head = head.next;
            } else if (head.val != head.next.val && dup) {
                dup = false;
                last.next = head.next;
                head = head.next;
            } else { //head.val == head.next.val
                dup = true;
                head = head.next;
            }
        }
        if (dup) {
            last.next = null;
        }
        return dummy.next;
    }
}
 
```

### 206. Reverse Linked List

https://www.lintcode.com/problem/reverse-linked-list/description
https://leetcode-cn.com/problems/reverse-linked-list/

Version 1 插入新链表

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        // 2019.7.7
        if not head or not head.next:
            return head
        
        dummy = ListNode(0)
        while head:
            temp = head
            head = head.next
            temp.next = dummy.next
            dummy.next = temp
        
        return dummy.next
```

```java
public class Solution {
    public ListNode reverse(ListNode head) {
        // 2015-08-26 
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        // dummy.next = head; 一定不能有
        ListNode temp;
        while (head != null) {
            // 把下一个节点先保存起来
            temp = head.next; 
            // 插入到dummy后面 不要理解为交换位置
            head.next = dummy.next;
            dummy.next  = head;
            // 指向下一节点
            head = temp;
        }
        return dummy.next;
    }
}
```

Version 2

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        // 2019.7.7 修改相邻位置的指针
        if not head or not head.next:
            return head
        
        left = head
        right = head.next
        left.next = None
        while right:
            head = right
            right = right.next
            head.next = left
            left = head
        return head
            
```

### 92. Reverse Linked List II

https://www.lintcode.com/problem/reverse-linked-list-ii/description

https://leetcode-cn.com/problems/reverse-linked-list-ii/

Input: 1->2->3->4->5->NULL, m = 2 and n = 4, 
Output: 1->4->3->2->5->NULL.

Given m, n satisfy the following condition: 1 ≤ m ≤ n ≤ length of list.

这题好烦， 关键要找到 node_m 的前一个节点， 根据 m ?= 1 分两种情况

```java
public class Solution {
    public ListNode reverseBetween(ListNode head, int m , int n) {
        // 2016-9-24
        if (head == null) {
            return head;
        }
        
        ListNode preHead = new ListNode(0);
        preHead.next = head;
        ListNode pre = preHead;
        ListNode node = head;
        for (int i = 1; i < m; i++) {
            pre = node;
            node = node.next;
        }
        pre.next = null;
        ListNode finalReverseNode = node;
        for (int i = 0; i < n - m + 1; i++) {
            ListNode temp = node.next;
            node.next = pre.next;
            pre.next = node;
            node = temp;
        }
        finalReverseNode.next = node;
        return preHead.next;
    }
}
```

```python
class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        if m < 1 or n < 1:
            return None

        if m == n:
            return head

        # part 1 find pre_m and node_m
        pre_m = ListNode(0)
        pre_m.next = head
        current_node = head
        pos = 1
        while pos < m:
            pre_m = current_node
            current_node = current_node.next
            pos += 1
        node_m = current_node

        # part 2
        while pos <= n:
            insert_node = current_node
            current_node = current_node.next
            insert_node.next = pre_m.next
            pre_m.next = insert_node
            pos += 1

        # part 3
        node_m.next = current_node

        if m == 1:
            return pre_m.next
        else:
            return head
```


### Partition List

https://www.lintcode.com/problem/partition-list/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @param x: an integer
     * @return: a ListNode 
     */
    public ListNode partition(ListNode head, int x) {
        // 2015-4-24
        if (head == null) {
            return head;
        }
        // 两个新的链表
        ListNode dummyA = new ListNode(0);
        ListNode nodeA = dummyA;
        ListNode dummyB = new ListNode(0);
        ListNode nodeB = dummyB;
        
        while (head != null) {
            if (head.val < x) {
                nodeA.next = head;
                nodeA = nodeA.next;
            } else {
                nodeB.next = head;
                nodeB = nodeB.next;
            }
            head = head.next;
        }
        nodeA.next = dummyB.next;
        nodeB.next = null;
        return dummyA.next;
    }
}
 
```

### Sort List

https://www.lintcode.com/problem/sort-list/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
     */
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

### Reorder List

https://www.lintcode.com/problem/reorder-list/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: void
     */
    public void reorderList(ListNode head) {  
        // 2015-08-22 基于栈 O(n)
        if (head == null || head.next == null) {
            return;
        }
        ListNode midNode = findMid(head);
        ListNode leftHead = head;
        ListNode rightHead = midNode.next;
        midNode.next = null;
        
        ArrayDeque<ListNode> stack = new ArrayDeque<>();
        // 入栈
        while (rightHead != null) {
            stack.push(rightHead);
            rightHead = rightHead.next;
        }
        // 出栈
        while (!stack.isEmpty()) {
            ListNode temp = leftHead.next;
            leftHead.next = stack.pop();
            leftHead = leftHead.next;
            leftHead.next = temp;
            if (leftHead.next != null) {
                leftHead = leftHead.next;
            }
        }
        return;
    }
    
    /**
     * return the middle node of the list
     */
     private ListNode findMid(ListNode head) {
         if (head == null || head.next == null) {
             return head;
         }
         ListNode fast = head;
         ListNode slow = head;
         while (fast.next != null && fast.next.next != null) {
             fast = fast.next.next;
             slow = slow.next;
         }
         return slow;
     }
}
```

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: void
     */
    public void reorderList(ListNode head) {  
        // 2015-04-24 O(n)
        if (head == null) {
            return;
        }
        ListNode mid = findMid(head);
        ListNode tail = reverseList(mid.next);
        mid.next = null;
        mergeList(head, tail);
    }
    
    private ListNode findMid (ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
    
    private ListNode reverseList(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        while (head != null) {
            ListNode temp = head.next;
            head.next = dummy.next;
            dummy.next = head;
            head = temp;
        }
        return dummy.next;
    }
    
    private ListNode mergeList(ListNode listA, ListNode listB) {
        // 已知 listB 比 listA 短一 或 相等
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while (listA != null && listB != null) {
            tail.next = listA;
            tail = tail.next;
            listA = listA.next;
            tail.next = listB;
            tail = tail.next;
            listB = listB.next;
        }
        if (listA != null) {
            tail.next = listA;
        }
        return dummy.next;
    }
}
 
```

### Rotate List

https://www.lintcode.com/problem/rotate-list/description

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param head: the List
     * @param k: rotate to the right k places
     * @return: the list after rotation
     */
    public ListNode rotateRight(ListNode head, int k) {
        // 2015-08-28 O(n) 
        // 这一题需要注意测试用例，k可以为0也可以很大
        if (head == null) {
            return head;
        }
        int length = getLength(head);
        k %= length;
        if (k == 0) {
            return head;
        }
        
        ListNode fast = head;
        ListNode slow = head;
        for (int i = 0; i < k; i++) {
            fast = fast.next;
        }
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        // 找到新的表头，断开并拼接链表
        ListNode firstNode = slow.next;
        slow.next = null;
        fast.next = head;
        
        return firstNode;
    }
    
    private int getLength(ListNode head) {
        int count = 0;
        while (head != null) {
            count++; 
            head = head.next;
        }
        return count;
    }
}
```

### Nth to Last Node in List

https://www.lintcode.com/problem/nth-to-last-node-in-list/

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @param n: An integer.
     * @return: Nth to last node of a singly linked list. 
     */
    ListNode nthToLast(ListNode head, int n) {
        // 2015-08-28
        if (head == null) {
            return null;
        }
        
        ListNode fast = head;
        ListNode slow = head;
        
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        
        return slow;
    }
}
```


### Add Two Numbers

https://www.lintcode.com/problem/add-two-numbers/description

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;      
 *     }
 * }
 */
public class Solution {
    /**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        // 2015-08-28
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        int carry = 0; // 进位
        
        while (l1 != null && l2 != null) {
            int sum = l1.val + l2.val + carry;
            carry = sum / 10;
            tail.next = new ListNode(sum % 10);
            l1 = l1.next;
            l2 = l2.next;
            tail = tail.next;
        }
        
        while (l1 != null) {
            int sum = l1.val + carry;
            carry = sum / 10;
            tail.next = new ListNode(sum % 10);
            l1 = l1.next;
            tail = tail.next;
        }
        
        while (l2 != null) {
            int sum = l2.val + carry;
            carry = sum / 10;
            tail.next = new ListNode(sum % 10);
            l2 = l2.next;
            tail = tail.next;
        }
        
        if (carry != 0) {
            tail.next = new ListNode(carry);
        }
        
        return dummy.next;
    }
}
```

### Insertion Sort List

https://www.lintcode.com/problem/insertion-sort-list/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: The head of linked list.
     */
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

### Linked List Cycle

https://www.lintcode.com/problem/linked-list-cycle/

Given a linked list, determine if it has a cycle in it.

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    public boolean hasCycle(ListNode head) {  
        // 2015-04-26
        if (head == null) {
            return false;
        }
        ListNode fast = head, slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
 
```

### Linked List Cycle II HARD

https://www.lintcode.com/problem/linked-list-cycle-ii/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: The node where the cycle begins. 
     *           if there is no cycle, return null
     */
    public ListNode detectCycle(ListNode head) {  
        // 2015-5-31 实在想不到
        if (head == null || head.next==null) {
            return null;
        }
 
        ListNode fast, slow;
        fast = head.next;
        slow = head;
        while (fast != slow) {
            if(fast==null || fast.next==null)
                return null;
            fast = fast.next.next;
            slow = slow.next;
        } 
        
        while (head != slow.next) {
            head = head.next;
            slow = slow.next;
        }
        return head;
    }
}
 
```

### Copy List with Random Pointer

https://www.lintcode.com/problem/copy-list-with-random-pointer/description

Version I HashMap

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
        // 2015-4-26 O(n)
        if (head == null) {
            return null;
        }
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>(); 
        RandomListNode dummy = new RandomListNode(0); // 新链表
        RandomListNode tail = dummy; // 新链表
        RandomListNode newNode;
        // 一次遍历
        while (head != null) {
            if (map.containsKey(head)) {
                newNode = map.get(head);
            } else {
                newNode = new RandomListNode(head.label);
                map.put(head, newNode);
            }
            tail.next = newNode;
            if (head.random != null) {
                if (map.containsKey(head.random)) {
                    newNode = map.get(head.random);
                } else {
                    newNode = new RandomListNode(head.random.label);
                    map.put(head.random, newNode);
                }
                tail.next.random = newNode;
            }
            head = head.next;
            tail = tail.next;
        }
        
        return dummy.next;
    }
}
```

Version II no HashMap

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {  
    /**
     * 遍历链表，将每一个节点复制一份，并插进原链表，插在被复制节点的下一个节点的位置，
     * 链表的长度将变为原来的二倍。
     */
    private void copyNode(RandomListNode head) {  
        while (head != null) {  
            RandomListNode newNode = new RandomListNode(head.label);  
            newNode.random = head.random;  
            newNode.next = head.next;  
            head.next = newNode;  
            head = head.next.next;  
        }  
    }  
    
    /**
     * 遍历链表，修正新节点的ramdom指针
     */
    private void copyRandom(RandomListNode head) {  
        while (head != null) {  
            if (head.next.random != null) {  
                head.next.random = head.random.next;  
            }  
            head = head.next.next;  
        }  
    }  
    
    /**
     * 遍历链表，将链表一分为二，将所有新节点分出来，并修正其next指针
     */
    private RandomListNode splitList(RandomListNode head) {  
        RandomListNode newHead = head.next;  
        while (head != null) {  
            RandomListNode temp = head.next;  
            head.next = temp.next;  
            head = head.next;  
            if (temp.next != null) {  
                temp.next = temp.next.next;  
            }  
        }  
        return newHead;  
    }  
    
    /**
     * Copy List with Random Pointer 
     */
    public RandomListNode copyRandomList(RandomListNode head) {  
        if (head == null) {  
            return null;  
        }  
        copyNode(head);  
        copyRandom(head);  
        return splitList(head);  
    }  
} 
 
```

### Remove Nth Node From End of List

https://www.lintcode.com/problem/remove-nth-node-from-end-of-list/description

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @param n: An integer.
     * @return: The head of linked list.
     */
    ListNode removeNthFromEnd(ListNode head, int n) {
        // 2015-04-29 O(n)
        if (head == null) {
            return null;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head; //被删除的节点可能是头节点，所以要用dummy
        head = dummy;
        ListNode nthNode = dummy;
        for (int i = 0; i < n; i++) {
            nthNode = nthNode.next;
            if (nthNode == null) {
                return null;
            }
        }
        while (nthNode.next != null) {
            head = head.next;
            nthNode = nthNode.next;
        }
        // 删掉head的下一个节点
        head.next = head.next.next;
        return dummy.next;
    }
}
 
```

### Merge k Sorted Lists

https://www.lintcode.com/problem/merge-k-sorted-lists/description

Version 1

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // 2015-08-21
        if (lists == null || lists.size() == 0) {
            return null;
        }
        
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy; // sorted list的尾节点
        // 这个数组保存了各链表中未排序的部分的表头
        ArrayList<ListNode> headList = new ArrayList<>(lists); 
        
        while (true) {
            int minValue = Integer.MAX_VALUE;
            int indexOfMin = -1;
            // 遍历数组，找到值最小的节点，并保存其序号
            for (int i = 0; i < headList.size(); i++) {
                ListNode temp = headList.get(i);
                if (temp == null) {
                    continue;
                }
                if (minValue > temp.val) {
                    minValue = temp.val;
                    indexOfMin = i;
                }                
            }
            // 更新sorted list和headList
            if (indexOfMin == -1) {
                break;
            } else {
                ListNode minNode = headList.get(indexOfMin);
                tail.next = minNode;
                tail = tail.next;
                headList.set(indexOfMin, minNode.next);
            }
        }
        return dummy.next;
    }
}
```

Version 2 堆排序

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    private Comparator<ListNode> ListNodeComparator = new Comparator<ListNode>() {
        public int compare(ListNode left, ListNode right) {
            if (left == null) {
                return 1;
            } else if (right == null) {
                return -1;
            }
            return left.val - right.val;
        }
    };
    
    public ListNode mergeKLists(List<ListNode> lists) {
        if (lists == null || lists.size() == 0) {
            return null;
        }
        
        Queue<ListNode> heap = new PriorityQueue<ListNode>(lists.size(), ListNodeComparator);
        for (int i = 0; i < lists.size(); i++) {
            if (lists.get(i) != null) {
                heap.add(lists.get(i));
            }
        }
        
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while (!heap.isEmpty()) {
            ListNode head = heap.poll();
            tail.next = head;
            tail = head;
            if (head.next != null) {
                heap.add(head.next);
            }
        }
        return dummy.next;
    }
}
 
```