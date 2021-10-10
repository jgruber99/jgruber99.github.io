# 删除链表中的元素



## lintcode 访问地址

> [http://www.lintcode.com/zh-cn/problem/remove-linked-list-elements/](http://www.lintcode.com/zh-cn/problem/remove-linked-list-elements/)

### 描述

> 删除链表中等于给定值`val`的所有节点。

### 样例

> 给出链表`1->2->3->3->4->5->3`, 和 val =`3`, 你需要返回删除3之后的链表：`1->2->4->5`。

### Java代码实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    /**
     * @param head a ListNode
     * @param val an integer
     * @return a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        // Write your code here
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        head = dummy;
        while (head.next != null) {
            if (head.next.val == val) {
                head.next = head.next.next;
            } else {
                head = head.next;
            }
        }
        return dummy.next;
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
    # @param head, a ListNode
    # @param val, an integer
    # @return a ListNode
    def removeElements(self, head, val):
        # Write your code here
        if head is None:
            return head
        dummy = ListNode(-1)
        dummy.next = head
        pre = dummy
        while head:
            if head.val == val:
                pre.next = head.next
            else:
                pre = pre.next
            head = head.next
        return dummy.next
```
