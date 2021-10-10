# 二叉树的后序遍历



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-postorder-traversal/](http://www.lintcode.com/zh-cn/problem/binary-tree-postorder-traversal/)
> 
### 描述

> 给出一棵二叉树，返回其节点值的后序遍历。

### 样例

> 给出一棵二叉树 `{1,#,2,3}`,
> 
```
   1
    \
     2
    /
   3
```
> 返回 `[3,2,1]`

### 何为二叉树后序遍历

> 顺序：左 -\> 右 -\> 根
> 
> 获取到一个节点后，将其暂存，遍历完左右子树后，再输出该节点的值。

### Java代码实现

#### 非递归实现

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode node = root;
        TreeNode lastVisit = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
            node = stack.peek();
            if (node.right == null || node.right == lastVisit) {
                result.add(node.val);
                stack.pop();
                lastVisit = node;
                node = null;
            } else {
                node = node.right;
            }
        }
        return result;
    }
}
```

#### 递归实现

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        result.addAll(postorderTraversal(root.left));
        result.addAll(postorderTraversal(root.right));
        result.add(root.val);
        return result;
    }
}
```

### Python代码实现

#### 非递归实现

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
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        # write your code here
        result = []
        stack = []
        node = root
        last_visit = root
        while node or stack:
            while node:
                stack.append(node)
                node = node.left
            node = stack[-1]
            if node.right is None or node.right == last_visit:
                result.append(node.val)
                stack.pop()
                last_visit = node
                node = None
            else:
                node = node.right
        return result
```

#### 递归实现

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
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        # write your code here
        self.result = []
        self.traversal(root)
        return self.result
        
    def traversal(self, root):
        if root is None:
            return
        self.traversal(root.left)
        self.traversal(root.right)
        self.result.append(root.val)
```

