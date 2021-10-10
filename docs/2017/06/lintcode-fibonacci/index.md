# 斐波纳契数列



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/fibonacci/](http://www.lintcode.com/zh-cn/problem/fibonacci/)

### 描述

> 查找斐波纳契数列中第 N 个数。
> 
> 所谓的斐波纳契数列是指：
> 
> * 前2个数是 0 和 1 。
> * 第 i 个数是第 i-1 个数和第i-2 个数的和。
> 
> 斐波纳契数列的前10个数字是：
> 
> ``0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...``

#### 注意事项

> 在测试事例中，第n个斐波那契数不会超过32位字节整数的最大值。

### 样例

> 给定 `1`，返回 `0`
>
> 给定 `2`，返回 `1`
>
> 给定 `10`，返回 `34`

### Java代码实现

```java
class Solution {
    /**
     * @param n: an integer
     * @return an integer f(n)
     */
    public int fibonacci(int n) {
        // write your code here
        int a = 0;
        int b = 1;
        for (int i = 0; i < n - 1; i++) {
            int c = a + b;
            a = b;
            b = c;
        }
        return a;
    }
}
```

### Python代码实现

```python
class Solution:
    # @param n: an integer
    # @return an integer f(n)
    def fibonacci(self, n):
        # write your code here
        a = 0
        b = 1
        for i in range(n - 1):
            a, b = b, a + b
        return a
```
