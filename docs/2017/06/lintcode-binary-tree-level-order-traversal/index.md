# 二叉树的层次遍历



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-level-order-traversal/](http://www.lintcode.com/zh-cn/problem/binary-tree-level-order-traversal/)

### 描述

> 给出一颗二叉树，返回其节点值的层次遍历（逐层从左往右访问）。

### 样例

> 给出一颗二叉树`{3,9,20,#,#,15,7}`：
> 
```
 3
 / \
9  20
  /  \
 15   7
```
> 返回他的分层遍历结果：
> 
```
[
  [3],
  [9,20],
  [15,7]
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
     * @param root: The root of binary tree.
     * @return: Level order a list of lists of integer
     */
    public ArrayList<ArrayList<Integer>> levelOrder(TreeNode root) {
        // write your code here
        ArrayList result = new ArrayList();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            ArrayList<Integer> level = new ArrayList<Integer>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode head = queue.poll();
                level.add(head.val);
                if (head.left != null) {
                    queue.offer(head.left);
                }
                if (head.right != null) {
                    queue.offer(head.right);
                }
            }
            result.add(level);
        }
        return result;
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
    @return: Level order in a list of lists of integers
    """
    def levelOrder(self, root):
        # write your code here
        self.results = []
        if not root:
            return self.results
        q = [root]
        while q:
            self.results.append([n.val for n in q])
            new_q = []
            for node in q:
                if node.left:
                    new_q.append(node.left)
                if node.right:
                    new_q.append(node.right)
            q = new_q
        return self.results
```

