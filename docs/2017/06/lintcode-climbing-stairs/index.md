# 爬楼梯



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/climbing-stairs/](http://www.lintcode.com/zh-cn/problem/climbing-stairs/)

### 描述

> 假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

### 样例

> 比如n=3，`1+1+1=1+2=2+1=3`，共有`3`中不同的方法
> 
> 返回3

### Java代码实现

#### 非递归实现

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
        if (n < 2) {
            return 1;
        }
        int a = 1;
        int b = 1;
        int temp = 0;
        for (int i = 2; i < n + 1; i++) {
            temp = a + b;
            a = b;
            b = temp;
        }
        return temp;
    }
}
```

#### 递归实现

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
        if (n < 2) {
            return 1;
        }
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```
> n 大的时候会超时

### Python代码实现

#### 非递归实现

```python
class Solution:
    """
    @param n: An integer
    @return: An integer
    """
    def climbStairs(self, n):
        # write your code here
        if n < 2:
            return 1
        result = [1, 2]
        for i in range(n - 2):
            result.append(result[-1] + result[-2])
        return result[-1]

```

#### 递归实现

```python
class Solution:
    """
    @param n: An integer
    @return: An integer
    """
    def climbStairs(self, n):
        # write your code here
        if n < 2:
            return 1
        return self.climbStairs(n - 1) + self.climbStairs(n - 2)
```

