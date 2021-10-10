# 克隆二叉树



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/clone-binary-tree/](http://www.lintcode.com/zh-cn/problem/clone-binary-tree/)

### 描述

> 深度复制一个二叉树。
> 
> 给定一个二叉树，返回一个他的 克隆品 。

### 样例

> 给定一个二叉树：
> 
```
     1
   /  \
  2    3
 / \
4   5
```
> 返回其相同结构相同数值的克隆二叉树：
> 
```
    1
   /  \
  2    3
 / \
4   5
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
     * @param root: The root of binary tree
     * @return root of new tree
     */
    public TreeNode cloneTree(TreeNode root) {
        // Write your code here
        if (root == null) {
            return null;
        }
        TreeNode c_root = new TreeNode(root.val);
        c_root.left = cloneTree(root.left);
        c_root.right = cloneTree(root.right);
        return c_root;
    }
}
```

### Python代码实现

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        this.val = val
        this.left, this.right = None, None
"""
class Solution:
    """
    @param {TreeNode} root: The root of binary tree
    @return {TreeNode} root of new tree
    """
    def cloneTree(self, root):
        # Write your code here
        if root is None:
            return None
        c_root = TreeNode(root.val)
        c_root.left = self.cloneTree(root.left)
        c_root.right = self.cloneTree(root.right)
        return c_root
```

