# 两个大整数相加



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/big-integer-addition/][1]

### 描述

> 给出两个非负整数`num1`和`num2`的字符串形式，返回`num1`和`num2`的和。

#### 注意事项

> * 两个字符串`num1`和`num2`的长度小于5100。
> * 两个字符串`num1`和`num2`只包含[0-9]()(#)。
> * 两个字符串`num1`和`num2`不会以`0`开头。
> * 不能使用任何整数类库，也不能将输入参数直接转换成整数。

### 样例

> 给出 num1 = `"123"`，num2 = `"45"`， 返回 `"168"`。

### Java代码实现

```java
public class Solution {
    /**
     * @param num1 a non-negative integers
     * @param num2 a non-negative integers
     * @return return sum of num1 and num2
     */
    public String addStrings(String num1, String num2) {
        // Write your code here
        int lena = num1.length() - 1;
        int lenb = num2.length() - 1;
        int temp = 0;
        String result = "";
        while (lena >= 0 || lenb >= 0) {
            int x = 0;
            int y = 0;
            if (lena >= 0) {
                x = num1.charAt(lena) - '0';
            }
            if (lenb >= 0) {
                y = num2.charAt(lenb) - '0';
            }
            int sum = x + y + temp;
            result = String.valueOf(sum % 10) + result;
            temp = sum / 10;
            lena = lena - 1;
            lenb = lenb - 1;
        }
        if (temp > 0) {
            result = String.valueOf(temp) + result;
        }
        return result;
    }
}

```

### Python代码实现

```python
class Solution:
    # @param {string} num1 a non-negative integers
    # @param {string} num2 a non-negative integers
    # @return {string} return sum of num1 and num2
    def addStrings(self, num1, num2):
        # Write your code here
        lena = len(num1) - 1
        lenb = len(num2) - 1
        temp = 0
        result = ""
        while lena >= 0 or lenb >= 0:
            x = int(num1[lena]) if lena >= 0 else 0
            y = int(num2[lenb]) if lenb >= 0 else 0
            sum = x + y + temp
            result = str(sum % 10) + result
            temp = sum / 10
            lena, lenb = lena - 1, lenb - 1
        if temp > 0:
            result = str(temp) + result
        return result

```

[1]:	http://www.lintcode.com/zh-cn/problem/big-integer-addition/

