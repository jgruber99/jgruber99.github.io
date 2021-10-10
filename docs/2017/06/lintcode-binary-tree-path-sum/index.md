# 二叉树的路径和



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-path-sum/](http://www.lintcode.com/zh-cn/problem/binary-tree-path-sum/)

### 描述

> 给定一个二叉树，找出所有路径中各节点相加总和等于给定`目标值`的路径。一个有效的路径，指的是从根节点到叶节点的路径。

### 样例

> 给定一个二叉树，和`目标值 = 5`：
> 
```
     1
    / \
   2   4
  / \
 2   3
```
> 返回：
> 
```
[
  [1, 2, 2],
  [1, 4]
]
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
     * @param target an integer
     * @return all valid paths
     */
    public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
        // Write your code here
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> path = new ArrayList<Integer>();
        search(root, target, path, result);
        return result;
    }
    private void search(TreeNode root, int target, List<Integer> path, List<List<Integer>> result) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null && root.val == target) {
            path.add(root.val);
            result.add(path);
            return;
        }
        if (root.left != null) {
            List<Integer> left = new ArrayList<Integer>(path);
            left.add(root.val);
            search(root.left, target - root.val, left, result);
        }
        if (root.right != null) {
            List<Integer> right = new ArrayList<Integer>(path);
            right.add(root.val);
            search(root.right, target - root.val, right, result);
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
    # @param {int} target an integer
    # @return {int[][]} all valid paths
    def binaryTreePathSum(self, root, target):
        # Write your code here
        result = []
        path = []
        self.search(root, target, path, result)
        return result
        
    def search(self, root, target, path, result):
        if root is None:
            return
        if root.left is None and root.right is None and root.val == target:
            path.append(root.val)
            result.append(path)
            return
        if root.left:
            left = path[:]
            left.append(root.val)
            self.search(root.left, target - root.val, left, result)
        if root.right:
            right = path[:]
            right.append(root.val)
            self.search(root.right, target - root.val, right, result)
```

