# 二进制求和



## lincode 访问地址

> [http://www.lintcode.com/zh-cn/problem/add-binary/](http://www.lintcode.com/zh-cn/problem/add-binary/)

### 描述

> 给定两个二进制字符串，返回他们的和（用二进制表示）。

### 样例

> a = `11`
> b = `1`
> 返回 `100`

### Java代码实现

```java
public class Solution {
    /**
     * @param a a number
     * @param b a number
     * @return the result
     */
    public String addBinary(String a, String b) {
        // Write your code here
        if (a.length() < b.length()) {
            String tmp = a;
            a = b;
            b = tmp;
        }
        int pa = a.length() - 1;
        int pb = b.length() - 1;
        int carries = 0;
        String rst = "";
        while (pb >= 0) {
            int sum = (int) (a.charAt(pa) - '0') + (int) (b.charAt(pb) - '0') + carries;
            rst = String.valueOf(sum % 2) + rst;
            carries = sum / 2;
            pa--;
            pb--;
        }
        while (pa >= 0) {
            int sum = (int) (a.charAt(pa) - '0') + carries;
            rst = String.valueOf(sum % 2) + rst;
            carries = sum / 2;
            pa--;
        }
        if (carries == 1) {
            rst = "1" + rst;
        }
        return rst;
    }
}
```

### Python代码实现

```python
class Solution:
    # @param {string} a a number
    # @param {string} b a number
    # @return {string} the result
    def addBinary(self, a, b):
        # Write your code here
        lena = len(a) - 1
        lenb = len(b) - 1
        temp = 0
        result = ""
        while lena >= 0 or lenb >= 0:
            x = int(a[lena]) if lena >= 0 else 0
            y = int(b[lenb]) if lenb >= 0 else 0
            sum = x + y + temp
            result = str(sum % 2) + result
            temp = sum / 2
            lena, lenb = lena - 1, lenb - 1
        if temp > 0:
            result = "1" + result
        return result

```

