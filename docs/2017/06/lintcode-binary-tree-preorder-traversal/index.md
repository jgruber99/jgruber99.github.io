# 二叉树的前序遍历



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-preorder-traversal/](http://www.lintcode.com/zh-cn/problem/binary-tree-preorder-traversal/)

### 描述

> 给出一棵二叉树，返回其节点值的前序遍历。

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
> 返回 `[1,2,3]`

### 何为二叉树前序遍历

> 顺序： 根 -\> 左 -\> 右
> 
> 获取到一个节点后，即刻输出该节点的值，并继续遍历其左右子树。

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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                result.add(node.val);
                stack.push(node);
                node = node.left;
            }
            if (!stack.isEmpty()) {
                node = stack.pop();
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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        traversal(root, result);
        return result;
    }
    
    public void traversal(TreeNode root, ArrayList<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        traversal(root.left, result);
        traversal(root.right, result);
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
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        # write your code here
        result = []
        stack = []
        node = root
        while node or stack:
            while node:
                result.append(node.val)
                stack.append(node)
                node = node.left
            if stack:
                node = stack.pop()
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
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        # write your code here
        self.result = []
        self.traversal(root)
        return self.result
        
    def traversal(self, root):
        if root is None:
            return
        self.result.append(root.val)
        self.traversal(root.left)
        self.traversal(root.right)
```

