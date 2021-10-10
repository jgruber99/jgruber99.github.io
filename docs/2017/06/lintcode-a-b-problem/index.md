# A + B 问题



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/a-b-problem/](http://www.lintcode.com/zh-cn/problem/a-b-problem/)

### 描述

> 给出两个整数a和b, 求他们的和, 但不能使用`+`等数学运算符。

#### 注意事项

> 你不需要从输入流读入数据，只需要根据aplusb的两个参数a和b，计算他们的和并返回就行。

### 说明

> a和b都是`32位`整数么？
> 
> * 是的
> 
> 我可以使用位运算符么？
> 
> * 当然可以

### 样例

> 如果`a=1`并且`b=2`，返回`3`

<!-- more -->

### Java代码实现

```java
class Solution {
    /*
     * param a: The first integer
     * param b: The second integer
     * return: The sum of a and b
     */
    public int aplusb(int a, int b) {
        // write your code here, try to do it without arithmetic operators.
        if (b == 0) {
            return a;
        }
        return aplusb(a ^ b, (a & b) << 1);
    }
}
```

### Python代码实现

```python
class Solution:
    """
    @param a: The first integer
    @param b: The second integer
    @return:  The sum of a and b
    """
    def aplusb(self, a, b):
        # write your code here, try to do it without arithmetic operators.
        if b == 0:
            return a
        if a == 0:
            return b
        while b != 0:
            carry = (a & b) << 1
            a = a ^ b
            b = carry
        return a
```

> 注意：由于有[100,-100]这个参数，所以会超时！

