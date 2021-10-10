# 二叉树的中序遍历



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-inorder-traversal/][1]

### 描述

> 给出一棵二叉树,返回其中序遍历。

### 样例

> 给出二叉树`{1, #, 2, 3}`
> 
```
   1
	\
	 2
	/
   3
```
> 返回`[1,3,2]`。

### 何为二叉树中序遍历

> 顺序：左 -\> 根 -\> 右
> 
> 获取到一个节点后，将其暂存，遍历完左子树后，再输出该节点的值，然后再遍历右子树。

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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // write your code here
        Stack<TreeNode> treeNodeStack = new Stack<TreeNode>();
        ArrayList<Integer> result = new ArrayList<Integer>();
        TreeNode node = root;
        while (node != null || !treeNodeStack.empty()) {
            while (node != null) {
                treeNodeStack.push(node);
                node = node.left;
            }
            if (!treeNodeStack.empty()) {
                node = treeNodeStack.pop();
                result.add(node.val);
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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        recursionMiddleorderTraversal(root, result);
        return result;
    }
    public static void recursionMiddleorderTraversal(TreeNode root, ArrayList<Integer> result) {
        if (root != null) {
            recursionMiddleorderTraversal(root.left, result);
            result.add(root.val);
            recursionMiddleorderTraversal(root.right, result);
        }
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
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        # write your code here
        stack = []
        result = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
                node = node.left
            if stack:
                node = stack.pop()
                result.append(node.val)
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
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        # write your code here
        if root is None:
            return []
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```

[1]:	http://www.lintcode.com/zh-cn/problem/binary-tree-inorder-traversal/
