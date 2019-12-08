# 为什么Java应用最广泛？

从互联网到企业平台，Java是应用最广泛的编程语言，原因在于：

- Java是基于JVM虚拟机的跨平台语言，一次编写，到处运行；
- Java程序易于编写，而且有内置垃圾收集，不必考虑内存管理；
- Java虚拟机拥有工业级的稳定性和高度优化的性能，且经过了长时期的考验；
- Java拥有最广泛的开源社区支持，各种高质量组件随时可用。

Java语言常年霸占着三大市场：

- 互联网和企业应用，这是Java EE的长期优势和市场地位；
- 大数据平台，主要有Hadoop、Spark、Flink等，他们都是Java或Scala（一种运行于JVM的编程语言）开发的；
- Android移动平台。

这意味着Java拥有最广泛的就业市场。

# Java虚拟机——JVM

JVM(Java Virtual Machine ):Java虚拟机，简称JVM，是运行所有Java程序的假想计算机，是Java程序的运行环境，是Java 最具吸引力的特性之一。我们编写的Java代码，都运行在 JVM 之上。

跨平台:任何软件的运行，都必须要运行在操作系统之上，而我们用Java编写的软件可以运行在任何的操作系 统上，这个特性称为Java语言的跨平台特性。该特性是由JVM实现的，我们编写的程序运行在JVM上，而JVM 运行在操作系统上。

# JRE和JDK

- JRE (Java Runtime Environment) :是Java程序的运行时环境，包含 JVM 和运行时所需要的 核心类库 。 
- JDK (Java Development Kit):是Java程序开发工具包，包含 JRE 和开发人员使用的工具。
- JDK>JRE>JVM

我们想要运行一个已有的Java程序，那么只需安装 JRE 即可。 我们想要开发一个全新的Java程序，那么必须安装 JDK 。

# 入门程序说明

```Java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!"); }
}
```

- 编译:是指将我们编写的Java源文件翻译成JVM认识的class文件，在这个过程中， javac 编译器会检查我们 所写的程序是否有错误，有错误就会提示出来，如果没有错误就会编译成功。
- 运行:是指将 class文件 交给JVM去运行，此时JVM就会去执行我们编写的程序了。
- main方法:称为主方法。写法是固定格式不可以更改。main方法是程序的入口点或起始点，无论我们编写多少程序，JVM在运行的时候，都会从main方法这里开始执行。
- 注释:就是对代码的解释和说明。其目的是让人们能够更加轻松地了解代码。为代码添加注释，是十分必须 要的，它不影响程序的编译和运行。

- 关键字:是指在程序中，Java已经定义好的单词，具有特殊含义。
  - HelloWorld案例中，出现的关键字有 public 、 class 、 static 、 void 等，这些单词已经被 Java定义好，全部都是小写字母，notepad++中颜色特殊。
- 标识符:是指在程序中，我们自己定义内容。比如类的名字、方法的名字和变量的名字等等，都是标识符。 
  - HelloWorld案例中，出现的标识符有类名字 HelloWorld 。
- 命名规则: 硬性要求
  - 标识符可以包含 英文字母26个(区分大小写) 、 0-9数字 、 $(美元符号) 和 _(下划线) 。 
  - 标识符不能以数字开头。
  -  标识符不能是关键字。

- 命名规范: 软性建议
  - 类名规范:首字母大写，后面每个单词首字母大写(大驼峰式)。 
  - 方法名规范: 首字母小写，后面每个单词首字母大写(小驼峰式)。 
  - 变量名规范:全部小写。

# Java数据类型

## 数据类型分类

Java的数据类型分为两大类:

- 基本数据类型:包括 整数、浮点数、字符、布尔。 
- 引用数据类型:包括 类 、 数组 、 接口 。

## 变量的定义

- 格式
  - 数据类型 变量名 = 数据值;

- 基本数据类型

```java
public class Variable {
public static void main(String[] args){
//定义字节型变量
byte b = 100; System.out.println(b); //定义短整型变量
short s = 1000; System.out.println(s); //定义整型变量
int i = 123456; System.out.println(i); //定义长整型变量
long l = 12345678900L; System.out.println(l); //定义单精度浮点型变量 float f = 5.5F; System.out.println(f); //定义双精度浮点型变量 double d = 8.5; System.out.println(d); //定义布尔型变量
boolean bool = false; System.out.println(bool); //定义字符型变量
char c = 'A'; System.out.println(c);
} }

//变量名称:在同一个大括号范围内，变量的名字不可以相同。 
//变量赋值:定义的变量，不赋值不能使用。
```

