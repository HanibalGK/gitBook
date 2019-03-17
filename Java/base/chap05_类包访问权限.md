# Java 类 & 包的访问权限

1. 为什么会有访问权限控制

> 为了将变动的事物与保持不变的事物区分开来
>
> 类库的使用者必须依赖他所使用的那部分类库, 并且能够知道类库出现了新版本, 而并不需要修改代码。

2. 访问权限关键字

`public`,`protected`,`包访问权限(无关键字)`,`private`

## 包: 库单元

> 包内含有一组类, 它们在单一的名字空间之下被组织在一起

例如 ArrayList:

```java
// 全名指定
java.util.ArrayList<String> list = new java.util.ArrayList<>();
// 使用 import 引入
import java.util.ArrayList;
ArrayList<String> list = new ArrayList<>();
```

如果需要导入包下所有的类的话, 只需要使用 `*`

```java
import java.util.*;
```

### 代码组织

#### 代码规范

1.1. 包名全部使用小写, 中间字也是小写

1.2. 包名第一部分是网络域名的反序列 (net.mindmanager.XXX)

1.3. package & import 作用是将全局名字空间分隔开,

使得无论多少人使用Internet 以及 Java 开始编写类, 都不会出现名称冲突问题


#### 代码的编译和加载

1. 编译

编译 .java 文件时, 流程如下:

.java  --->  编译器  ---> .obj ---> 链接器 / 类库产生器 ---> 同类文件捆绑在一起 .class

`jar` 命令可以打包并压缩 .class 文件

2. 执行

Java 解释器运行过程:

    1. 找出环境变量 CLASSPATH (包含一个或多个目录, 用作查找 .class 文件根目录)
    2. 解释器获取包名称, 将 "." 替换成 "/" (example: foo.bar.baz -> foo\bar\baz 或 foo/bar/baz)
    3. 得到路径与不同 CLASSPATH 连接, 解释器查找索要创建类的名称相关的 .class文件

example:

```java
/**
 * PACKAGE: net.mindmanager.simple
 * CLASS  : mindmanager
 **/
public class List {

    public List() {
        System.out.println("net.mindmanager.simple.List");
    }

}
```
```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
public class Vector {

    public Vector() {
        System.out.println("net.mindmanager.simple.Vector");
    }

}
```

```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
public class LibTest {

    public static void main(String[] args) {
        Vector v = new Vector();
        List l = new List();
    }

}
```

```
// Output
net.mindmanager.simple.Vector
net.mindmanager.simple.List
```

3. 包冲突

当 `import` 的包内有相同类名的类时, 需要对类进行全名引入, 以避免在运行时出现引用类错误

## Java 访问权限修饰

> public \ protected \ private

1. public

任意谁都可以访问

```java
public class Cookie {
  public Cookie() {
    System.out.println("Cookie Contructor");
  }
  public bite() {
    System.out.println("bite");
  }
}
```
```java
public static void main(String[] args) {
  Cookie x = new Cookie();
  x.bite();
}
```

2. 默认什么也不添加

```java
class Cookie {
  public Cookie() {
    System.out.println("Cookie Contructor");
  }
  public bite() {
    System.out.println("bite");
  }
}
```
```java
public static void main(String[] args) {
  Cookie x = new Cookie();
  x.bite();
}
```

依然可以访问, 两个类在同一个包里面

3. private

无法访问

```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
class Sundae {
    private Sundae() { }
    static Sundae makeSundae() {
        return new Sundae();
    }
}
public class IceCrame {
    public static void main(String[] args) {
        // 因为构造方法已经私有化(private), 无法直接创建
        //! Sundae x = new Sundae();
        Sundae x = Sundae.makeSundae();
    }
}
```

private 是阻止别人直接访问某个特定构造器

4. protected

只有继承该类的时候, 才可有访问 protected 修饰的字段或方法

```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
public class Cookie2 {

    public Cookie2() {
        System.out.println("net.mindmanager.access.Cookie2");
    }

    protected void bite() {
        System.out.println("bite");
    }

}
```

```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
public class ChocolateChip2 extends Cookie2 {
    public ChocolateChip2() {
        System.out.println("net.mindmanager.access.ChocolateChip2");
    }
    public void chop() {
        bite();
    }
    public static void main(String[] args) {
        ChocolateChip2 chocolateChip2 = new ChocolateChip2();
        chocolateChip2.chop();
    }

}
```

## 类访问权限

1. 编译文件只有一个 public 类
2. public 类名称必须与编译文件名称匹配 (包括大小写)
3. 可以完全不带 public
4. 类只能是 public / default (默认修饰)

```java
/**
 * PACKAGE: net.mindmanager.access
 * CLASS  : mindmanager
 **/
class Soup1 {
    private Soup1() { }
    public static Soup1 makeSoup1() {
        return new Soup1();
    }
}
class Soup2 {
    private static Soup2 soup2 = new Soup2();
    private Soup2() {}
    public static Soup2 access() {
        return soup2;
    }
    public void f() {}
}
public class Lunch {

    public static void main(String[] args) {

    }
    void testPrivate() {
        // Soup1 soup = new Soup1();
    }
    void testStatic() {
        Soup1 soup = Soup1.makeSoup1();
    }
    void testSingleton() {
        Soup2.access().f();
    }
}
```
