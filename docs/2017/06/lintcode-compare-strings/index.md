# 比较字符串



## lintcode 访问路径

> [http://www.lintcode.com/zh-cn/problem/compare-strings/](http://www.lintcode.com/zh-cn/problem/compare-strings/)

### 描述

> 比较两个字符串A和B，确定A中是否包含B中所有的字符。字符串A和B中的字符都是`大写字母`。

#### 注意事项

> 在A中出现的B字符串里的字符不需要连续或者有序。

### 样例

> 给出 A = `"ABCD"` B = `"ACD"`，返回`true`
> 
> 给出 A = `"ABCD"` B = `"AABC"`，返回`false`

### Java代码实现

```java
public class Solution {
    /**
     * @param A : A string includes Upper Case letters
     * @param B : A string includes Upper Case letter
     * @return :  if string A contains all of the characters in B return true else return false
     */
    public boolean compareStrings(String A, String B) {
        // write your code here
        if (A == null || B == null || A.length() < B.length()) {
            return false;
        }
        int[] check = new int[26];
        for (int i = 0; i < A.length(); i++) {
            check[A.charAt(i) - 'A']++;
        }
        for (int i = 0; i < B.length(); i++) {
            check[B.charAt(i) - 'A']--;
            if (check[B.charAt(i) - 'A'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

### Python代码实现

```python
class Solution:
    """
    @param A : A string includes Upper Case letters
    @param B : A string includes Upper Case letters
    @return :  if string A contains all of the characters in B return True else return False
    """
    def compareStrings(self, A, B):
        # write your code here
        if A is None or B is None:
            return False
        if len(B) == 0:
            return True
        if len(A) == 0:
            return False
        check = [0 for _ in range(26)]
        for i in A:
            check[ord(i) - 65] += 1
        for i in B:
            check[ord(i) - 65] -= 1
            if check[ord(i) - 65] < 0:
                return False
        return True
```

