# 二叉树的所有路径



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/binary-tree-paths/](http://www.lintcode.com/zh-cn/problem/binary-tree-paths/)

### 描述

> 给一棵二叉树，找出从根节点到叶子节点的所有路径。

### 样例

> 给出下面这棵二叉树：
> 
```
   1
 /   \
2     3
 \
  5
```
> 所有根到叶子的路径为：
> 
```
[
  "1->2->5",
  "1->3"
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
     * @param root the root of the binary tree
     * @return all root-to-leaf paths
     */
    public List<String> binaryTreePaths(TreeNode root) {
        // Write your code here
        List<String> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        String path = "";
        paths(root, path, result);
        return result;
    }
    
    public void paths(TreeNode root, String path, List<String> result) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            if ("".equals(path)) {
                path += root.val;
            } else {
                path += "->" + root.val;
            }
            result.add(path);
            return;
        }
        
        if ("".equals(path)) {
            path += root.val;
        } else {
            path += "->" + root.val;
        }

        paths(root.left, path, result);
        paths(root.right, path, result);
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
    # @param {TreeNode} root the root of the binary tree
    # @return {List[str]} all root-to-leaf paths
    def binaryTreePaths(self, root):
        # Write your code here
        result = []
        if root is None:
            return result
        path = ""
        self.paths(root, path, result)
        return result
        
    def paths(self, root, path, result):
        if root is None:
            return
        if root.left is None and root.right is None:
            if path == "":
                path = str(root.val)
            else:
                path = str(path) + "->" + str(root.val)
            result.append(path)
            return;
        if path == "":
            path = str(root.val)
        else:
            path = str(path) + "->" + str(root.val)
        self.paths(root.left, path, result)
        self.paths(root.right, path, result)
```

