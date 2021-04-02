## 面向对象

### Java三大特性：

#### 封装

​		封装就是把对象的属性和操作（或者服务）结合为一个整体，并尽可能的隐藏对象的内部实现细节。

#### 继承

​		继承是面向对象最显著的一个特性。继承是从已有的类中派生出新的类，新的类能获得已有类的属性和行为，并能扩展新的能力。

#### 多态

​		对象在不同时刻表现出来的不同状态。一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。

​		方法的重写Overrid和重载Overload是Java多态性的不同表现。重写是父类与子类之间多态性的一种表现，重载是一个类中多态性的一种表现。

### 五大基本原则

-   **单一职责原则**

    类的功能要单一，不能太杂。

-   **开放封闭原则**

    一个模块应该是对扩展开放，对修改封闭。可以继承它，添加接口扩展功能，而不是修改原有代码。

-   **里氏替换原则**

    子类可以替换父类出现在父类能够出现的任何地方。父类可以出现的地方，子类都可以出现。

-   **依赖倒置原则**

     依赖于抽象而不依赖于具体，我们尽量使用接口来规范依赖。比如你要出国要说你是中国人而不能说你是那个村子的人，中国人是抽象的，下面有具体的县、村子，要依赖的是中国人而不是哪个村的人。

-   **接口隔离原则**

    每一个接口的职责单一明确，所以当我们要实现一个复杂的类时，我们尽量去实现多个接口，而不要把多个接口的功能放到一个接口

### 设计模式

#### 单例模式

**意图**：保证一个类仅有一个实例，并提供一个访问它的全局访问点。

**解决**：一个全局使用的类频繁的创建与销毁。

**关键代码**：构造方法私有化。

##### 懒汉式（需要时才初始化）

```java
public class Singleton2 {

    private static Singleton2 instance;

    private Singleton2() {}

    public static synchronized Singleton2 getInstance() {
        if (instance == null) {
            instance = new Singleton2();
        }

        return instance;
    }
}
```

##### 饿汉式（类加载时初始化）

一般情况下建议使用饿汉式。

```java
public class Singleton3 {

    private static Singleton3 instance = new Singleton3();

    private Singleton3() {}

    public static Singleton3 getInstance() {
        return instance;
    }
} 
```

##### 双重校验锁

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码，提升效率
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

#### 工厂模式

工厂模式违背了

##### 简单工厂模式

一个工厂，一个方法，客户向工厂传递类型来指定工厂要创建的对象

![工厂模式的 UML 图](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/简单工厂模式.png)

工厂类代码：

```java
public class PhoneFactory {
    public Phone makePhone(String phoneType) {
        if(phoneType.equalsIgnoreCase("MiPhone")){
            return new MiPhone();
        }
        else if(phoneType.equalsIgnoreCase("iPhone")) {
            return new IPhone();
        }
        return null;
    }
}
```

##### 工厂方法模式

##### 抽象工厂模式

抽象工厂是围绕一个超级工厂创建其他工厂。

### 抽象类和接口

接口和抽象类都不能实例化

-   **方法**：
    -   接口的方法都是public的，都是抽象方法(1.8以前)，jdk 8 的时候接⼝可以有默认⽅法和静态⽅法功能；
    -   抽象类的方法可以是public、protected、default、private，不一定都是抽象方法，抽象方法的修饰符只能为public或者protected，默认为public。
-   **成员变量**：接口的成员变量是public static final的；抽象类不一定。
-   实现接口的关键字为implements，继承抽象类的关键字为extends。一个类可以实现多个接口，但一个类只能继承一个抽象类。

### 访问控制修饰符

|           | 类内部 | 本包 | 子类 | 外部包 |
| --------- | ------ | ---- | ---- | ------ |
| public    | √      | √    | √    | √      |
| protected | √      | √    | √    | ×      |
| default   | √      | √    | ×    | ×      |
| private   | √      | ×    | ×    | ×      |

### 4种内部类

#### 成员内部类

可以直接使用外部类的所有成员和方法，即使是private的，成员内部类不能有静态成员和方法，创建内部类对象要先创建外部类对象`outer.inner obj = outerobj.new inner();`

#### 局部内部类

定义在一个方法或者一个作用域里面的类，局部内部类只能访问方法内的内容。

局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。

#### 静态内部类

比成员内部类多了static修饰，静态内部类是不需要依赖于外部类的。不能访问外部类的非static成员和方法。

和成员内部类一样，不会因为外部类的加载而加载。

#### 匿名内部类

匿名内部类必须继承一个类或者实现一个接口，没有名字所以只能使用一次，通常用来简化代码。

## 8种基本数据数据类型

| 基本类型 | 位数 | 字节 | 默认值  |
| -------- | ---- | ---- | ------- |
| int      | 32   | 4    | 0       |
| short    | 16   | 2    | 0       |
| long     | 64   | 8    | 0L      |
| byte     | 8    | 1    | 0       |
| char     | 16   | 2    | 'u0000' |
| float    | 32   | 4    | 0f      |
| double   | 64   | 8    | 0d      |
| boolean  | 1    |      | false   |

### 数值溢出

在整型中，每个类型都有一定的表示范围，如果程序计算过程中数字超出了这个类型的表示范围，也不会报错。比如：`Integer.MAX_VALUE + 1`会变成`Integer.MIN_VALUE`，就好像一个环。

### 基本数据类型的好处

对象是保存在堆的，本身就比较耗资源，比如我们经常使用到的int类型，如果每次使用一个数字都要new一个对象的话，就会比较笨重也耗时。Java提供了基本数据类型，这种数据的变量就不需要使用new创建对象，而是直接在栈内存中存储，比较高效。

### 为什么要使用包装类型

基本数据类型是高效，但是Java是面向对象的语言，很多地方都需要使用对象而不是基本数据类型。因为集合要求的元素是Object类，所以在集合中无法将int等基本数据类型放进去。

为了使基本数据类型也具有对象的特征，出现了包装类，将它们包装起来，让他们有对象的性质，赋予他们对象的属性和方法，丰富了基本数据的操作。

### 自动拆装箱带来的问题

-   包装类型的数据比较大小不能用`==`，在包装类型常量池范围内的数据可以直接用`==`，之外的数据要用equals
-   自动拆箱时，如果是包装类型是null的话，会抛出NPE
-   如果一个for循环中有大量拆装箱操作，会浪费很多资源

### 自动装箱与拆箱

装箱就是把基本数据类型转换成包装类型的过程，打包装；拆箱就是把包装类型转换成基本数据类型的过程，拆包装。

装箱过程是通过调用包装器的valueOf方法实现的，而拆箱过程是通过调用包装器的 xxxValue方法实现的

**注意：Integer、Short、Byte、Character、Long这几个类的valueOf方法的实现是类似的。**

​			**Double、Float的valueOf方法的实现是类似的。**

### 基本数据类型的包装类和常量池

Java 基本类型的包装类的大部分都实现了常量池技术。即 Byte,Short,Integer,Long,Character,Boolean；前面 4 种包装类默认创建了数值[-128，127] 的相应类型的缓存数据，Character 创建了数值在[0,127]范围的缓存数据，Boolean 直接返回 True Or False。如果超出对应范围仍然会去创建新的对象。

*为啥把缓存设置为[-128，127]区间？性能和资源之间的权衡。*

**注意：当 "=="运算符的两个操作数都是 包装器类型的引用，则是比较指向的是否是同一个对象，而如果其中有一个操作数是表达式（即包含算术运算）则比较的是数值（即会触发自动拆箱的过程）。另外，对于包装器类型，equals方法并不会进行类型转换。**

## 成员变量、局部变量、静态变量的区别

|          | 成员变量       | 局部变量                     | 静态变量           |
| -------- | -------------- | ---------------------------- | ------------------ |
| 定义位置 | 在类中，方法外 | 方法中或方法的形式参数       | 在类中，方法外     |
| 初始化值 | 有默认初始化值 | 没有，先定义，赋值后才能使用 | 有默认初始化值     |
| 调用方式 | 实例对象调用   |                              | 类名调用，对象调用 |
| 存储位置 | 堆中           | 栈中                         | 方法区             |
| 生命周期 | 与对象共存亡   | 与方法共存亡                 | 与类共存亡         |

特殊情况：

被final修饰的成员变量和静态变量没有初始化值必须显式赋值

## **==与 equals 的区别**

对于基本类型来说，== 比较的是值是否相等；

对于引用类型来说，== 比较的是两个引用是否指向同一个对象地址（两者在内存中存放的地址（堆内存地址）是否指向同一个地方）；

对于引用类型（包括包装类型）来说，`equals`如果没有被重写，`Object`的`equals`方法默认是== 比较；如果`equals()`方法被重写（例如 String），则比较的是地址里的内容。

## String为什么设计成不可变的？

1.  字符串常量池的需要

    如果字符串是可变的，某一个字符串变量改变了其值，那么其指向的变量的值也会改变，String pool将不能够实现！

2.  允许String对象缓存HashCode

    String对象的HashCode被频繁使用，字符串不变性保证了HashCode的唯一性，于是在创建对象时其hashcode就可以放心的缓存了，不需要重新计算。（String类中定义了一个成员变量缓存HashCode）

3.  安全性

    Java中String常常作为参数使用，例如 网络连接地址URL,文件路径path,还有反射机制所需要的String参数，如果String是可变的，将会出现各种安全隐患

## 方法

#### Java只有值传递

Java 程序设计语言总是采用按值调用（**方法接收的是调用者提供的值**）。也就是说，方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。

### 重载和重写

| 区别点     | 重载方法 | 重写方法                                                     |
| ---------- | -------- | ------------------------------------------------------------ |
| 发生范围   | 同一个类 | 子类                                                         |
| 参数列表   | 必须修改 | 一定不能修改                                                 |
| 返回类型   | 可修改   | 子类方法返回值类型应比父类方法返回值类型更小或相等           |
| 异常       | 可修改   | 子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等； |
| 访问修饰符 | 可修改   | 一定不能做更严格的限制（可以降低限制）                       |
| 发生阶段   | 编译期   | 运行期                                                       |

### 重写（Override）

重写是子类对父类的允许访问的方法的实现过程进行重新编写

**方法的重写要遵循“两同两小一大”**

-   “两同”即方法名相同、形参列表相同；
-   “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
-   “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。

##### **重写的规则**

-   参数列表与被重写方法的参数列表必须完全相同。
-   返回类型与被重写方法的返回类型可以不相同，但是必须是父类返回值的派生类（java5 及更早版本返回类型要一样，java7 及更高版本可以不同）。
-   访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为 public，那么在子类中重写该方法就不能声明为 protected。
-   父类的成员方法只能被它的子类重写。
-   声明为 final 的方法不能被重写。
-   声明为 static 的方法不能被重写，但是能够被再次声明。
-   子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为 private 和 final 的方法。
-   子类和父类不在同一个包中，那么子类只能够重写父类的声明为 public 和 protected 的非 final 方法。
-   重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
-   构造方法不能被重写。
-   如果不能继承一个类，则不能重写该类的方法。

#### 重载（Overload）

重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

##### 重载的规则

-   被重载的方法必须改变参数列表(参数个数或类型不一样)；
-   被重载的方法可以改变返回类型；
-   被重载的方法可以改变访问修饰符；
-   被重载的方法可以声明新的或更广的检查异常；
-   方法能够在同一个类中或者在一个子类中被重载。
-   无法以返回值类型作为重载函数的区分标准。

## Static关键字

### 四种使用场景

1.  修饰成员变量和成员方法
2.  静态代码块
3.  修饰类（只能修饰内部类）
4.  静态导包（用来导入类中的静态资源，1.5之后），import static

### 静态代码块

静态代码块定义在类中方法之外，静态代码块在非静态代码块之前执行（静态代码块->非静态代码块->构造方法）。不管创建多少个对象，静态代码块只执行一次。

一个类中可以有多个静态代码块，位置可以随便放，他不在任何方法体内，JVM加载类的时候会执行这些静态的代码块，如果静态代码块有多个，JVM将按照它们在类中出现的先后顺序依次执行，每个代码块只会执行一次。

**静态代码块对于定义在它之后的静态变量，可以赋值，但是不能访问，在它之前的静态变量可以赋值也可以访问。**

![静态代码块赋值静态变量](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/静态代码块赋值静态变量.jpg)

### 静态内部类

静态内部类和非静态内部类最大的区别在于，非静态内部类在编译完成之后会隐含的保存一个引用，这个引用是指向创建它的外围类，但是静态内部类没有。这个引用就意味着：

1.  静态内部类的创建不需要外围类的创建。
2.  静态内部类不能使用任何外围类的非static成员变量和方法。

#### 静态内部类的加载时机

静态内部类和非静态内部类一样，都不会因为外部内的加载而加载，同时静态内部类的加载不需要依附外部类，在使用时才加载，不过在加载静态内部类的过程中也会加载外部类

Example（静态内部类实现单例模式）

```java
public class Singleton {

    //声明为 private 避免调用默认构造方法创建对象
    private Singleton() {
    }

    //声明为 private 表明静态内部该类只能在该 Singleton 类中被访问
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getUniqueInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

*当 Singleton 类加载时，静态内部类 SingletonHolder 没有被加载进内存。只有当调用 `getUniqueInstance() `方法从而触发 `SingletonHolder.INSTANCE` 时 SingletonHolder 才会被加载，此时初始化 INSTANCE 实例，并且 JVM 能确保 INSTANCE 只被实例化一次。*

*这种方式不仅具有延迟初始化的好处，而且由 JVM 提供了对线程安全的支持。*

### static{}与{}、构造方法

相同点：都是在JVM加载类时且在构造方法之前执行，在类中可以定义多个，定义多个的时候按定义顺序执行

不同点：①静态代码块在非静态代码块之前执行。②静态代码块只在第一次加载类时执行，之后不再执行，而非静态代码块在每次创建对象就执行一次。③非静态代码块可以在方法体中定义，静态代码块不行

Example：

```java
public class Test {
    public Test() {
        System.out.print("默认构造方法！--");
    }

    //非静态代码块
    {
        System.out.print("非静态代码块！--");
    }

    //静态代码块
    static {
        System.out.print("静态代码块！--");
    }

    private static void test() {
        System.out.print("静态方法中的内容! --");
        {
            System.out.print("静态方法中的代码块！--");
        }

    }

    public static void main(String[] args) {
        Test test = new Test();
        Test.test();//静态代码块！--静态方法中的内容! --静态方法中的代码块！--
    }
}
```

上诉代码输出：

```
静态代码块！--非静态代码块！--默认构造方法！--静态方法中的内容! --静态方法中的代码块！--
```

当只执行 `Test.test();` 时输出：

```
静态代码块！--静态方法中的内容! --静态方法中的代码块！--
```

当只执行 `Test test = new Test();` 时输出：

```
静态代码块！--非静态代码块！--默认构造方法！--
```

非静态代码块与构造函数的区别是： 非静态代码块是给所有对象进行统一初始化，而构造函数是给对应的对象初始化，因为构造函数是可以多个的，运行哪个构造函数就会建立什么样的对象，但无论建立哪个对象，都会先执行相同的构造代码块。也就是说，非静态代码块中定义的是不同对象共性的初始化内容。

### Java类加载顺序

1.  无父类情况下，加载顺序为：

    静态成员变量、静态代码块 ---》 成员变量、非静态代码块 ---》 构造方法。

2.  有父类的情况下，加载顺序为：

    父类静态成员变量、父类静态代码块 ---》 子类静态成员变量、子类静态代码块 ---》 

    父类成员变量、父类非静态代码块 ---》 父类构造方法 ---》 

    子类成员变量、子类非静态代码块 ---》 子类构造方法

*变量和代码块的加载顺序和在代码中的先后顺序有关*

## 反射

Java反射机制是在运行状态中，对于任何一个类，都能知道这个类的所有属性和方法；对于任意一个对象，都能调用它的任意一个方法和属性。动态获取信息以及动态调用对象的方法。

### 获取Class对象

1.  知道具体类的情况下：

    ```java
    Class alunbarClass = TargetObject.class;
    ```

    这种方式不会进行初始化

2.  通过 `Class.forName()`传入类的路径获取：

    ```java
    Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
    ```

    **Class.forName(className)默认是需要初始化。**

    一旦初始化，static参数会被再次初始化，也会触发目标对象的 static块代码执行。

3.  通过对象实例`instance.getClass()`获取：

    ```java
    Employee e = new Employee();
    Class alunbarClass2 = e.getClass();
    ```

4.  通过类加载器`xxxClassLoader.loadClass()`传入类路径获取：

    ```java
    class clazz = ClassLoader.LoadClass("cn.javaguide.TargetObject");
    ```

    通过类加载器获取Class对象不会进行初始化，意味着不进行包括初始化等一些列步骤，静态块和静态对象不会得到执行

### 获取当前方法名

```java
Thread.currentThread().getStackTrace()[1].getMethodName(); // 方法名
Thread.currentThread().getStackTrace()[1].getClassName(); // 类名
```

### 优缺点

-   优点：运行期类型的判断，动态加载类，提高代码灵活度

-   缺点：性能，反射相当于一系列解释操作，通知 JVM 要做的事情，性能比直接的 java 代码要慢很多。

    安全问题，让我们可以动态操作改变类的属性同时也增加了类的安全隐患。

## IO

为了保证操作系统的稳定性和安全性，一个进程的地址空间划分为 **用户空间（User space）** 和 **内核空间（Kernel space ）**。

从应用程序的视角来看的话，我们的应用程序对操作系统的内核发起 IO 调用（系统调用），操作系统负责的内核执行具体的 IO 操作。也就是说，我们的应用程序实际上只是发起了 IO 操作的调用而已，具体 IO 的执行是由操作系统的内核来完成的。

当应用程序发起 I/O 调用后，会经历两个步骤：

1.  **内核等待 I/O 设备准备好数据**
2.  **内核将数据从内核空间拷贝到用户空间。**

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/BIO、NIO、AIO.jpg)

### BIO (Blocking I/O)

BIO属于同步阻塞IO模型，应用程序发起 read 调用后，会一直阻塞，直到在内核把数据拷贝到用户空间。

![图源：《深入拆解Tomcat & Jetty》](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/同步阻塞IO模型.jpg)

### NIO (Non-blocking/New I/O)

![图源：《深入拆解Tomcat & Jetty》](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/同步非阻塞IO模型.jpg)

同步非阻塞 IO 模型中，应用程序会一直发起 read 调用，等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。

相比于同步阻塞 IO 模型，同步非阻塞 IO 模型确实有了很大改进。通过轮询操作，避免了一直阻塞。

但是，这种 IO 模型同样存在问题：**应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。**

这个时候，**I/O 多路复用模型** 就上场了。

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/IO多路复用模型.jpg)

### AIO (Asynchronous I/O)

异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/异步IO模型.jpg)

## 集合

Java集合接口分成两组

### Collection集合结构：

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/Collection集合.jpg)

### Map集合结构：

![Java Map Hierarchy](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/Map集合.jpg)

### 说说List、Set、Map三者的区别

-   `List`(对付顺序的好帮手)： 存储的元素是有序的、可重复的。
-   `Set`(注重独一无二的性质)：存储的元素师无序的、不可重复。
-   `Map`(用key来搜索的专家)：使用键值对（key-value）存储元素，key是无序的不可重复，value是无序的可重复，每个键最多映射到一个值。

### List

-   `Arraylist`： `Object[]`数组，适用于频繁的查找工作，线程不安全 
-   `Vector`：`Object[]`数组，线程安全的
-   `LinkedList`： 双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)

#### ArrayList和LinkedList的区别

1.  ArrayList是基于动态数组，LinkedList是基于双向链表的。
2.  **随机访问**：ArrayList 比 LinkedList 在随机访问的时候效率要高，因为 LinkedList 是线性的数据存储方式，所以需要移动指针从前往后依次查找。
3.  **插入和删除**：在尾部插入和删除，两者效率差不多；在指定位置插入和删除，ArrayList中指定位置后面的元素都要移位操作，LinkedList只需要把前一个元素的后继结点指向要插入的元素，
4.  **内存空间占用**：LinkedList 比 ArrayList 更占内存，因为 LinkedList 的节点除了存储数据，还存储了两个引用，一个指向前一个元素，一个指向后一个元素。

#### ArrayList的扩容机制

-   在JDK1.8中，如果通过无参构造创建，初始数组容量为0，当添加第一个元素的时候，才真正分配容量为默认容量10。
-   当数组容量不足时(最低容量要求(list的size+1)大于数组的长度时)，开始扩容：
    1.  先判断按照1.5倍（位运算:`1+(1>>1)`）扩容是否能满足最低容量要求，如果可以，则按1.5倍扩容，否则扩容到最低容量要求
    2.  新的数组长度大于`Integer.MAX_VALUE - 8`的话，数组扩容到``Integer.MAX_VALUE`
    3.  调用`Arrays.copyOf()`返回新的数组

![image-20210312224227778](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/ArrayList扩容.png)

### Set

-   `HashSet`：无序、唯一，基于HashMap实现，底层采用HashMap来保存元素。
-   `LinkedHashSet`：是HashSet的子类，其内部是通过LinkedHashMap实现的。
-   `TreeSet`：有序、唯一，红黑树（自平衡的排序二叉树）。

### Map

-   `HashMap`： JDK1.8 之前 `HashMap` 由数组+链表组成的，数组是 `HashMap` 的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间
-   `LinkedHashMap`： `LinkedHashMap` 继承自 `HashMap`，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，`LinkedHashMap` 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。详细可以查看：[《LinkedHashMap 源码详细分析（JDK1.8）》](https://www.imooc.com/article/22931)
-   `Hashtable`： 数组+链表组成的，数组是 `HashMap` 的主体，链表则是主要为了解决哈希冲突而存在的
-   `TreeMap`： 红黑树（自平衡的排序二叉树）

#### HashMap的底层实现

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashMap结构图.jpg)

##### 重要参数

-   **容量(Capacity)**：必须为2的幂，默认1 << 4
-   **负载因子(Load factor)**：默认0.75f，HashMap中元素个数超过了容量的3/4会扩容。
-   **阈值(threshold)**：下一个要调整大小的阈值（容量*负载系数），当HashMap的size到达这个threshold这个阈值时会扩容成当前容量的2倍

##### 创建HashMap

1.  创建时不指定初始容量，默认初始化大小为16，之后每次扩容，容量变为原来的2倍

2.  创建时指定初始容量，`tableSizeFor(initialCapacity)`这个方法会把容量扩充为 `大于等于initialCapacity的最小的2的幂` (initialCapacity最大1<<30)，然后把这个capacity赋值给了threshold（当HashMap的size到达这个threshold这个阈值时会扩容），**在构造方法中，并没有对table这个成员变量进行初始化，table的初始化被推迟到了put方法中，在put方法中会对threshold重新计算**

    ```java
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: ");
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }
    
    /**
     * Returns a power of two size for the given target capacity.
     * 找到大于等于initialCapacity的最小的2的幂（initialCapacity如果就是2的幂，则返回的还是这个数）
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
    ```

    ![这里写图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashMap的初始化容量为什么是2的幂.png)

##### hash方法的实现

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

**key的hash值高16位不变，低16位与高16位异或作为key的最终hash值**。（h >>> 16，表示无符号右移16位，高位补0，任何数跟0异或都是其本身，因此key的hash值高16位不变。）

![这里写图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashMap的hash方法.png)

##### 计算下标index

```java
index = （n-1） & hash; // n是resize()之后table的长度
```

![hash](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashMap计算下标.png)

##### put方法的实现

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashMap的put(key,val)方法.png)

大致的过程为：

1.  判断键值对数组tab[]是否为空或为null，否则以默认大小resize()，初始化tab[16]；
2.  对key的hashCode()做hash，然后再计算index;
3.  如果没碰撞直接放到bucket里；
4.  如果碰撞了：
    1.  key的hash相等，key也相等，则替换val
    2.  如果key的hash相等，key不相等，同时不是TreeNode数据对象，遍历链表找到对应元素，有就覆盖无则新建	
5.  如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD=8)，**①hashMap的长度大于等于64就把链表转换成红黑树；②长度小于64就只是resize()将HashMap扩容2倍，不把链表转换成红黑树**
6.  如果节点已经存在就替换old value(保证key的唯一性)
7.  插入之后，如果buckets满了(超过load factor*current capacity)，就要resize，每次扩容操作，新的数组大小将是原始的数组长度的两倍。

```java
public V put(K key, V value) {
    // 对key的hashCode()做hash
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 判断数组是否为空，长度是否为0，是则进行扩容数组初始化，默认大小
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // 通过hash算法找到数组下标得到数组元素，为空则新建
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        // 表示有冲突,开始处理冲突
        Node<K,V> e; K k;
        // 找到数组元素，hash相等同时key相等，则直接覆盖
        if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
            // 该数组元素在链表长度>8后形成红黑树结构的对象
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            // 该数组元素hash相等，key不等，同时链表长度<8.进行遍历寻找元素，有就覆盖无则新建
            for (int binCount = 0; ; ++binCount) {
                // 指针为空就挂在后面
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    //如果冲突的节点数已经达到8个，看是否需要改变冲突节点的存储结构
                    //treeifyBin首先判断当前hashMap的长度，如果不足64，只进行resize，扩容table
                    //如果达到64，那么将冲突的存储结构为红黑树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        // 链表长度>=8 结构转为 红黑树
                        treeifyBin(tab, hash);
                    break;
                }
                // 如果有相同的key值就结束遍历，后面会覆盖
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        // 就是链表上有相同的key值
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue; //返回存在的Value值
        }
        // 碰撞结束
    }
    ++modCount;
    // 如果当前大小大于阈值(容量*负载因子)
    if (++size > threshold)
        resize(); // 扩容两倍
    afterNodeInsertion(evict);
    return null;
}
```

##### get方法的实现

1.  根据key的计算hash和下标
2.  若与数组中对应下标元素的第一个节点的key和hash相等，则直接返回
3.  若不相等，并且是存储结构是红黑树的话，就在树中查找，如果是链表，则遍历链表查找

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        // 直接命中
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        // 未命中
        if ((e = first.next) != null) {
            // 在树中get
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 在链表中get
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

##### 为什么要扩容？

和哈希碰撞有关，如果一个 HashMap 中冲突太高，元素就会多的放到同一个位置，那么数组的链表就会退化为链表，这时候查询速度会大大降低，在合适的时候扩大数组容量，减少碰撞的概率。

##### 为什么负载因子默认是0.75f？

一般来说，默认的负载因子 (0.75) 在时间和空间成本之间提供了很好的权衡。更高的值减少了空间开销，但增加了查找成本(反映在 HashMap 类的大多数操作中，包括 get 和 put)，**负载因子不能太大，不然会导致大量的哈希冲突，也不能太小，那样会浪费空间。**。

试想一下，如果我们把负载因子设置成 1，容量使用默认初始值 16，那么表示一个 HashMap 需要在 "满了" 之后才会进行扩容。那么在 HashMap 中，最好的情况是这 16 个元素通过 hash 算法之后分别落到了 16 个不同的桶中，否则就必然发生哈希碰撞。而且随着元素越多，哈希碰撞的概率越大，查找速度也会越低。

#### ConCurrentHashMap

采用Node+CAS+synchronized来保证线程安全：

synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突的话，不加锁用CAS技术操作。

### HashMap和Hashtable的区别

1.  **线程是否安全：**`HashMap`是非线程安全的，`Hashtable`是线程安全的，因为`Hashtable`内部方法基本都被`synchronized`修饰。（如果你要保证线程安全的话就使用 `ConcurrentHashMap` 吧！）；

2.  **效率：**因为线程安全原因，`HashMap` 要比 `HashTable` 效率高一点。另外，`HashTable` 基本被淘汰，不要在代码中使用它；

3.  **对 Null key 和 Null value 的支持：** `HashMap` 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；HashTable 不允许有 null 键和 null 值，否则会抛出 `NullPointerException`。

4.  **初始容量大小和每次扩充容量大小的不同：**

    ① 创建时如果不指定容量初始值，`Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。`HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。

    ② 创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 `HashMap` 会将其扩充为 2 的幂次方大小（`HashMap` 中的`tableSizeFor()`方法保证，下面给出了源代码）。也就是说 `HashMap` 总是使用 2 的幂作为哈希表的大小,后面会介绍到为什么是 2 的幂次方。

5.  **底层数据结构：** JDK1.8 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

### HashMap和HashSet的区别

| `HashMap`                              | `HashSet`                                                    |
| -------------------------------------- | ------------------------------------------------------------ |
| 实现了 `Map` 接口                      | 实现 `Set` 接口                                              |
| 存储键值对                             | 仅存储对象                                                   |
| 调用 `put()`向 map 中添加元素          | 调用 `add()`方法向 `Set` 中添加元素                          |
| `HashMap` 使用键（Key）计算 `hashcode` | `HashSet` 使用成员对象来计算 `hashcode` 值，对于两个对象来说 `hashcode` 可能相同，所以` equals()`方法用来判断对象的相等性 |

HashSet底层是基于HashMap实现的，元素存放在内部的HashMap的key中，map所有的value是同一个Object对象

![image-20210310111536457](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/HashSet的add方法.jpg)

### Comparable和Comparator的区别

-   `Comparable`接口实际上是出自`java.lang`包，它有一个 `compareTo(Object obj)`方法用来排序
-   `Comparator`接口实际上是出自 java.util 包，它有一个`compare(Object obj1, Object obj2)`方法用来排序
-   `Comparable` 自然排序。（实体类实现Comparable）
-   `Comparator`是定制排序。（无法修改实体类时，直接在调用方创建Comparator对象）

**同时存在时采用 Comparator（定制排序）的规则进行比较。**

对于一些普通的数据类型（比如 String, Integer, Double…），它们默认实现了Comparable 接口，重写 compareTo 方法，我们可以直接使用。

而对于一些自定义类，它们可能在不同情况下需要实现不同的比较策略，我们可以新创建 Comparator 接口对象，重写compare方法实现进行比较。

### HashSet如何检查重复

当把对象加入到`HashSet`时，`HashSet`会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的`hashcode`值比较，如果没有相同的`hashcode`，`HashSet`会假设对象没有重复出现。但是如果发现有相同的`hashcode`值的对象，这时会调用`equals()`方法检查`hashcode`相等的对象是否真的相同，如果两者相同，`HashSet`就不会让加入操作成功。这样就大大减少了 equals 的次数，相应就大大提高了执行速度。

**`hashCode()`与 `equals()` 的相关规定：**

1.  如果两个对象相等，则 `hashcode` 一定也是相同的
2.  两个对象相等,对两个 `equals()` 方法返回 true
3.  两个对象有相同的 `hashcode` 值，它们也不一定是相等的
4.  综上，`equals()` 方法被覆盖过，则 `hashCode()` 方法也必须被覆盖
5.  `hashCode() `的默认行为是对堆上的对象产生独特值。如果没有重写 `hashCode()`，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。

### TreeSet和TreeMap如何实现排序

TreeSet要求存放的**元素必须实现Comparable接口**重写compareTo()方法，当插入元素时会回调该方法比较元素大小。TreeMap 要求存放的键值对映射的key必须实现 Comparable 接口从而根据key对元素进行排序。

## 多线程

### 并发编程的优点

-   提高多核CPU的利用率
-   提高系统并发能力和性能，方便系统业务拆分

### 进程和线程的关系

-   线程是进程划分成的更小的运行单元。

-   基本上各进程是独立的，但是各线程不一定，因为同一进程中的线程有可能会相互影响。
-   线程执行开销小，但不利于资源的管理和保护；而进程相反

### 并发和并行的区别

-   并发：同一时间段，多个任务都在执行（单位时间内不一定同时执行）
-   并行：单位时间内，多个任务同时执行

### 多线程可能带来什么问题？

内存泄露、频繁的上下文切换、死锁

#### 减少上下文切换的方案

-   无锁并发编程：就像是ConcurrentHashMap的分段锁，不同的线程处理不同段的数据，减少线程切换
-   CAS算法：利用Atomic使用CAS算法来更新数据，使用自旋锁减少上下文切换。
-   创建最少的线程：避免创建不需要的线程，比如任务少的时候创建了大量线程，会造成大量的线程处于等待状态。

### 什么是上下文切换？

多线程编程中⼀般线程的个数都⼤于 CPU 核⼼的个数，⽽⼀个 CPU 核⼼在任意时刻只能被⼀个线程使⽤，为了让这些线程都能得到有效执⾏，CPU 采取的策略是为每个线程分配时间⽚并轮转的形式。当⼀个线程的时间⽚⽤完的时候就会重新处于就绪状态让给其他线程使⽤，这个过程就属于⼀次上下⽂切换。

概括来说就是：当前任务在执⾏完 CPU 时间⽚切换到另⼀个任务之前会先保存⾃⼰的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是⼀次上下⽂切换**。

上下⽂切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒⼏⼗上百次的切换中，每次切换都需要纳秒量级的时间。所以，上下⽂切换对系统来说意味着消耗⼤量的CPU 时间，事实上，可能是操作系统中时间消耗最⼤的操作。

Linux 相⽐与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有⼀项就是，其上下⽂切换和模式切换的时间消耗⾮常少。

### 创建线程的4种方式

1.  继承Thread类：定义一个类继承Thread，重写run方法，创建这个类的实例并调用start()方法启动线程。

2.  实现Runable接口：定义一个类实现Runable接口，重写run方法，将这个类的实例作为参数传给Thread类创建Thread类实例并调用start()方法启动线程。

3.  使用Callable和Future创建线程

    ```java
    public static class ThreadA implements Callable<Integer> {
        @Override
        public Integer call() {
            return 100;
        }
    }
    
    public static void main(String[] args) {
        // 方式一
        ThreadA threadA = new ThreadA();
        FutureTask<Integer> futureTask = new FutureTask<>(threadA);
    
        // 方式二
        FutureTask<String> futureTask2 = new FutureTask<>(new Callable<String>() {
            @Override
            public String call() {
                return "hello world";
            }
        });
    
        // 启动线程
        Thread thread = new Thread(futureTask);
        thread.start();
        try {
            Integer result = futureTask.get();
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
    }
    ```

4.  使用Executor框架创建线程池

### 线程的生命周期和状态

![image-20210314213925521](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程6种状态.png)

![image-20210314214116219](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/Java线程状态切换.png)

线程创建之后它将处于 **NEW（新建）** 状态，调用 `start()` 方法后开始运行，等到线程获得了 CPU 时间片（timeslice）后就处于 **RUNNING（运行）** 状态。

当线程执行 `wait()`方法之后，线程进入 **WAITING（等待）** 状态。进入等待状态的线程需要依靠其他线程的通知才能够返回到运行状态，而 **TIME_WAITING(超时等待)** 状态相当于在等待状态的基础上增加了超时限制，比如通过 `sleep（long millis）`方法或 `wait（long millis）`方法可以将 Java 线程置于 TIMED WAITING 状态。当超时时间到达后 Java 线程将会返回到 RUNNABLE 状态。当线程调用同步方法时，在没有获取到锁的情况下，线程将会进入到 **BLOCKED（阻塞）** 状态。线程在执行 Runnable 的`run()`方法之后将会进入到 **TERMINATED（终止）** 状态。

### 线程状态的基本操作

#### Interrupt

中断可以理解为线程的一个标志位，它表示了一个运行中的线程是否被其他线程进行了中断操作。中断好比其他线程对该线程打了一个招呼。其他线程可以调用该线程的interrupt()方法对其进行中断操作。**需要注意的是，当抛出InterruptedException时候，会清除中断标志位，也就是说再调用isInterrupted会返回false。**

| 方法名               | 方法注释                 | 备注                                                         |
| -------------------- | ------------------------ | ------------------------------------------------------------ |
| interrupt()          | 中断该线程对象           | 如果该线程被调用了Object wait/Object wait(long)，或者被调用sleep(long)，join()/join(long)方法时会抛出interruptedException并且中断标志位将会被清除 |
| isinterrupted()      | 测试该线程对象是否被中断 | 中断标志位不会被清除                                         |
| Thread.interrupted() | 测试当前线程是否被中断   | 中断标志位会被清除，第二次调用会返回false                    |

#### Join

join可以看做是线程之间的协作，一个线程的输入可能依赖另一个线程的输出，在线程A中调用线程B的join()方法，A线程会等待B线程执行完再继续执行。

#### yield

调用Thread.yield()方法，它会让当前线程让出CPU，**让出的时间片只会分配给当前线程优先级相同的线程**，但是不代表当前线程不执行了，可能在下次竞争中还是获得了CPU时间片继续执行。

**wait()交出来的时间片其他线程都可以去竞争**

### 线程组

-   Java中使用ThreadGroup来表示线程组，它可以对一批线程进行分类管理。对线程组的控管理，即同时控制线程组里面的这一批线程。

-   用户创建的所有线程都属于指定线程组，如果没有显示指定属于哪个线程组，那么该线程就属于默认线程组（即main线程组）。默认情况下，子线程和父线程处于同一个线程组。new Thread时可以指定。

-   只有在创建线程时才能指定其所在的线程组，线程运行中途不能改变它所属的线程组，也就是说线程一旦指定所在的线程组，就直到该线程结束。
    线程组与线程之间结构类似于树形的结构：

    ![这里写图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程组与线程.png)

### 线程优先级

操作系统采用分时调度运行中的线程，系统会分出一个个时间片，线程会分配到若干个时间片，当前时间片用完后会发生线程调度，等待下次分配。

1.  **记住当线程的优先级没有指定时，所有线程都携带普通优先级**，setPriority(int)方法进行设置。
2.  优先级可以用从1到10的范围指定。10表示最高优先级，1表示最低优先级，5是普通优先级。
3.  ==记住在线程开始方法被调用之前，线程的优先级应该被设定==。

Java中的优先级来说不是特别的可靠，**Java程序中对线程所设置的优先级只是给操作系统一个建议，操作系统不一定会采纳。而真正的调用顺序，是由操作系统的线程调度算法决定的**。

### 锁

#### 乐观 VS 悲观锁

悲观锁，认为自己在使用数据时一定有别的线程来修改数据，所以在获取数据时会先加锁，确保数据不被其他线程修改，synchronized和Lock的实现类都是悲观锁。

乐观锁，认为自己在使用数据时不会有别的线程修改数据，所以不会加锁，只有在更新数据时会判断之前有没别的线程修改了数据，如果这个数据没有被修改，当前线程将自己修改的数据成功写入；如果数据已经被其他线程更新，则根据不同的实现方式执行不同的操作(报错或者重试)。Java中，乐观锁通过使用无锁编程来实现，最常用的是CAS算法。

-   悲观锁适合写操作多的场景，先加锁可以保证写操作是数据正确。
-   乐观锁适合读操作多的场景，不加锁可以使读的性能大幅提升。

#### 自旋锁 VS 适应性自旋锁

阻塞或唤醒一个Java线程需要操作系统切换CPU状态来完成，如果同步代码中的内容过于简单，上下文切换消耗的时间可能比用户执行代码的时间还长。

在很多场景中，同步资源的锁定时间很短，为了这一小段时间去切换线程，线程挂起和恢复的花费可能会让系统得不偿失。这个时候可以让请求锁的线程不放弃CPU时间片，看看持有锁的线程是不是很快就会释放锁。自旋锁就是实现当前线程"稍等一下"，如果在自旋完成之后前面锁定同步资源的线程已经释放锁了，那么当前线程就可以直接获取同步资源，从而避免了上下文切换的开销。

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/自旋锁.png)

**自旋锁（spinlock）**：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。 

**自旋锁的原理也是CAS**

JDK1.6默认开启自旋锁，并引入了适应性自旋锁，就是自旋次数(时间)不固定，如果对于某个锁自旋很少成功获得过，那么以后尝试获得这个锁的时候就不会自旋，直接阻塞线程。

```java
// 自旋锁的例子
public class SpinLock {
    private AtomicReference<Thread> cas = new AtomicReference<Thread>();
    public void lock() {
        Thread current = Thread.currentThread();
        // 利用CAS
        while (!cas.compareAndSet(null, current)) {
            // DO nothing
        }
    }
    public void unlock() {
        Thread current = Thread.currentThread();
        cas.compareAndSet(current, null);
    }
}
```

#### 公平锁 VS 非公平锁

公平锁是指多个线程按照申请锁的顺序来获得锁，线程直接进入一个等待队列，队列中的第一个线程才能获得锁。

-   优点：等待锁的线程不会饿死。
-   缺点：等待队列中除了第一个线程以外的所有线程都阻塞，CPU唤醒线程的开销比非公平锁大。

非公平锁是多个线程直接尝试获取锁，获取不到才会到等待队列的队尾等待。比如此时锁刚好可用，那么这个线程无需阻塞可以直接获取到锁，非公平锁可能出现后申请的线程先获取到锁。

-   优点：可以减少唤起线程的开销，因为线程有几率不用阻塞直接获得锁。
-   缺点：处于等待队列中的线程可能会饿死，或者等很久才获得锁。

synchronized，ReentrantLock默认是非公平锁，可以通过构造方法指定使用公平锁。

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/非公平锁.png)

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/非公平锁.png)

#### 可重入锁 VS 非可重入锁

可重入锁是指，在同一个线程的外层方法获取锁的时候，再进入一个内层方法会自动获取锁(前提是两个方法的锁对象是同一个)，不会因为之前获取了锁没释放而阻塞，可以一定程度上避免死锁。synchronized和ReentrantLock都是可重入锁，NonReentrantLock是非可重入锁。

```java
public class Widget {
    public synchronized void doSomething() {
        System.out.println("方法1执行...");
        doOthers();
    }

    public synchronized void doOthers() {
        System.out.println("方法2执行...");
    }
}
```

#### 独占锁 VS 共享锁

独享锁也叫排他锁，**是指该锁只能被一个线程所持有**，如果线程T对数据A加上排他锁之后，其他线程不能再对A加任何类型的锁，**获得排他锁的线程既可以读取也可以修改数据**。**synchronized和Lock的实现类都是互斥锁**。

共享锁是指该锁可被多个线程持有，如果线程T对数据A加上共享锁，那么其他线程只能对A再加共享锁，不能加排他锁。**获得共享锁的线程只能读数据**，不能修改数据。

独占锁和共享锁是通过AQS实现的。

##### ReentrantReadWriteLock(读写锁)

针对**读多写少**的情况，可以使用读写锁，允许同一个时刻被多个线程访问，但是在写线程访问时，所有读线程和其他写线程会阻塞。

### 线程死锁

#### 什么是线程死锁？

**死锁**是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。

线程死锁描述的是：多个线程互相等待对方的资源释放，线程被⽆限期地阻塞，因此程序不可能正常终⽌。

如下图所示，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对⽅的资源，所以这两个线程就会互相等待⽽进⼊死锁状态。

![线程死锁示意图 ](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/死锁1.png)

死锁例子：

```java
public class DeadLockDemo {
    private static Integer resource1 = 1;//资源 1
    private static Integer resource2 = 2;//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource2) {
                System.out.println(Thread.currentThread() + "get resource2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource1");
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                }
            }
        }, "线程 2").start();
    }
}
```

*线程 A 通过 synchronized (resource1) 获得 resource1 的监视器锁，然后通过`Thread.sleep(1000);`让线程 A 休眠 1s 为的是让线程 B 得到执行然后获取到 resource2 的监视器锁。线程 A 和线程 B 休眠结束了都开始企图请求获取对方的资源，然后这两个线程就会陷入互相等待的状态，这也就产生了死锁。*

#### 死锁的四个条件

1.  互斥条件：该资源任意一个时刻只由一个线程占用。
2.  请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3.  不剥夺条件：线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4.  循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。

#### 避免线程死锁

只要破坏四个条件中的一个就可以了。

1.  **破坏互斥条件** ：这个条件我们没有办法破坏，因为我们用锁本来就是想让他们互斥的（临界资源需要互斥访问）。
2.  **破坏请求与保持条件** ：一次性申请所有的资源。
3.  **破坏不剥夺条件** ：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源
4.  **破坏循环等待条件** ：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

对线程 2 的代码修改成下面这样就不会产生死锁了。

```java
public class DeadLockDemo {
    private static Integer resource1 = 1;//资源 1
    private static Integer resource2 = 2;//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 2").start();
    }
}
```

*线程 1 首先获得到 resource1 的监视器锁,这时候线程 2 就获取不到了在等待。然后线程 1 再去获取 resource2 的监视器锁，可以获取到。然后线程 1 释放了对 resource1、resource2 的监视器锁的占用，线程 2 获取到就可以执行了。这样就破坏了破坏循环等待条件，因此避免了死锁。*

### sleep()和wait()方法的区别？

1.  **sleep()** ⽅法没有释放锁，⽽ **wait()** ⽅法释放了锁
2.  sleep()是Thread线程类的方法，wait()是Object类的方法
3.  **sleep()方法**可以在任何地方使用；**wait()方法**则只能在同步方法或同步块中使用
4.  wait() ⽅法被调⽤后，线程不会⾃动苏醒，需要别的线程调⽤同⼀个对象上的 notify() 或 者 notifyAll() ⽅法。sleep() ⽅法执⾏完成后，线程会⾃动苏醒。或者可以使⽤ wait(long timeout) 超时后线程会⾃动苏醒。

### 为什么我们不能直接调⽤ **run()** ⽅法？

new 一个 Thread，线程进入了新建状态。调用 `start()`方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。 `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。 但是，**直接执行 `run()` 方法，会把 `run()` 方法当成一个 main 线程下的普通方法去执行**，并不会在某个线程中执行它，所以这并不是多线程工作。

**总结： 调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。**

### synchronized

#### 对synchronized关键字的理解

**synchronized** 关键字解决的是多个线程之间访问资源的同步性， **synchronized** 关键字可以保证被它修饰的⽅法或者代码块在任意时刻只能有⼀个线程执⾏。

#### synchronized的使用

1.  **修饰实例方法:** 作用于当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁**

    ```java
    synchronized void method() {
      //业务代码
    }
    ```

2.  **修饰静态方法:** 也就是给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。因为静态成员不属于任何一个实例对象，是类成员（ _static 表明这是该类的一个静态资源，不管 new 了多少个对象，只有一份_）。所以，如果一个线程 A 调用一个实例对象的非静态 `synchronized` 方法，而线程 B 需要调用这个实例对象所属类的静态 `synchronized` 方法，是允许的，不会发生互斥现象，因为访问静态 `synchronized` 方法占用的锁是当前类的锁，而访问非静态 `synchronized` 方法占用的锁是当前实例对象锁。**class锁和实例对象锁不一样**

    ```java
    synchronized static void method() {
    //业务代码
    }
    ```

3.  **修饰代码块** ：指定加锁对象，对给定对象/类加锁。`synchronized(this|object)` 表示进入同步代码库前要获得**给定对象的锁**。`synchronized(类.class)` 表示进入同步代码前要获得 **当前 class 的锁**

    ```java
    synchronized(this) {
      //业务代码
    }
    ```

**注意**：尽量不要使用 `synchronized(String a)` 因为 JVM 中，字符串常量池具有缓存功能！

**双重校验锁实现对象单例（线程安全）**

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码，提升效率
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

另外，需要注意 `uniqueInstance` 采用 `volatile` 关键字修饰也是很有必要。

`uniqueInstance` 采用 `volatile` 关键字修饰也是很有必要的， `uniqueInstance = new Singleton();` 这段代码其实是分为三步执行：

1.  **为 `uniqueInstance` 分配内存空间**
2.  **初始化 `uniqueInstance`**
3.  **将 `uniqueInstance` 指向分配的内存地址**

但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用 `getUniqueInstance`() 后发现 `uniqueInstance` 不为空，因此返回 `uniqueInstance`，但此时 `uniqueInstance` 还未被初始化。

使用 `volatile` 可以禁止 JVM 的**指令重排**，保证在多线程环境下也能正常运行。

#### synchronized的原理

**synchronized 关键字底层原理属于 JVM 层面。**同步语句块和同步方法原理不同，**两者的本质都是对对象监视器 monitor 的获取。**

1.  同步语句块的情况

    synchronized同步语句块使用的是monitorenter和monitorexit指令，monitorenter指向同步代码块的开始位置，monitorexit指向结束位置。

    在执行`monitorenter`时，会尝试获取对象的锁，如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。

    在执行 `monitorexit` 指令后，将锁计数器设为 0，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。

2.  同步方法的情况

    同步方法有`ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

### Java内存模型

在Java虚拟机中，定义了一种内存模型(JMM)来屏蔽各个硬件平台和操作系统的内存访问差异，以实现Java程序在各种平台下都能达到一致的内存访问效果。

JMM主要目标是定义程序中各个变量的访问规则，即在虚拟机中将变量存储到内存中和从内存中取出变量的底层细节。这里的(共享)变量包括了类的成员变量、静态成员变量和构成数组对象的元素，不包括局部变量和方法参数，后者属于线程私有，不会被共享。

JMM规定了所有共享变量存储在主内存中，每个线程有自己的工作内存(相当于CPU高速缓存)，线程的工作内存保存了被线程使用到的变量的主内存副本拷贝，线程对变量的操作(读取、赋值)都是在工作内存完成的，而不是直接在主存中进行读写，不同的线程之间也无法直接访问对方工作内存中的变量。这就可能造成一个线程在主存中修改了一个变量的值，而另外一个线程还继续使用它在工作内存中的变量值的拷贝。

#### 内存间交互操作

Java 内存模型定义了8个操作来完成主内存和工作内存的交互操作

![主内存和工作内存交互操作](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/主内存和工作内存交互操作.png)

**lock(锁定)**：作用于主内存中的变量，把变量标识为一个线程独占的状态。

**unlock(解锁)**：作用于主内存中的变量，把处于锁定状态的变量释放出来，才可以被其他线程锁定。

**read(读)**：作用于主内存中的变量，把主内存的变量的值传输到工作内存，以便后续的load操作。

**load(载入)**：作用于工作内存中的变量，把从工作内存read进来的变量的值放入工作内存中的变量副本。

**use(使用)**：作用于工作内存中的变量，把工作内存中的变量的值传给执行引擎，每当虚拟机遇到一个需要使用变量值的指令时将会执行这个操作。

**assign(赋值)**：作用于工作内存中的变量，它把一个从执行引擎接收到的值赋值给工作内存中的变量，每当虚拟机遇到要给变量赋值的指令时会执行这个操作。

**store(存储)**：作用于工作内存中的变量，它把工作内存中的变量的值传给主内存以便随后的write操作使用。

**write(写)**：作用于主内存中的变量，它把从工作内存中的变量的值放入主内存的变量中。

#### 内存屏障

内存屏障可以阻止编辑器和处理器对指令序列进行重排序

##### JMM内存屏障分为四类

| 屏障类型            | 指令示例                   | 说明                                                         |
| ------------------- | -------------------------- | ------------------------------------------------------------ |
| LoadLoad Barriers   | Load1；LoadLoad；Load2     | 确保Load1的数据的载入先于Load2及所有后续装载指令的装载       |
| StoreStore Barriers | Store1；StoreStore；Store2 | 确保Store1数据对其他处理器可见（刷新到内存）先于Store2及所有后续存储指令的存储 |
| LoadStore Barriers  | Load1；LoadStore；Store2   | 确保Load1的数据的装载先于Store2及所有后续存储指令的存储      |
| StoreLoad Barriers  | Store1；StoreLoad；Load2   | 确保Store1的数据对其他处理器可见（刷新到内存）先于Load2及所有后续的装载指令的装载 |

### volatile

volatile是Java提供的轻量级的同步机制。

1.  保证变量对所有线程的可见性，当一个变量定义为volatile时，一个线程修改了这个变量的值，新值会立即同步到主内存，其他线程使用它要到主内存中读取。

2.  禁止指令重排序优化。在编译时，会在指令序列中插入内存屏障来禁止重排序

    *重排序是指编译器和处理器为了优化程序性能而对指令序列进行重新排序的一种手段*

#### volatile重排序规则表

为了实现volatile的内存语义，JMM针对编译器制定了volatile重排序规则表：

![在这里插入图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/volatile重排序规则表.png)

"NO"表示禁止重排序，为了实现volatile语义，编译器在生成字节码时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序，JMM采取了保守策略：

1.  在每个volatile**写**操作**前面**插入一个**StoreStore**屏障，禁止上面的普通写和下面的volatile写重排序；
2.  在每个volatile**写**操作**后面**插入一个**StoreLoad**屏障，防止上面的volatile写和下面可能有的volatile读/写
3.  在每个volatile**读**操作**后面**插入一个**LoadLoad**屏障，禁止上面的volatile读和下面所有的普通读重排序；
4.  在每个volatile**读**操作**后面**插入一个**LoadStore**屏障，禁止上面的volatile读和下面所有的普通写重排序。

![在这里插入图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/volatile写内存屏障.png)

![在这里插入图片描述](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/volatile读内存屏障.png)

#### **volatile原理**

在JVM底层volatile是采用“内存屏障”来实现的，编译后，会多出一个lock前缀指令，lock前缀指令实际上相当于一个内存屏障（也称内存栅栏），内存屏障会提供3个功能：

（1）它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；

（2）它会强制将对缓存的修改操作立即写入主存；

（3）如果是写操作，它会导致其他CPU中对应的缓存行无效。

### 并发编程的三个特性

1.  **原子性**：一个操作或者多个操作，要么所有操作都得到执行并且受到任何因素的干扰，要么都不执行 。`synchronized` 可以保证代码片段的原子性。
2.  **可见性**：当一个线程对共享变量做了修改，那么其他线程都是可以立即看到修改后的最新值。`synchronized`和`volatile`关键字可以保证共享变量的可见性，final也可以实现可见性。
3.  **有序性**：程序按照代码的先后顺序执行。volatile关键字可以禁止指令重排序。synchronized语义表示“一个变量在同一个时刻只能有一个线程对它进行lock操作”，就保证了多个线程只能“串行”进入同步代码块的。

### ThreadLocal

通常情况下，创建的变量是可以被所有线程访问并修改的，ThreadLocal可以为每个线程创建一个单独的变量副本

#### 原理

每个线程都有一个ThreadLocalMap类型的threadLocals成员变量，保存这个线程所有的的ThreadLocal变量，key为ThreadLocal对象，value就是ThreadLocal对象对应的值。threadLocals默认为null，只有当调用ThreadLocal的set和get方法时才会创建。

ThreadLocalMap是ThreadLocal静态内部类(类似HashMap)

#### ThreadLocal内存泄露问题

`ThreadLocalMap` 中使用的 key 为 `ThreadLocal` 的弱引用,而 value 是强引用。所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。这样一来，`ThreadLocalMap` 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。ThreadLocalMap 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 `ThreadLocal`方法后 最好手动调用`remove()`方法

### CAS(无锁技术)

#### 并发安全问题

volatile可以保证变量的可见性，但是不能保证原子操作。synchronized可以通过JVM字节码层面来实现同步机制，它是一个悲观锁机制。

```java
public class AddTest {
  public volatile int i;
  public void add() {
    i++;
  }
}
```

通过java -c AddTest;反编译得到字节码指令：

```java
public void add();
    Code:
       0: aload_0
       1: dup
       2: getfield      #2                  // Field i:I
       5: iconst_1
       6: iadd
       7: putfield      #2                  // Field i:I
      10: return
```

`i++`被拆分成了几个指令：

1.  执行`getfield`拿到原始 i；
2.  执行`iadd`进行加 1 操作；
3.  执行`putfield`写把累加后的值写回 i。

当线程1执行到加1的步骤时，由于还没执行赋值并改变变量的值，这是不会刷新最新值到主存中，如果此时线程2要拷贝变量1的副本到工作线程，当线程2拷贝完之后，线程1执行赋值立马刷新主内存的值，这时线程2的工作内存变量的值就是旧的。

可以通过加synchronized关键字解决这个问题，但是性能会降低。

CAS（Compare and Swap），即比较并替换，实现并发算法时常用到的一种技术，**CAS是通过unsafe类的compareAndSwap方法实现的**。
 CAS的思想很简单：三个参数，一个当前内存值V、旧的预期值A、即将更新的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做，并返回false。

#### 问题

1.  **ABA问题**

    如果一个值原来是A，变成了B，然后又变成了A，那么在CAS检查的时候会发现没有改变，但是实质上它已经发生了改变，这就是ABA问题。

    **举例：**	

    ```java
    现在有一个单向链表实现的栈，栈顶是A，A.next为B，期望CAS把栈顶元素改成B。
    有一个线程1获得了元素A，此时线程1被挂起，线程2也获得了元素A，并将A、B出栈，并push了D、C、A，这时线程1恢复执行CAS，因为栈顶元素还是A，替换执行成功，栈顶元素换成B，但是B.next为null，这就导致了C、D丢失了。
    ```

    对于ABA问题其解决方案是加上版本号，即在每个变量都加上一个版本号，每次改变时加1，即A —> B —> A，变成1A —> 2B —> 3A。**原子类之AtomicStampedReference可以解决ABA问题**

2.  **自旋问题**

    自旋问题就是，当期望值与内存值不同时，操作结果失败后继续循环操作，也叫做自旋锁，是一种乐观锁机制，一般来说都会给一个限定的自选次数，防止进入死循环。

    自旋锁不需要休眠当前线程，因为自旋锁使用者一般保持锁时间比较短，这样也避免了上下文切换的开销。

    但是，如果CAS长时间操作不成功，会造成长时间原地自旋，CPU执行开销大。

### 线程池

线程池实现原理：https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html

#### 创建线程池

线程池的创建主要是创建**ThreadPoolExecutor**对象完成

```java
ThreadPoolExecutor(int corePoolSize,
                   int maximumPoolSize,
                   long keepAliveTime,
                   TimeUnit unit,
                   BlockingQueue<Runnable> workQueue,
                   ThreadFactory threadFactory,
                   RejectedExecutionHandler handler)
```

参数说明：

1.  **corePoolSize**：核心线程池大小。如果调用了`preStartCoreThread()`或者`prestartAllCoreThreads()`，线程池创建时所有的核心线程都会被创建并且启动。
2.  **maximumPoolSize**：线程池的最大线程数。
3.  **keepAliveTime**：空闲线程存活时间。如果当前线程池的线程个数超过了corePoolSize，并且线程空闲时间超过了keepAliveTime的话，就会将这些空闲线程销毁。可以通过设置allowCoreThreadTimeOut(true)来设置核心线程也使用保活策略。
4.  **unit**：keepAliveTime的单位。
5.  **wordQueue**：阻塞队列(缓冲队列)，用来保存等待执行的任务的队列。
6.  **threadFactory**：创建线程的工厂类。可以通过指定的线程工作为每个创建出来的线程设置有意义的名字。
7.  **handler**：饱和策略。当线程池的阻塞队列已满，线程数达到maximumPoolSize时，再提交的任务要采用饱和策略处理：
    1.  AbortPolicy： 直接拒绝所提交的任务，并抛出RejectedExecutionException异常，默认的策略；
    2.  CallerRunsPolicy：只用调用者所在的线程来执行任务；
    3.  DiscardPolicy：不处理，静默丢弃掉任务；
    4.  DiscardOldestPolicy：丢弃掉阻塞队列中存放时间最久的任务(最久的未处理任务)，执行当前任务

#### 线程池运行流程

![图2 ThreadPoolExecutor运行流程](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池运行流程.png)

##### 任务管理：

​		任务管理充当生产者，当任务提交后，线程池判断任务后续的流转：①直接申请线程执行该任务；②缓冲阻塞队列中等待线程执行；③拒绝该任务

##### 线程管理：

​		线程管理部分是消费者，根据任务请求进行线程分配，线程执行完任务后会继续获取新的任务去执行，当线程空闲时就会被回收。

#### 线程池的生命周期

线程池维护了两个变量：**运行状态(runState)**和**有效线程数量(workCount)**

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池运行状态.png)

![图3 线程池生命周期](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池生命周期.png)

#### 线程池任务执行机制

##### 任务调度

当用户提交了一个线程，所有的任务调度都是由execute方法完成，包括检查线程池运行状态、运行线程数、饱和策略，决定任务要如何流转。

execute方法执行过程：

1.  检查线程池运行状态，如果不是RUNNING，则直接拒绝任务。
2.  如果workCount < corePoolSize，则创建并启动一个线程执行新提交的任务。
3.  如果workCount >= corePoolSize，且阻塞队列未满时，把任务添加到阻塞队列等待执行。
4.  如果workCount >= corePoolSize，并且阻塞队列已满，然后workCount < maximumPoolSize，则创建并启动一个线程执行新提交的任务。
5.  如果阻塞队列已满，workCount >= maximumPoolSize，根据饱和策略处理该任务，默认是直接抛异常。

![图4 任务调度流程](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池任务调度流程.png)

##### 任务缓冲

阻塞队列：

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池的阻塞队列.png)

##### 任务申请

任务执行有两种情况：①任务直接由新创建的线程执行；②任务在阻塞队列中被线程获取执行，执行完任务的空闲线程会再次去队列申请任务执行。第二种情况是线程获取任务的大多数情况。

##### 任务拒绝

四种拒绝策略

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/线程池拒绝策略.png)

## JVM

### 运行时数据区域

JVM数据区分为：java虚拟机栈、本地方法栈、程序计数器、堆，其中堆是线程共享的。

<center class="half">
    <img src="https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/JDK1.6运行时数据区域.png" width="50%"/>
    <img src="https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/Java运行时数据区域JDK1.8.png" width="49%"/>
</center>
#### 程序计数器

每个线程一块，是当前线程正在执行的字节码的行号指示器。

程序计数器是唯一一个不会出现OutOfMemoryError的内存区域，它的生命周期，随着线程的创建而创建，随着线程的结束和死亡。


#### 程序计数器为什么是私有的？

程序计数器有下面两个作用：

1.  字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。

2.  在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程切换回来的时候能够知道该线程上次运行到哪里。

    *需要注意的是，如果执行的是native方法，那么程序计数器记录的是undefined地址，只有执行java代码时程序计数器记录的才是下一条指令的地址。*

所以，程序计数器私有主要是**为了线程切换回来后能恢复到正确的执行位置。**

#### Java虚拟机栈

![stack](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/Java虚拟机栈.png)

Java虚拟机栈也是线程私有的，生命周期和线程相同，描述的是Java方法执行的内存模型，每次方法调用的数据都是通过栈传递的。

Java虚拟机栈是由一个一个栈帧组成的，每个方法被执行的同时会创建**栈桢**，方法执行时入栈，方法执行完出栈，每一个栈帧都拥有：局部变量表、操作数栈、动态链接、方法出口信息。局部变量表主要存放了：八大数据类型、对象引用。

Java虚拟机栈会出现两种错误：StackOverFlowError和OutOfMemoryError

-   StackOverFlowError：若虚拟机栈的内存不允许动态扩展，当线程请求栈的深度超过虚拟机栈最大深度时
-   OutOfMemoryError：虚拟机栈的内存运行动态扩展，如果在动态扩展栈时无法申请到足够的内存空间，就会抛出

#### 本地方法栈

本地方法栈和虚拟机栈非常相似，区别在于：java虚拟机栈为虚拟机执行Java方法服务，本地方法栈为虚拟机使用到的Native方法服务。在HotSpot虚拟机中和Java虚拟机栈合二为一。

#### 虚拟机栈和本地方法栈为什么是私有的？

-   **虚拟机栈**：每个Java方法在执行的同时会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，就对应这一个栈帧在Java虚拟机栈中入栈和出栈的过程。
-   **本地方法栈**：和虚拟机栈所发挥的作用非常类似，区别是：虚拟机栈为虚拟机执行Java方法(也就是字节码)服务，而本地方法栈则为虚拟机使用到的native方法服务。在HotSpot虚拟机中和Java虚拟机栈合二为一。

所以，**为了保证线程中的局部变量不被别的线程访问到**，虚拟机栈和本地方法栈是线程私有的。

#### 堆

![heap](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/堆.png)

堆是Java虚拟机所管理的内存中最大的一块，是所有线程共享的，在Java虚拟机启动时创建。**此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。**

1.7后，字符串常量池从永久代中剥离出来，存放在堆中。

##### 堆空间内存分配(默认情况下)

-   新生代：三分之一的堆空间
    -   eden区： 8/10 的新生代空间
    -   survivor0 : 1/10 的新生代空间(复制回收算法)
    -   survivor1 : 1/10 的新生代空间
-   老年代：三分之二的对空间

##### ⼀句话简单了解堆和⽅法区

堆和方法区是所有线程共享和资源，其中堆是进程中最大的一块区域，主要用于存放新创建的对象(所有对象都在这里分配内存)，**方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。**

#### 元数据区

元数据区取代了1.7及之前的永久代，元空间使用的是直接内存。元数据区和永久代本质上都是方法区的实现。方法区存放虚拟机加载的类信息，静态变量，常量等数据。

##### 为什么要用元空间替换永久代？

这个永久代有一个JVM本身设置的固定大小限制，元空间使用的是直接内存，受本机可用内存的限制，这样能加载的类就更多了，元空间内存溢出的概率比原来小。

#### 运行时常量池

1.7运行时常量池在方法区中，1.8之后永久代被元空间替代，运行时常量池还在方法区，只是方法区的实现变成了元空间。

#### 直接内存

**直接内存并不是虚拟机运行时数据区的一部分，也不是虚拟机规范中定义的内存区域，但是这部分内存也被频繁地使用。而且也可能导致 OutOfMemoryError 错误出现。**

### Java内存分配与回收

-   程序计数器是是唯一一个不会出现OOM的内存区域，所以不用GC；

-   Java虚拟机栈描述的是方法执行时的内存模型，方法执行时入栈，执行完出栈，出栈相当于清除了数据，出入栈的时机很明确，所以不用GC；
-   本地内存，不受JVM大小限制，所以也不用GC
-   堆，对象实例和数组在这块区域上分配内存，GC也主要是对这两类数据进行回收，堆是发生GC的区域

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/堆空间.png)

#### 对象优先在Eden区分配

大多数情况下，对象在新生代的Eden去分配，当Eden区没有足够的内存时，虚拟机将发起一次Minor GC

#### 大对象直接进入老年代

大对象就是需要大量连续内存空间的对象（比如：字符串、数组），为了避免对象先放入新生代后面发现新生代内存不够要放进老年代，省去这个过程会提高效率。

#### 长期存活的对象进入老年代

虚拟机采用了分代收集的思想管理内存，虚拟机给每个对象一个对象年龄(Age)计数器。

如果对象在Eden出生并经过一次Minor GC之后还存活，并且可以被Survivor容纳的话，会被移到Survivor空间，此时对象的年龄设为1，对象在Survivor每熬过一次Minor GC年龄就+1，当年龄到达一定程度(**默认15，不同的垃圾收集器不一样**)，就会晋升到老年代。

#### 动态年龄算法

当新生代的对象年龄达到一定程度会晋升到老年代，这个年龄阈值是动态的。

遍历所有对象，按照年龄从小到大，累积各个年龄的对象占用大小，当累积的某个年龄的大小超过Survivor区的一半时，取这个年龄和 MaxTenuringThreshold 中更小的一个值，作为新的晋升年龄阈值

### 判断对象已经死亡

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/对象已经死亡.png)

#### 引用计数法

给对象一个引用计数器，每当有一个地方引用它，计数器就+1；当引用失效，就-1；任何时候，计数器为0的对象就是不可能再被使用的。

这个方法简单，高效，但是主流的虚拟机没有采用这个算法来管理内存，因为它很难解决对象之间互相循环引用的问题。比如：除了对象 objA 和 objB 相互引用着对方之外，这两个对象之间再无任何引用。但是他们因为互相引用对方，导致它们的引用计数器都不为 0，于是引用计数算法无法通知 GC 回收器回收他们。

```java
public class ReferenceCountingGc {
    Object instance = null;
    public static void main(String[] args) {
        ReferenceCountingGc objA = new ReferenceCountingGc();
        ReferenceCountingGc objB = new ReferenceCountingGc();
        objA.instance = objB;
        objB.instance = objA;
        objA = null;
        objB = null;
    }
}
```

#### 可达性分析算法

这个算法的思想就是通过一系列的"GC Roots"对象作为起点，从这些节点向下搜索，走过的路径成为引用链，当一个对象到GC Roots没有任何引用链的话(就是到达不了GC Roots)，就说明这个对象是不可用的。

![可达性分析算法 ](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/GC Roots.png)

可以作为GC Roots的对象有：

-   虚拟机栈(栈帧中的本地变量表)中引用的对象
-   本地方法栈(Native 方法)中引用的对象
-   方法区中类静态属性引用的对象
-   方法区中常量引用的对象
-   所有被同步锁持有的对象

#### 引用

-   **强引用**：Object a = new Object ()，a是一个强引用，一个对象被强引用，GC OOM都不会回收它
-   **软引用**：当内存不足才会回收
-   **弱引用**：回收过程发现只具有弱引用的对象，就会回收，不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。
-   **虚引用**：一个对象仅持有虚引用，就和没有任何引用一样，在任何时候都可能被垃圾回收

#### 不可达的对象并非"非死不可"

即使在可达性分析算法中不可达的对象，也并非"非死不可"的。要真正回收一个对象，要经过两个标记过程：

1.  可达性分析法中不可达的对象进行**第一次标记**，并**筛选**，筛选条件是此对象是否有必要执行finalize()方法。

    -   **没有必要执行**：①当对象没有重写finalize()方法，②finalize()方法已经被虚拟机调用过了。这两种情况就可以认为对象已死可以回收

    -   **有必要执行** ：对有必要执行finalize()方法的对象，被放入F-Queue队列中；稍后在JVM自动建立、低优先级的Finalizer线程（可能多个线程）中触发这个方法

2.  **第二次标记**对F-Queue队列中的对象进行第二次小规模标记，除非这个对象在finalize方法中()与引用链上的任何一个对象建立关联，否则就会被真的回收。

**对象的finalize()方法只会被JVM执行一次，第二次GC再次回收该对象时就不会再执行了，直接会回收掉。**

### 垃圾收集算法

#### 标记清除

该算法分为"**标记**"和"**清除**"两个阶段：首先标记出所有不需要回收的对象，标记完成后清除所有没有被标记的对象。这个是最基础的算法，后面的算法都在在它之上进行改进。

**效率问题**、**空间问题（标记清除后会产生大量不连续的碎片）**

![img](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/标记-清除算法.jpeg)

#### 标记复制

标记复制法是解决效率问题，它可以把内存分为大小相同的两块，每次只使用其中一块，当这一块内存使用完后，把所有还存活的对象复制到另一块去，再把使用的空间一次性清除掉，这样每次回收都是对内存的一半进行回收。

![复制算法](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/标记-复制算法.png)

#### 标记整理

根据老年代的特点提出的一种标记算法，标记过程仍然与“标记-清除”算法一样，但不是标记不需要回收的对象，而是让所有存活的对象向一端移动，然后直接清理掉端边界以外的内存。

![标记-整理算法 ](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/标记-整理算法.png)

#### 分代收集

根据堆内存各个年代的特点选择回收算法

**新生代**每次收集会有大量的对象死亡，所以选择"**标记-复制**"，只需要少量的复制成本就可以完成每次回收。

**老年代**对象存活几率是比较高的，而且没有额外的空间对它进行分配担保，所以我们必须选择“标记-清除”或“标记-整理”算法进行垃圾收集

*HotSpot 为什么要分为新生代和老年代？*    主要是为了提升 GC 效率，使用分代收集算法。

### 什么时候会触发垃圾回收？

-   调用System.gc时，系统建议执行Full GC，但是不必然执行
-   当Eden区和From Survivor区满时
-   由Eden区、From Survivor区向To Survivor区复制时，对象大小大于To Survivor可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小
-   老年代空间不足
-   方法区(元空间)空间不足

### 垃圾收集器

7种  新生代：Serial、ParNew、Parallel Scavenge，老年代：Serial Old、Parallel Old、CMS，新老：G1

-   Serial收集器（复制算法): 新生代单线程收集器，标记和清理都是单线程，优点是简单高效；
-   ParNew收集器 (复制算法): 新生代收并行集器，实际上是Serial收集器的多线程版本，在多核CPU环境下有着比Serial更好的表现；
-   Parallel Scavenge收集器 (复制算法): 新生代并行收集器，追求高吞吐量，高效利用 CPU。吞吐量 = 用户线程时间/(用户线程时间+GC线程时间)，高吞吐量可以高效率的利用CPU时间，尽快完成程序的运算任务，适合后台应用等对交互相应要求不高的场景；
-   Serial Old收集器 (标记-整理算法): 老年代单线程收集器，Serial收集器的老年代版本；
-   Parallel Old收集器 (标记-整理算法)： 老年代并行收集器，吞吐量优先，Parallel Scavenge收集器的老年代版本；
-   CMS(Concurrent Mark Sweep)收集器（标记-清除算法）： 老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有高并发、低停顿的特点，追求最短GC回收停顿时间。
-   G1(Garbage First)收集器 (标记-整理算法)： Java堆并行收集器，G1收集器是JDK1.7提供的一个新收集器，G1收集器基于“标记-整理”算法实现，也就是说不会产生内存碎片。此外，G1收集器不同于之前的收集器的一个重要特点是：G1回收的范围是整个Java堆(包括新生代，老年代)，而前六种收集器回收的范围仅限于新生代或老年代。

### 类加载机制和类加载器

#### 类加载时机

1.  创建类的实例，也就是new一个对象。
2.  访问某个类或接口的静态变量，或者对静态变量赋值（被final修饰，已在编译器把结果放入常量池的静态字段除外）
3.  调用类的静态方法
4.  通过反射加载(Class.forName("com."))
5.  初始化一个类的子类(会先初始化这个类的父类)
6.  jvm启动是指定启动类，即文件名和类名一致

#### 类加载器

类加载器将类的class文件读入内存，并为之创建一个java.lang.Class对象，类通常是按需加载，即第一次使用该类时才加载，JVM允许预先加载某些类。

**在JVM通过类的全路径+这个类的加载器标识作为唯一限定**

类加载器通常由JVM提供，我们也可以通过继承java.lang.ClassLoader来自定义类加载器。

![image.png](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/类加载器分类.png)

#### 类加载机制

##### 双亲委派

![ClassLoader](https://raw.githubusercontent.com/2587696775/Java-Notes-Pics/main/Java基础/双亲委派机制.png)

类加载时，首先会判断这个类是否被加载过，已经被加载过的类会直接返回，否则才尝试加载。加载的时候，会先请求父类加载器加载，父类加载器无法加载时，才会自己处理。

当父类加载器为null时，会使用启动类加载器 `BootstrapClassLoader` 作为父类加载器，因此所有的请求最终都应该传送到顶层的启动类加载器 `BootstrapClassLoader` 中。

`AppClassLoader`的父类加载器为`ExtClassLoader` ，`ExtClassLoader`的父类加载器为null，**null并不代表`ExtClassLoader`没有父类加载器，而是 `BootstrapClassLoader`** 。

**双亲委派的好处**

可以避免重复加载类，保证Java核心API不被篡改。JVM是通过类名+类加载器标识不同的类，如果不用双亲委派，通过自定义类加载器加载类，假如我们编写一个java.lang.Object类的话，程序运行的时候，就会出现两个Object。