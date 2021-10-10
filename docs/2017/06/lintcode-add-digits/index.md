# 各位相加


## lincode 访问地址

> [http://www.lintcode.com/zh-cn/problem/add-digits/][1]

### 描述

> 给出一个非负整数`num`，反复的将所有位上的数字相加，直到得到一个一位的整数。

### 样例

> 给出`num` = 38。
> 
> 相加的过程如下：`3 + 8 = 11, 1 + 1 = 2`。因为`2`只剩下一个数字，所以返回`2`。

### Java代码实现

```java
public class Solution {
    /**
     * @param num a non-negative integer
     * @return one digit
     */
    public int addDigits(int num) {
        // Write your code here
        if (num == 0) {
            return 0;
        }
        int temp = 0;
        for (; num != 0;) {
            int digit = num % 10;
            temp = (temp * 10 + digit) % 9;
            num = num / 10;
        }
        return temp == 0 ? 9 : temp;
    }
}
```

### Python代码实现

```python
class Solution:
    # @param {int} num a non-negative integer
    # @return {int} one digit
    def addDigits(self, num):
        # Write your code here
        if num == 0:
            return 0
        temp = 0
        while num != 0:
            digit = num % 10
            temp = (temp * 10 + digit) % 9
            num = num / 10
        return 9 if temp == 0 else temp
```

[1]:	http://www.lintcode.com/zh-cn/problem/add-digits/
