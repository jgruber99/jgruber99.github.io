# 二叉搜索树转变为大数二叉搜索树



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/convert-bst-to-greater-tree/](http://www.lintcode.com/zh-cn/problem/convert-bst-to-greater-tree/)

### 描述

> 给出一个二叉搜索树，将每个节点重新赋值，本节点的值加上所有比本节点值大的节点的值。

### 样例

> 给出一个二叉搜索树 `{5, 2, 3}`：
> 
```
              5
            /   \
           2     13
```
> 返回一个新树
> 
```
             18
            /   \
          20     13
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
     * @param root the root of binary tree
     * @return the new root
     */
    public TreeNode convertBST(TreeNode root) {
        // Write your code here
        convert(root);
        return root;
    }
    private int sum = 0;
    private void convert(TreeNode root) {
        if (root == null) {
            return;
        }
        if (root.right != null) {
            convert(root.right);
        }
        sum += root.val;
        root.val = sum;
        if (root.left != null) {
            convert(root.left);
        }
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
    # @param {TreeNode} root the root of binary tree
    # @return {TreeNode} the new root
    def convertBST(self, root):
        # Write your code here
        self.sum = 0
        self.convert(root)
        return root
        
    def convert(self, root):
        if root is None:
            return
        if root.right:
            self.convert(root.right)
        self.sum += root.val
        root.val = self.sum
        if root.left:
            self.convert(root.left)
```

