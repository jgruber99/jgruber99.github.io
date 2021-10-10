# 二叉树的最大节点


## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-maximum-node/](http://www.lintcode.com/zh-cn/problem/binary-tree-maximum-node/)

### 描述

> 在二叉树中寻找值最大的节点并返回。

### 样例

给出如下一颗二叉树：

		 1
	   /   \
	 -5     2
	 / \   /  \
	0   3 -4  -5 


返回值为`3`的节点。

### Java代码实现

```java
public class Solution {
    /**
     * @param root the root of binary tree
     * @return the max ndoe
     */
    public TreeNode maxNode(TreeNode root) {
        // Write your code here
        if (root == null)
            return root;
        
        TreeNode left = maxNode(root.left);
        TreeNode right = maxNode(root.right);
        
        return max(root, max(left, right));
    }
    
    TreeNode max(TreeNode a, TreeNode b) {
        if (a == null) return b;
        if (b == null) return a;
        
        if (a.val > b.val) return a;
        
        return b;
    }
}
```

### Python代码实现

```python
class Solution:
    # @param {TreeNode} root the root of binary tree
    # @return {TreeNode} the max node
    def maxNode(self, root):
        # Write your code here
        if root is None:
            return None
        left = self.maxNode(root.left)
        right = self.maxNode(root.right)
        return self.max(root, self.max(left, right))
        
    def max(self, a, b):
        if a is None:
            return b
        if b is None:
            return a
        if a.val > b.val:
            return a
        return b
```

