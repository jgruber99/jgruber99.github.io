# 平衡二叉树


## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/balanced-binary-tree/][1]

### 描述

> 给定一个二叉树,确定它是高度平衡的。对于这个问题,一棵高度平衡的二叉树的定义是：一棵二叉树中每个节点的两个子树的深度相差不会超过1。

### 样例

> 给出二叉树 A=`{3,9,20,#,#,15,7}`, B=`{3,#,20,15,7}`
> 
```
A)  3            B)    3 
   / \                  \
  9  20                 20
    /  \                / \
   15   7              15  7
```
> 二叉树A是高度平衡的二叉树，但是B不是

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
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) {
        // write your code here
        return maxDepth(root) != -1;
    }
    private int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        if (left == -1 || right == -1 || Math.abs(left - right) > 1) {
            return -1;
        }
        return Math.max(left, right) + 1;
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
    @param root: The root of binary tree.
    @return: True if this Binary tree is Balanced, or false.
    """
    def isBalanced(self, root):
        # write your code here
        balanced, _ = self.validate(root)
        return balanced
        
    def validate(self, root):
        if root is None:
            return True, 0
        balanced, left = self.validate(root.left)
        if not balanced:
            return False, 0
        balanced, right = self.validate(root.right)
        if not balanced:
            return False, 0
        return abs(left - right) < 2, max(left, right) + 1
```

[1]:	http://www.lintcode.com/zh-cn/problem/balanced-binary-tree/
