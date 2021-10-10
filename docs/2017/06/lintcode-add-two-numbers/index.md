# 二叉树的最大节点



## lintcode 访问地址

> [http://www.lintcode.com/zh-cn/problem/add-two-numbers/][1]

### 描述

> 你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中`相反`的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。

### 样例

给出两个链表`3->1->5->null`和`5->9->2->null`，返回`8->0->8->null`。

### Java代码实现

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
        // write your code here
        if (l1 == null && l2 == null) {
            return null;
        }
        ListNode head = new ListNode(0);
        ListNode ptr = head;
        int carry = 0;
        while (true) {
            if (l1 != null) {
                carry += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                carry += l2.val;
                l2 = l2.next;
            }
            ptr.val = carry % 10;
            carry = carry / 10;
            if (l1 != null || l2 != null || carry != 0) {
                ptr.next = new ListNode(0);
                ptr = ptr.next;
            } else {
                break;
            }
        }
        return head;
    }
}
```

### Python代码实现

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # @param l1: the first list
    # @param l2: the second list
    # @return: the sum list of l1 and l2 
    def addLists(self, l1, l2):
        # write your code here
        head = ListNode(0)
        ptr = head
        carry = 0
        while True:
            if l1 is not None:
                carry += l1.val
                l1 = l1.next
            if l2 is not None:
                carry += l2.val
                l2 = l2.next
            ptr.val = carry % 10
            carry = carry / 10
            if l1 is not None or l2 is not None or carry != 0:
                ptr.next = ListNode(0)
                ptr = ptr.next
            else:
                break
        return head
```

[1]:	http://www.lintcode.com/zh-cn/problem/add-two-numbers/
