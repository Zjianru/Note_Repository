# Object类简述

## 一点概念

- 在 java 中 Object 类作为所有类的终极父类存在，所有类都继承于 Object 类
- 在不给出某类的具体父类的时候， java 自动将 Object 做为该类的父类
- 因为继承的原因，可以使用 Object 类型指代任意类型的对象 （ 向上自动转型 ）。但只可作为数据持有对象，处理数据还是要把对象转化成特定对象
- Object 类有一个默认的构造方法，在构造子类实例时，都会先调用这个默认构造方法

---

##  API 简介（11个）

- ### `equals( Object obj )`
```java
public boolean equals( Object obj )

    指示其他某个对象是否与此对象“相等”。
    参数：
        obj - 要与之比较的引用对象。
    返回：
        如果此对象与 obj 参数相同，则返回 true；否则返回 false。
```

equals 方法用于判断对象相等：
- 自反性：对于任何非空引用值 `x` ， `x.equals(x)` 都应返回 true。
- 对称性：对于任何非空引用值 x 和 y，当且仅当 `y.equals(x)` 返回 true 时，`x.equals(y)` 才应返回 true。
- 传递性：对于任何非空引用值 x、y 和 z，如果 `x.equals(y)` 返回 true，并且 `y.equals(z)` 返回 true，那么 `x.equals(z)` 应返回 true。
- 一致性：对于任何非空引用值 x 和 y，多次调用 `x.equals(y)` 始终返回 true 或始终返回 false ， 前提是对象中的信息没有被修改。
- 对于任何非空引用值  x ， `x.equals(null)` 都应返回 false。

对于任何非空引用值 x 和 y，当且仅当 x 和 y  *引用同一个对象* 时，此方法才返回 true （x == y 具有值 true）

#### Tips from API : 当此方法被重写时，通常有必要重写 hashCode 方法，以维护 hashCode 方法的常规协定，该协定声明相等对象必须具有相等的哈希码。


- ### `hashCode()`
```java
public int hashCode()
    返回该对象的哈希码值。支持此方法是为了提高哈希表的性能。
    返回：
        此对象的一个哈希码值。
```

- 在 Java 应用程序执行期间，在对同一对象多次调用 hashCode 方法时，必须一致地返回相同的整数，前提是将对象进行 equals 比较时所用的信息没有被修改
- 从某一应用程序的一次执行到同一应用程序的另一次执行，该整数无需保持一致。
- 如果根据 `equals(Object)` 方法，两个对象是相等的，那么对这两个对象中的每个对象调用 `hashCode()` 方法都必须生成相同的整数结果。
- 如果根据 `equals(java.lang.Object)` 方法，两个对象不相等，则这两个中的任一对象调用 `hashCode()` 方法不要求一定生成不同的整数结果。

#### Tips from API : 为不相等的对象生成不同整数结果可以提高哈希表的性能


- ### `toString()`
```java
public String toString()
返回：
该对象的字符串表示形式。
```
- 通常，`toString` 方法会返回一个 “以文本方式表示” 此对象的字符串。结果应是一个简明但易于读懂的信息表达式
- Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于：
- `getClass().getName() + '@' + Integer.toHexString(hashCode())`

#### Tips from API : 建议所有子类都重写此方法。

- ### `clone()`
```java
protected Object clone() throws CloneNotSupportedException
    返回：
        此实例的一个副本。
    抛出：
        CloneNotSupportedException - 如果对象的类不支持 Cloneable 接口，则重写 clone 方法的子类也会抛出此异常，以指示无法复制某个实例
```

- 创建并返回此对象的一个副本。“副本”的准确含义可能依赖于对象的类。
- 一般情况下： `x.clone().equals(x)`为 true，但这并非必须要满足的要求。
- 按照惯例，返回的对象应该通过调用 `super.clone` 获得。如果一个类及其所有的超类（Object 除外）都遵守此约定，则 `x.clone().getClass() == x.getClass()`
- 此方法返回的对象应该独立于该正在被复制的对象。要获得此独立性，在 `super.clone` 返回对象之前，有必要对该对象的一个或多个字段进行修改。这通常意味着要复制内部“深层结构”的所有可变对象，并使用对副本的引用替换对这些对象的引用。
- 如果一个类只包含基本字段或对不变对象的引用，那么通常不需要修改 super.clone 返回的对象中的字段。

- 如果此对象的类不能实现接口 Cloneable，则会抛出 CloneNotSupportedException

简言之 ： `clone( )` 有深拷贝和浅拷贝两种
- 浅拷贝： B 为 A 的克隆对象， A 中含有其他对象的引用 （ 例如自定义类或者 String ）， A 与 B 作为两个不同的引用 ( 指针 )， 指向了同一块内存区域 。即称 A 与 B 是浅拷贝
- 深拷贝： B 为 A 的克隆对象：
  + A 中包含对其他对象的引用（ 例如自定义类或者 String ）， 需要对引用链的每个层级上的对象都重写 `clone( )` 方法来做显示的调用
  + A 中不包含其他对象的调用  `clone()`方法将分配与原对象相同大小的内存，然后再使用原对象中各个域，填充新对象对应的域

#### Tips from API : Object 类本身不实现接口 Cloneable，所以在类为 Object 的对象上调用 clone 方法将会导致在运行时抛出异常 。另 。clone()方法默认使用浅拷贝的方式

- ### `getClass()`
```java
public final Class<?> getClass()
    返回：
        表示此对象运行时类的 Class 对象。返回的 Class 对象是由所表示类的 static synchronized 方法锁定的对象。
```

实际结果类型是 Class<? extends |X|>，其中 |X| 表示清除表达式中的静态类型，该表达式调用 getClass。 例如，以下代码片段中不需要强制转换：

```java
Number n = 0; 
Class<? extends Number> c = n.getClass();
```
java 中同样能够获得类名的方法除了 `getclass()` 之外还有 `class()` 。两者代码及对比如下：
```java
public  class Demo {
    public static void main(String[] args) {
    Demo demo = new Demo();
        System.out.println(demo.getClass());
        System.out.println(Demo.class);
    }
}

```

`getclass()` 方法用到了 java 中的反射，即在运行时确定类型，随着对象的指向进行动态的类型确定。且是一个类的实例拥有的方法。

上述代码将变量指定为 `Demo` 类型 故调用 `getclass()` 方法是能够输出 `Demo` 累的类名 如下代码对比

```java
class A extends Demo{}

public  class Demo{
    public static void main(String[] args) {
    Demo demo = new Demo();
        System.out.println(demo.getClass());
    A a = new A();
        demo = a ;
        System.out.println(demo.getClass());
    }
```
我们在代码运行时动态地改变对象的指向，那么此时输出就与之前大不一样了

而 `class()` 方法是一个类所具有的方法 （ 通过类名调用而非类实例 ） 即在代码编译时就确定了类的类型

- ### `finalize()`

```java
protected void finalize() throws Throwable
    子类可重写 finalize 方法，以配置系统资源或执行其他清除
    抛出：
        Throwable - 此方法抛出的 Exception
```
当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。

- 当 Java 虚拟机已确定尚未终止的任何线程无法再通过任何方法访问此对象时，将调用此方法，除非由于准备终止的其他某个对象或类的终结操作执行了某个操作。
- `finalize()`  方法可以采取任何操作，其中包括再次使此对象对其他线程可用；
- `finalize()` 的主要目的是在不可撤消地丢弃对象之前执行清除操作。

简言之： 不建议用 `finalize()` 方法完成“非内存资源”的清理工作，但建议用于：

1. 清理本地对象 ( 通过JNI创建的对象 ) ；
2. 作为确保某些非内存资源 ( 如Socket、文件等 ) 释放的一个补充：在finalize方法中显式调用其他资源释放方法

#### Tips from API :  Java语言规范并不保证 `finalize()` 方法会被及时地执行、而且根本不会保证它们会被执行。`finalize()` 方法可能会带来性能问题。因为JVM通常在单独的低优先级线程中完成finalize的执行。 

- ### `notify()`
```java
    public final void notify()
    抛出：
        IllegalMonitorStateException - 如果当前线程不是此对象监视器的所有者
```

唤醒在此对象监视器上等待的单个线程。如果所有线程都在此对象上等待，则会选择唤醒其中一个线程。

选择是任意性的，并在对实现做出决定时发生。线程通过调用其中一个 wait 方法，在对象的监视器上等待。

- ### `notifyAll()`
```java
    public final void notifyAll()
    唤醒在此对象监视器上等待的所有线程。线程通过调用其中一个 wait 方法，在对象的监视器上等待。
    抛出：
        IllegalMonitorStateException - 如果当前线程不是此对象监视器的所有者。
```

- ### `wait()`
```java
public final void wait() throws InterruptedException
    在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。
    抛出：
        IllegalMonitorStateException - 如果当前线程不是此对象监视器的所有者。
        InterruptedException - 如果在当前线程等待通知之前或者正在等待通知时，任何线程中断了当前线程。在抛出此异常时，当前线程的中断状态 被清除。
```
当前线程必须拥有此对象监视器。

该线程发布对此监视器的所有权并等待，直到其他线程通过调用 notify 方法，或 notifyAll 方法通知在此对象的监视器上等待的线程醒来。然后该线程将等到重新获得对监视器的所有权后才能继续执行