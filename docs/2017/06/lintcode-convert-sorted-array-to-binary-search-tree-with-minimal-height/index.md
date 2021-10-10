# 把排序数组转换为高度最小的二叉搜索树



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/convert-sorted-array-to-binary-search-tree-with-minimal-height/](http://www.lintcode.com/zh-cn/problem/convert-sorted-array-to-binary-search-tree-with-minimal-height/)

### 描述

> 给一个排序数组（从小到大），将其转换为一棵高度最小的排序二叉树。

#### 注意事项

> 有多个解决方法，返回任意一个即可。

### 样例

> 给出数组`[1,2,3,4,5,6,7]`, 返回
> 
```
     4
   /   \
  2     6
 / \    / \
1   3  5   7
```

### Java代码实现

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param A: an integer array
     * @return: a tree node
     */
    public TreeNode sortedArrayToBST(int[] A) {  
        // write your code here
        TreeNode root = null;
        int end = A.length - 1;
        root = recursion(A, 0, end, root);
        return root;
    }
    private TreeNode recursion(int[] A, int start, int end, TreeNode root) {
        if (start <= end) {
            int mid = (start + end) >> 1;
            root = new TreeNode(A[mid]);
            root.left = recursion(A, start, mid - 1, root.left);
            root.right = recursion(A, mid + 1, end, root.right);
        }
        return root;
    }
}
```

### Python代码实现

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:
    """
    @param A: a list of integer
    @return: a tree node
    """
    def sortedArrayToBST(self, A):
        # write your code here
        return self.recursion(A, 0, len(A) - 1,)
        
    def recursion(self, A, start, end):
        if start > end:
            return None
        if start == end:
            return TreeNode(A[start])
        mid = (start + end) >> 1
        root = TreeNode(A[mid])
        root.left = self.recursion(A, start, mid - 1)
        root.right = self.recursion(A, mid + 1, end)
        return root
```

