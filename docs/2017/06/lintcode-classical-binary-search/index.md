# 经典二分查找问题



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/classical-binary-search/#](http://www.lintcode.com/zh-cn/problem/classical-binary-search/)

### 描述

> 在一个排序数组中找一个数，返回该数出现的任意位置，如果不存在，返回-1

### 样例

> 给出数组 `[1, 2, 2, 4, 5, 5]`.
> 
> 对于 target =`2`, 返回 1 或者 2.
> 
> 对于 target =`5`, 返回 4 或者 5.
> 
> 对于 target =`6`, 返回 -1.

### Java代码实现

```java
public class Solution {
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return an integer
     */
    public int findPosition(int[] nums, int target) {
        // Write your code here
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int begin = 0;
        int end = nums.length - 1;
        while (begin < end) {
            int mid = (begin + end) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                end = mid - 1;
            } else {
                begin = mid + 1;
            }
        }
        return -1;
    }
}
```

### Python代码实现

```python
class Solution:
    # @param {int[]} A an integer array sorted in ascending order
    # @param {int} target an integer
    # @return {int} an integer
    def findPosition(self, A, target):
        # Write your code here
        if A is None or len(A) == 0:
            return -1
        begin = 0
        end = len(A) - 1
        while begin < end:
            mid = (begin + end) / 2
            if A[mid] == target:
                return mid
            elif A[mid] > target:
                end = mid - 1
            else:
                begin = mid + 1
        return -1
```

