# 整数排序



## lintcode 访问地址

> [http://www.lintcode.com/zh-cn/problem/sort-integers/](http://www.lintcode.com/zh-cn/problem/sort-integers/)

### 描述

> 给一组整数，按照升序排序，使用选择排序，冒泡排序，插入排序或者任何 O(n2) 的排序算法。

### 样例

对于数组`[3, 2, 1, 4, 5]`, 排序后为`[1, 2, 3, 4, 5]`。

### 直接插入排序

#### Java代码实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return void
     */
    public void sortIntegers(int[] A) {
        // Write your code here
        int length = A.length;
        int insertNum;
        for (int i = 1; i < length; i++) {
            insertNum = A[i];
            int j = i - 1;
            while (j >= 0 && A[j] > insertNum) {
                A[j + 1] = A[j];
                j--;
            }
            A[j + 1] = insertNum;
        }
    }
}
```

#### Python代码实现

```python
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        # Write your code here
        length = len(A)
        for i in range(1, length):
            key = A[i]
            j = i - 1
            while j >= 0 and A[j] > key:
                A[j + 1] = A[j]
                j -= 1
            A[j + 1] = key
        return A
```

### 选择排序

#### Java代码实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return void
     */
    public void sortIntegers(int[] A) {
        // Write your code here
        int length = A.length;
        for (int i = 0; i < length; i++) {
            int key = A[i];
            int position = i;
            for (int j = i + 1; j < length; j++) {
                if (A[j] < key) {
                    key = A[j];
                    position = j;
                }
            }
            A[position] = A[i];
            A[i] = key;
        }
    }
}
```

#### Python代码实现

```python
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        # Write your code here
        length = len(A)
        for i in range(0, length):
            min = i
            for j in range(i + 1, length):
                if A[min] > A[j]:
                    min = j
            A[min], A[i] = A[i], A[min]
        return A
```

### 冒泡排序

#### Java代码实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return void
     */
    public void sortIntegers(int[] A) {
        // Write your code here
        int length = A.length;
        int temp;
        for (int i = 0; i < length; i++) {
            for (int j = i + 1; j < length; j++) {
                if (A[i] > A[j]) {
                    temp = A[i];
                    A[i] = A[j];
                    A[j] = temp;
                }
            }
        }
    }
}
```

#### Python代码实现

```python
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        # Write your code here
        length = len(A)
        for i in range(0, length):
            for j in range(i + 1, length):
                if A[i] > A[j]:
                    A[i], A[j] = A[j], A[i]
        return A
```

