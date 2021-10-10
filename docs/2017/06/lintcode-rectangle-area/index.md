# 矩阵面积


## lincode 访问路径

> [http://www.lintcode.com/zh-cn/problem/rectangle-area/](http://www.lintcode.com/zh-cn/problem/rectangle-area/)

### 描述

> 实现一个矩阵类`Rectangle`，包含如下的一些成员变量与函数：
>
> 1. 两个共有的成员变量`width`和`height`分别代表宽度和高度。
> 2. 一个构造函数，接受2个参数`width`和`height`来设定矩阵的宽度和高度。
> 3. 一个成员函数 getArea，返回这个矩阵的面积。

### 样例

```java
Rectangle rec = new Rectangle(3, 4);
rec.getArea(); // should get 12
```

### Java代码实现

```java
public class Rectangle {
    /*
     * Define two public attributes width and height of type int.
     */
    // write your code here
    public int width;
    public int height;

    /*
     * Define a constructor which expects two parameters width and height here.
     */
    // write your code here
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    /*
     * Define a public method `getArea` which can calculate the area of the
     * rectangle and return.
     */
    // write your code here
    public int getArea() {
        return this.width * this.height;
    }
}
```

### Python代码实现

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = int(width)
        self.height = int(height)

    def get_area(self):
        return self.width * self.height
```
