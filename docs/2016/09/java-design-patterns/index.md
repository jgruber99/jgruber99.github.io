# JAVA与模式－－设计模式


{{< admonition note "背景" true >}}
此篇只为导航，感谢博客园中的[**java-my-lift**](http://www.cnblogs.com/java-my-life/
)的详细讲解。
每天学一篇设计模式，记录到导航页中，不做转载。
{{< /admonition >}}

<!--more-->

### 1. [简单工厂模式][static-factory-method]

> 简单工厂模式是类的创建模式，又叫做静态工厂方法（Static Factory Method）模式。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。
> 
> 例子：登陆－－域名登陆，口令登陆

### 2. [工厂方法模式][factory-method]

> 工厂方法模式是类的创建模式，又叫做虚拟构造子(Virtual Constructor)模式或者多态性工厂（Polymorphic Factory）模式。
> 
> 工厂方法模式的用意是定义一个创建产品对象的工厂接口，将实际创建工作推迟到子类中。
> 
> 例子：导出文件－－导出Excel，导出HTML，导出PDF

### 3. [抽象工厂模式][abstract-factory]

> 提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
> 
> 例子：装机工程师组装电脑

### 4. [单例模式][singleton]

> 作为对象的创建模式，单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。

#### 4-1. 饿汉式单例模式
- 空间换时间
- 类装载的时候就创建实例
 
```java
public class EagerSingleton {
    private static EagerSingleton instance = new EagerSingleton();
    /**
     * 私有默认构造子
     */
    private EagerSingleton(){}
    /**
     * 静态工厂方法
     */
    public static EagerSingleton getInstance(){
        return instance;
    }
}
```

#### 4-2. 懒汉式单例模式
- 时间换空间
- 调用时再创建实例

```java
public class LazySingleton {
    private static LazySingleton instance = null;
    /**
     * 私有默认构造子
     */
    private LazySingleton(){}
    /**
     * 静态工厂方法
     */
    public static synchronized LazySingleton getInstance(){
        if(instance == null){
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

#### 4-3. 双重检查加锁
- ``volatile``关键字
- 线程安全
- 运行效率不高，不建议采用

```java
public class Singleton {
    private volatile static Singleton instance = null;
    private Singleton(){}
    public static Singleton getInstance(){
        //先检查实例是否存在，如果不存在才进入下面的同步块
        if(instance == null){
            //同步块，线程安全的创建实例
            synchronized (Singleton.class) {
                //再次检查实例是否存在，如果不存在才真正的创建实例
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

#### 4-4. 类级内部类
- 延迟加载
- 线程安全

```java
public class Singleton {
    
    private Singleton(){}
    /**
     *    类级的内部类，也就是静态的成员式内部类，该内部类的实例与外部类的实例
     *    没有绑定关系，而且只有被调用到时才会装载，从而实现了延迟加载。
     */
    private static class SingletonHolder{
        /**
         * 静态初始化器，由JVM来保证线程安全
         */
        private static Singleton instance = new Singleton();
    }
    
    public static Singleton getInstance(){
        return SingletonHolder.instance;
    }
}
```

#### 4-5. 枚举单例
- 简洁、高效、安全

```java
public enum Singleton {
    /**
     * 定义一个枚举的元素，它就代表了Singleton的一个实例。
     */
    
    uniqueInstance;
    
    /**
     * 单例可以有自己的操作
     */
    public void singletonOperation(){
        //功能处理
    }
}
```



### 5. [建造模式][builder]

> 建造模式是对象的创建模式。建造模式可以将一个产品的内部表象（internal representation）与产品的生产过程分割开来，从而可以使一个建造过程生成具有不同的内部表象的产品对象。
> 
> 例子：订阅杂志，发送欢迎邮件，取消订阅，发送欢送邮件



### 6. [原型模式][prototype]

> 原型模式术语对象的创建模式。通过给出一个原型对象来指明所有创建的对象的类型，然后用复制这个原型对象的办法创建出更多同类型的对象。这就是选型模式的用意。
> 
> 例子：美猴王分身


### 7. [适配器模式][adapter]

> 适配器模式把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够一起工作。
> 
> 例子：三相插头转到两相插头的转换器

### 8. [合成模式][composite]

> 合成模式属于对象的机构模式，有时又叫做“部分－－－－整体”模式。合成模式将对象组装到树结构中，可以用来描述整体与部分的关系。合成模式可以使客户端将单纯元素与复合元素同等看待。

### 9. [装饰模式][decorator]

> 装饰模式又名包装（Wrapper）模式。装饰模式以对客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。
> 
> 例子：齐天大圣七十二变，JAVA I/O库的基本模式

### 10. [代理模式][proxy]

> 代理模式是对象的结构模式。代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。

### 11. [享元模式][flyweight]

> 享元模式是对象的结构模式。享元模式以共享的方式高效地支持大量的细粒度对象。

### 12. [门面模式][facade]

> 门面模式是对象的结构模式，外部与一个子系统的通信必须通过一个统一的门面对象进行。门面模式提供一个高层次的接口，使得子系统更易于使用。
> 
> 例子：医院的各个部门由接待员负责。

### 13. [桥梁模式][bridge]

> 桥梁模式是对象的结构模式。又称为柄体(Handle and Body)模式或接口(interface)模式。桥梁模式的用意是“将抽象化(Abstraction)与实现化(Implementation)脱耦，使得二者可以独立地变化”。
> 
> 例子：发送消息－－普通消息，加急消息，特急消息
> 
> JDBC驱动器就是一个典型的桥梁模式。

### 14. [不变模式][immutable]

> 一个对象的状态在对象被创建之后就不再变化，这就是所谓的不变模式。
> 
> 不变模式只涉及到一个类。一个类的内部状态创建后，在整个生命周期都不会发生变化时，这样的类称作不变类。这种使用不变类的做法叫做不变模式。不变模式有两种形式：一种是弱不变模式，另一种是强不变模式。

### 15. [策略模式][strategy]

> 策略模式属于对象的行为模式。其用意是针对一组算法，将每一个算法封装到具有共同接口的独立类中，从而使得它们可以相互替换。策略模式使得算法可以在不影响到客户端的情况下发生改变。
> 
> 例子：购物网站，针对不同级别会员打折。

### 16. [模板方法模式][template-method]

> 模板方法模式是类的行为模式。准备一个抽象类，将部分逻辑以具体方法以及具体构造函数的形式实现，然后声明一些抽象方法来迫使子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法，从而对剩余的逻辑有不同的实现。这就是模版方法模式的用意。
> 
> 例子：存款利息－－货币市场和定期存款

### 17. [观察者模式][observer]

> 观察者模式是对象的行为模式，又叫发布－订阅(Publish/Subscribe)模式、模型－视图(Model/View)模式、源－监听器(Source/Listener)模式或从属者(Dependents)模式。
> 
> 观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态上发生变化时，会通知所有观察者对象，使它们能够自动更新自己。

### 18. [迭代子模式][iterator]

> 迭代子模式又叫游标(Cursor)模式，是对象的行为模式。迭代子模式可以顺序地访问一个聚集中的元素而不必暴露聚集的内部表象(internal representation)。

### 19. [责任链模式][chain-of-responsibility]

> 责任链模式时一种对象的行为模式。在责任链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。请求在这个链上传递，直到链上的某一个对象决定处理此请求。发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。
> 
> 例子：击鼓传花

### 20. [命令模式][command]

> 命令模式属于对象的行为模式。命令模式又称为行动(Action)模式或交易(Transaction)模式。
> 
> 命令模式把一个请求或者操作封装到一个对象中。命令模式允许系统使用不同的请求把客户端参数化，对请求排队活着记录请求日志，可以提供命令的撤销和恢复功能。
> 
> 例子：录音机

### 21. [备忘录模式][memento]

> 备忘录模式又叫做快照模式(Snapshot Pattern)或Token模式，是对象的行为模式。
> 
> 备忘录对象是一个用来存储另外一个对象内部状态的快找的对象。备忘录模式的用意是在不破坏封装的条件下，将一个对象的状态捕捉(Capture)住，并外部化，存储起来，从而可以在将来合适的时候把这个对象还原到存储起来的状态。备忘录模式常常与命令模式和迭代子模式一同使用。
> 
- 白箱
- 黑箱
- 多重检查点
- 自述历史

### 22. [状态模式][state]

> 状态模式，又称状态对象模式（Pattern of Objects for States），状态模式时对象的行为模式。
> 
> 状态模式允许一个对象在其内部状态改变的时候改变其行为。这个对象看上去就像是改变了它的类一样。
> 
> 例子：在线投票系统－－正常投票、重复投票、恶意刷屏、黑名单

### 23. [访问者模式][visitor]

> 访问者模式是对象的行为模式。访问者模式的目的是封装一些施加于某种数据结构元素之上的操作。一旦这些操作需要修改的话，接受这个操作的数据结构则可以保持不变。


### 24. [解释器模式][interpreter]

> 解释器是类的行为模式。给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。

### 25. [调停者模式][mediator]

> 调停者模式是对象的行为模式。调停者模式包装了一系列对象相互作用的方式，使得这些对象不必相互明显引用。从而使它们可以较松散地耦合。当这些对象中的某些对象之间的相互作用发生改变时，不会立即影响到其它的一些对象之间的相互作用。从而保证这些相互作用可以彼此独立地变化。










[java-my-lift]: http://www.cnblogs.com/java-my-life/

[static-factory-method]: http://www.cnblogs.com/java-my-life/archive/2012/03/22/2412308.html

[factory-method]: http://www.cnblogs.com/java-my-life/archive/2012/03/25/2416227.html

[abstract-factory]: http://www.cnblogs.com/java-my-life/archive/2012/03/28/2418836.html

[singleton]: http://www.cnblogs.com/java-my-life/archive/2012/03/31/2425631.html

[builder]: http://www.cnblogs.com/java-my-life/archive/2012/04/07/2433939.html

[prototype]: http://www.cnblogs.com/java-my-life/archive/2012/04/11/2439387.html

[adapter]: http://www.cnblogs.com/java-my-life/archive/2012/04/13/2442795.html

[composite]: http://www.cnblogs.com/java-my-life/archive/2012/04/17/2453861.html

[decorator]: http://www.cnblogs.com/java-my-life/archive/2012/04/20/2455726.html

[proxy]: http://www.cnblogs.com/java-my-life/archive/2012/04/23/2466712.html

[flyweight]: http://www.cnblogs.com/java-my-life/archive/2012/04/26/2468499.html

[facade]: http://www.cnblogs.com/java-my-life/archive/2012/05/02/2478101.html

[bridge]: http://www.cnblogs.com/java-my-life/archive/2012/05/07/2480938.html

[immutable]: http://www.cnblogs.com/java-my-life/archive/2012/05/08/2487757.html

[strategy]: http://www.cnblogs.com/java-my-life/archive/2012/05/10/2491891.html

[template-method]: http://www.cnblogs.com/java-my-life/archive/2012/05/14/2495235.html

[observer]: http://www.cnblogs.com/java-my-life/archive/2012/05/16/2502279.html

[iterator]: http://www.cnblogs.com/java-my-life/archive/2012/05/22/2511506.html

[chain-of-responsibility]: http://www.cnblogs.com/java-my-life/archive/2012/05/28/2516865.html

[command]: http://www.cnblogs.com/java-my-life/archive/2012/06/01/2526972.html

[memento]: http://www.cnblogs.com/java-my-life/archive/2012/06/06/2534942.html

[state]: http://www.cnblogs.com/java-my-life/archive/2012/06/08/2538146.html

[visitor]: http://www.cnblogs.com/java-my-life/archive/2012/06/14/2545381.html

[interpreter]: http://www.cnblogs.com/java-my-life/archive/2012/06/19/2552617.html

[mediator]: http://www.cnblogs.com/java-my-life/archive/2012/06/20/2554024.html
