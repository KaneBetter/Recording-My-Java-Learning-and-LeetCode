# 廖雪峰JAVA

## 面向对象编程

### JAVA核心类

#### 包装类型

Java核心库提供的包装类型可以把基本类型包装为`class`；

自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；

装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；

包装类型的比较必须使用`equals()`；

整数和浮点数的包装类型都继承自`Number`；

包装类型提供了大量实用方法。

#### JavaBean

JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；

属性是一种通用的叫法，并非Java语法规定；

可以利用IDE快速生成`getter`和`setter`；

使用`Introspector.getBeanInfo()`可以获取属性列表。

#### 枚举类

Java使用`enum`定义枚举类型，它被编译器编译为`final class Xxx extends Enum { … }`；

通过`name()`获取常量定义的字符串，注意不要使用`toString()`；

通过`ordinal()`返回常量定义的顺序（无实质意义）；

可以为`enum`编写构造方法、字段和方法

`enum`的构造方法要声明为`private`，字段强烈建议声明为`final`；

`enum`适合用在`switch`语句中。

#### 记录类

从Java 14开始，提供新的`record`关键字，可以非常方便地定义Data Class：

- 使用`record`定义的是不变类；
- 可以编写Compact Constructor对参数进行验证；
- 可以定义静态方法。

#### BigInteger

`BigInteger`用于表示任意大小的整数；

`BigInteger`是不变类，并且继承自`Number`；

将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

#### BigDecimal

`BigDecimal`用于表示精确的小数，常用于财务计算；

比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。

#### 常用工具类

Java提供的常用工具类有：

- Math：数学计算
- Random：生成伪随机数
- SecureRandom：生成安全的随机数

## 异常处理

### Java的异常

Java使用异常来表示错误，并通过`try ... catch`捕获异常；

Java的异常是`class`，并且从`Throwable`继承；

`Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误；

`RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明；

不推荐捕获了异常但不进行任何处理。

### 捕获异常

使用`try ... catch ... finally`时：

- 多个`catch`语句的匹配顺序非常重要，子类必须放在前面；
- `finally`语句保证了有无异常都会执行，它是可选的；
- 一个`catch`语句也可以匹配多个非继承关系的异常。

### 抛出异常

调用`printStackTrace()`可以打印异常的传播栈，对于调试非常有用；

捕获异常并再次抛出新的异常时，应该持有原始异常信息；

通常不要在`finally`中抛出异常。如果在`finally`中抛出异常，应该原始异常加入到原有异常中。调用方可通过`Throwable.getSuppressed()`获取所有添加的`Suppressed Exception`。

### 自定义异常

抛出异常时，尽量复用JDK已定义的异常类型；

自定义异常体系时，推荐从`RuntimeException`派生“根异常”，再派生出业务异常；

自定义异常时，应该提供多种构造方法。

### NullPointerException

`NullPointerException`是Java代码常见的逻辑错误，应当早暴露，早修复；

可以启用Java 14的增强异常信息来查看`NullPointerException`的详细错误信息。

### 使用断言

断言是一种调试方式，断言失败会抛出`AssertionError`，只能在开发和测试阶段启用断言；

对可恢复的错误不能使用断言，而应该抛出异常；

断言很少被使用，更好的方法是编写单元测试。

### 使用JDK Logging

日志是为了替代`System.out.println()`，可以定义格式，重定向到文件等；

日志可以存档，便于追踪问题；

日志记录可以按级别分类，便于打开或关闭某些级别；

可以根据配置文件调整日志，无需修改代码；

Java标准库提供了`java.util.logging`来实现日志功能。

### 使用Commons Logging

和Java标准库提供的日志不同，Commons Logging是一个第三方日志库，它是由Apache创建的日志模块。

Commons Logging是使用最广泛的日志模块；

Commons Logging的API非常简单；

Commons Logging可以自动检测并使用其他日志模块。

### 使用Log4j

通过Commons Logging实现日志，不需要修改代码即可使用Log4j；

使用Log4j只需要把log4j2.xml和相关jar放入classpath；

如果要更换Log4j，只需要移除log4j2.xml和相关jar；

只有扩展Log4j时，才需要引用Log4j的接口（例如，将日志加密写入数据库的功能，需要自己开发）。

### 使用SLF4J和Logback

SLF4J和Logback可以取代Commons Logging和Log4j；

始终使用SLF4J的接口写入日志，使用Logback只需要配置，不需要修改代码。

## 反射

### Class类

JVM为每个加载的`class`及`interface`创建了对应的`Class`实例来保存`class`及`interface`的所有信息；

获取一个`class`对应的`Class`实例后，就可以获取该`class`的所有信息；

通过Class实例获取`class`信息的方法称为反射（Reflection）；

JVM总是动态加载`class`，可以在运行期根据条件来控制加载class。

### 访问字段

Java的反射API提供的`Field`类封装了字段的所有信息：

通过`Class`实例的方法可以获取`Field`实例：`getField()`，`getFields()`，`getDeclaredField()`，`getDeclaredFields()`；

通过Field实例可以获取字段信息：`getName()`，`getType()`，`getModifiers()`；

通过Field实例可以读取或设置某个对象的字段，如果存在访问限制，要首先调用`setAccessible(true)`来访问非`public`字段。

通过反射读写字段是一种非常规方法，它会破坏对象的封装。

### 调用方法

Java的反射API提供的Method对象封装了方法的所有信息：

通过`Class`实例的方法可以获取`Method`实例：`getMethod()`，`getMethods()`，`getDeclaredMethod()`，`getDeclaredMethods()`；

通过`Method`实例可以获取方法信息：`getName()`，`getReturnType()`，`getParameterTypes()`，`getModifiers()`；

通过`Method`实例可以调用某个对象的方法：`Object invoke(Object instance, Object... parameters)`；

通过设置`setAccessible(true)`来访问非`public`方法；

通过反射调用方法时，仍然遵循多态原则。

### 调用构造方法

`Constructor`对象封装了构造方法的所有信息；

通过`Class`实例的方法可以获取`Constructor`实例：`getConstructor()`，`getConstructors()`，`getDeclaredConstructor()`，`getDeclaredConstructors()`；

通过`Constructor`实例可以创建一个实例对象：`newInstance(Object... parameters)`； 通过设置`setAccessible(true)`来访问非`public`构造方法。

### 获取继承关系

通过`Class`对象可以获取继承关系：

- `Class getSuperclass()`：获取父类类型；
- `Class[] getInterfaces()`：获取当前类实现的所有接口。

通过`Class`对象的`isAssignableFrom()`方法可以判断一个向上转型是否可以实现。

### 动态代理

Java标准库提供了动态代理功能，允许在运行期动态创建一个接口的实例；

动态代理是通过`Proxy`创建代理对象，然后将接口方法“代理”给`InvocationHandler`完成的。

## 注解（Annotation）

### 使用注解

注解（Annotation）是Java语言用于工具处理的标注：

注解可以配置参数，没有指定配置的参数使用默认值；

如果参数名称是`value`，且只有一个参数，那么可以省略参数名称。

### 定义注解

Java使用`@interface`定义注解：

可定义多个参数和默认值，核心参数使用`value`名称；

必须设置`@Target`来指定`Annotation`可以应用的范围；

应当设置`@Retention(RetentionPolicy.RUNTIME)`便于运行期读取该`Annotation`。

### 处理注解

可以在运行期通过反射读取`RUNTIME`类型的注解，注意千万不要漏写`@Retention(RetentionPolicy.RUNTIME)`，否则运行期无法读取到该注解。

可以通过程序处理注解来实现相应的功能：

- 对JavaBean的属性值按规则进行检查；
- JUnit会自动运行`@Test`标记的测试方法。

## 泛型

泛型是一种“代码模板”，可以用一套代码套用各种类型。

### 什么是泛型

泛型就是编写模板代码来适应任意类型；

泛型的好处是使用时不必对类型进行强制转换，它通过编译器对类型进行检查；

注意泛型的继承关系：可以把`ArrayList`向上转型为`List`（`T`不能变！），但不能把`ArrayList`向上转型为`ArrayList`（`T`不能变成父类）。

### 使用泛型

使用泛型时，把泛型参数``替换为需要的class类型，例如：`ArrayList`，`ArrayList`等；

可以省略编译器能自动推断出的类型，例如：`List list = new ArrayList<>();`；

不指定泛型参数类型时，编译器会给出警告，且只能将``视为`Object`类型；

可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

### 编写泛型

编写泛型时，需要定义泛型类型``；

静态方法不能引用泛型类型``，必须定义其他类型（例如``）来实现静态泛型方法；

泛型可以同时定义多种类型，例如`Map`。

### 擦拭法

Java的泛型是采用擦拭法实现的；

擦拭法决定了泛型``：

- 不能是基本类型，例如：`int`；
- 不能获取带泛型类型的`Class`，例如：`Pair.class`；
- 不能判断带泛型类型的类型，例如：`x instanceof Pair`；
- 不能实例化`T`类型，例如：`new T()`。

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

子类可以获取父类的泛型类型``。

### extends通配符

使用类似``通配符作为方法参数时表示：

- 方法内部可以调用获取`Number`引用的方法，例如：`Number n = obj.getFirst();`；
- 方法内部无法调用传入`Number`引用的方法（`null`除外），例如：`obj.setFirst(Number n);`。

即一句话总结：使用`extends`通配符表示可以读，不能写。

使用类似``定义泛型类时表示：

- 泛型类型限定为`Number`以及`Number`的子类。

### super通配符

使用类似``通配符作为方法参数时表示：

- 方法内部可以调用传入`Integer`引用的方法，例如：`obj.setFirst(Integer n);`；
- 方法内部无法调用获取`Integer`引用的方法（`Object`除外），例如：`Integer n = obj.getFirst();`。

即使用`super`通配符表示只能写不能读。

使用`extends`和`super`通配符要遵循PECS原则。

无限定通配符``很少使用，可以用``替换，同时它是所有``类型的超类。

### 泛型和反射

部分反射API是泛型，例如：`Class`，`Constructor`；

可以声明带泛型的数组，但不能直接创建带泛型的数组，必须强制转型；

可以通过`Array.newInstance(Class, int)`创建`T[]`数组，需要强制转型；

同时使用泛型和可变参数时需要特别小心。

## 集合

### Java集合简介

Java的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。Java集合使用统一的`Iterator`遍历，尽量不要使用遗留接口。

### 使用List

`List`是按索引顺序访问的长度可变的有序表，优先使用`ArrayList`而不是`LinkedList`；

可以直接使用`for each`遍历`List`；

`List`可以和`Array`相互转换。

### 编写equals方法

在`List`中查找元素时，`List`的实现类通过元素的`equals()`方法比较两个元素是否相等，因此，放入的元素必须正确覆写`equals()`方法，Java标准库提供的`String`、`Integer`等已经覆写了`equals()`方法；

编写`equals()`方法可借助`Objects.equals()`判断。

如果不在`List`中查找元素，就不必覆写`equals()`方法。

### 使用Map

`Map`是一种映射表，可以通过`key`快速查找`value`。

可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。

最常用的一种`Map`实现是`HashMap`。

### 编写equals和hashCode

要正确使用`HashMap`，作为`key`的类必须正确覆写`equals()`和`hashCode()`方法；

一个类如果覆写了`equals()`，就必须覆写`hashCode()`，并且覆写规则是：

- 如果`equals()`返回`true`，则`hashCode()`返回值必须相等；
- 如果`equals()`返回`false`，则`hashCode()`返回值尽量不要相等。

实现`hashCode()`方法可以通过`Objects.hashCode()`辅助方法实现。

### 使用EnumMap

如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。

### 使用TreeMap

`SortedMap`在遍历时严格按照Key的顺序遍历，最常用的实现类是`TreeMap`；

作为`SortedMap`的Key必须实现`Comparable`接口，或者传入`Comparator`；

要严格按照`compare()`规范实现比较逻辑，否则，`TreeMap`将不能正常工作。

### 使用Properties

Java集合库提供的`Properties`用于读写配置文件`.properties`。`.properties`文件可以使用UTF-8编码。

可以从文件系统、classpath或其他任何地方读取`.properties`文件。

读写`Properties`时，注意仅使用`getProperty()`和`setProperty()`方法，不要调用继承而来的`get()`和`put()`等方法。

### [使用Set](https://www.liaoxuefeng.com/wiki/1252599548343744/1265121225603904)

### [使用Queue](https://www.liaoxuefeng.com/wiki/1252599548343744/1265121791832960)

### [使用PriorityQueue](https://www.liaoxuefeng.com/wiki/1252599548343744/1265120632401152)

### [使用Deque](https://www.liaoxuefeng.com/wiki/1252599548343744/1265122668445536)

### [使用Stack](https://www.liaoxuefeng.com/wiki/1252599548343744/1265121668997888)

### [使用Iterator](https://www.liaoxuefeng.com/wiki/1252599548343744/1265124784468736)

### [使用Collections](https://www.liaoxuefeng.com/wiki/1252599548343744/1299919855943714)

## [IO](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945227202752)

### [File对象](https://www.liaoxuefeng.com/wiki/1252599548343744/1298069154955297)

Java标准库的`java.io.File`对象表示一个文件或者目录：

- 创建`File`对象本身不涉及IO操作；
- 可以获取路径／绝对路径／规范路径：`getPath()`/`getAbsolutePath()`/`getCanonicalPath()`；
- 可以获取目录的文件和子目录：`list()`/`listFiles()`；
- 可以创建或删除文件和目录。

### [InputStream](https://www.liaoxuefeng.com/wiki/1252599548343744/1298069163343905)

### [OutputStream](https://www.liaoxuefeng.com/wiki/1252599548343744/1298069169635361)

### [Filter模式](https://www.liaoxuefeng.com/wiki/1252599548343744/1298364142452770)

### [操作Zip](https://www.liaoxuefeng.com/wiki/1252599548343744/1298366336073762)

### [读取classpath资源](https://www.liaoxuefeng.com/wiki/1252599548343744/1298366384308257)

### [序列化](https://www.liaoxuefeng.com/wiki/1252599548343744/1298366845681698)

### [Reader](https://www.liaoxuefeng.com/wiki/1252599548343744/1298366902304801)

### [Writer](https://www.liaoxuefeng.com/wiki/1252599548343744/1298366912790561)

### [PrintStream和PrintWriter](https://www.liaoxuefeng.com/wiki/1252599548343744/1302299230076961)

## [日期与时间](https://www.liaoxuefeng.com/wiki/1252599548343744/1255943660631584)

### [基本概念](https://www.liaoxuefeng.com/wiki/1252599548343744/1298613246361634)

在编写日期和时间的程序前，我们要准确理解日期、时间和时刻的概念。

由于存在本地时间，我们需要理解时区的概念，并且必须牢记由于夏令时的存在，同一地区用`GMT/UTC`和城市表示的时区可能导致时间不同。

计算机通过`Locale`来针对当地用户习惯格式化日期、时间、数字、货币等。

### [Date和Calendar](https://www.liaoxuefeng.com/wiki/1252599548343744/1303791989162017)

计算机表示的时间是以整数表示的时间戳存储的，即Epoch Time，Java使用`long`型来表示以毫秒为单位的时间戳，通过`System.currentTimeMillis()`获取当前时间戳。

Java有两套日期和时间的API：

- 旧的Date、Calendar和TimeZone；
- 新的LocalDateTime、ZonedDateTime、ZoneId等。

分别位于`java.util`和`java.time`包中。

### [LocalDateTime](https://www.liaoxuefeng.com/wiki/1252599548343744/1303871087444002)

使用`LocalDateTime`可以非常方便地对日期和时间进行加减，或者调整日期和时间，它总是返回新对象；

使用`isBefore()`和`isAfter()`可以判断日期和时间的先后；

使用`Duration`和`Period`可以表示两个日期和时间的“区间间隔”。

### [ZonedDateTime](https://www.liaoxuefeng.com/wiki/1252599548343744/1303904694304801)

`ZonedDateTime`是带时区的日期和时间，可用于时区转换；

`ZonedDateTime`和`LocalDateTime`可以相互转换。

### [DateTimeFormatter](https://www.liaoxuefeng.com/wiki/1252599548343744/1303985694703650)

对`ZonedDateTime`或`LocalDateTime`进行格式化，需要使用`DateTimeFormatter`类；

`DateTimeFormatter`可以通过格式化字符串和`Locale`对日期和时间进行定制输出。

### [Instant](https://www.liaoxuefeng.com/wiki/1252599548343744/1303905346519074)

`nstant`表示高精度时间戳，它可以和`ZonedDateTime`以及`long`互相转换。

### [最佳实践](https://www.liaoxuefeng.com/wiki/1252599548343744/1303978948165666)

处理日期和时间时，尽量使用新的`java.time`包；

在数据库中存储时间戳时，尽量使用`long`型时间戳，它具有省空间，效率高，不依赖数据库的优点。

## [单元测试](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945269146912)

### [编写JUnit测试](https://www.liaoxuefeng.com/wiki/1252599548343744/1304048154181666)

JUnit是一个单元测试框架，专门用于运行我们编写的单元测试：

一个JUnit测试包含若干`@Test`方法，并使用`Assertions`进行断言，注意浮点数`assertEquals()`要指定`delta`。

### [使用Fixture](https://www.liaoxuefeng.com/wiki/1252599548343744/1304049490067490)

编写Fixture是指针对每个`@Test`方法，编写`@BeforeEach`方法用于初始化测试资源，编写`@AfterEach`用于清理测试资源；

必要时，可以编写`@BeforeAll`和`@AfterAll`，使用静态变量来初始化耗时的资源，并且在所有`@Test`方法的运行前后仅执行一次。

### [异常测试](https://www.liaoxuefeng.com/wiki/1252599548343744/1304064312737826)

测试异常可以使用`assertThrows()`，期待捕获到指定类型的异常；

对可能发生的每种类型的异常都必须进行测试。

### [条件测试](https://www.liaoxuefeng.com/wiki/1252599548343744/1304073489874978)

### [参数化测试](https://www.liaoxuefeng.com/wiki/1252599548343744/1304065789132833)

使用参数化测试，可以提供一组测试数据，对一个测试方法反复测试。

参数既可以在测试代码中写死，也可以通过`@CsvFileSource`放到外部的CSV文件中。

## [正则表达式](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945288020320)

### [正则表达式简介](https://www.liaoxuefeng.com/wiki/1252599548343744/1304066130968610)

正则表达式是用字符串描述的一个匹配规则，使用正则表达式可以快速判断给定的字符串是否符合匹配规则。Java标准库`java.util.regex`内建了正则表达式引擎。

### [匹配规则](https://www.liaoxuefeng.com/wiki/1252599548343744/1304066080636961)

### [复杂匹配规则](https://www.liaoxuefeng.com/wiki/1252599548343744/1306046675025953)

### [分组匹配](https://www.liaoxuefeng.com/wiki/1252599548343744/1306046706483233)

正则表达式用`(...)`分组可以通过`Matcher`对象快速提取子串：

- `group(0)`表示匹配的整个字符串；
- `group(1)`表示第1个子串，`group(2)`表示第2个子串，以此类推。

### [非贪婪匹配](https://www.liaoxuefeng.com/wiki/1252599548343744/1306046731649057)

正则表达式匹配默认使用贪婪匹配，可以使用`?`表示对某一规则进行非贪婪匹配。

注意区分`?`的含义：`\d??`。

### [搜索和替换](https://www.liaoxuefeng.com/wiki/1252599548343744/1306046817632290)

使用正则表达式可以：

- 分割字符串：`String.split()`
- 搜索子串：`Matcher.find()`
- 替换字符串：`String.replaceAll()`

## [加密与安全](https://www.liaoxuefeng.com/wiki/1252599548343744/1255943717668160)

### [编码算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227703947297)

URL编码和Base64编码都是编码算法，它们不是加密算法；

URL编码的目的是把任意文本数据编码为%前缀表示的文本，便于浏览器和服务器处理；

Base64编码的目的是把任意二进制数据编码为文本，但编码后数据量会增加1/3。

### [哈希算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227729113121)

哈希算法可用于验证数据完整性，具有防篡改检测的功能；

常用的哈希算法有MD5、SHA-1等；

用哈希存储口令时要考虑彩虹表攻击。

```java
import java.math.BigInteger;
import java.security.MessageDigest;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建一个MessageDigest实例:
        MessageDigest md = MessageDigest.getInstance("SHA-1");
        // 反复调用update输入数据:
        md.update("Hello".getBytes("UTF-8"));
        md.update("World".getBytes("UTF-8"));
        byte[] result = md.digest(); // 20 bytes: db8ac1c259eb89d4a131b253bacfca5f319d54f2
        System.out.println(new BigInteger(1, result).toString(16));
    }
}
```

### [BouncyCastle](https://www.liaoxuefeng.com/wiki/1252599548343744/1305362418368545)

BouncyCastle是一个开源的第三方算法提供商；

BouncyCastle提供了很多Java标准库没有提供的哈希算法和加密算法；

使用第三方算法前需要通过`Security.addProvider()`注册。

### [Hmac算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1305366354722849)

Hmac算法是一种标准的基于密钥的哈希算法，可以配合MD5、SHA-1等哈希算法，计算的摘要长度和原摘要算法长度相同。

### [对称加密算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227762667553)

对称加密算法使用同一个密钥进行加密和解密，常用算法有DES、AES和IDEA等；

密钥长度由算法设计决定，AES的密钥长度是128/192/256位；

使用对称加密算法需要指定算法名称、工作模式和填充模式。

### [口令加密算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227859136546)

PBE算法通过用户口令和安全的随机salt计算出Key，然后再进行加密；

Key通过口令和安全的随机salt计算得出，大大提高了安全性；

PBE算法内部使用的仍然是标准对称加密算法（例如AES）。

### [密钥交换算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227905273889)

DH算法是一种密钥交换协议，通信双方通过不安全的信道协商密钥，然后进行对称加密传输。

DH算法没有解决中间人攻击。

### [非对称加密算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227873816610)

非对称加密就是加密和解密使用的不是相同的密钥，只有同一个公钥-私钥对才能正常加解密；

只使用非对称加密算法不能防止中间人攻击。

### [签名算法](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227943022626)

数字签名就是用发送方的私钥对原始数据进行签名，只有用发送方公钥才能通过签名验证。

数字签名用于：

- 防止伪造；
- 防止抵赖；
- 检测篡改。

常用的数字签名算法包括：MD5withRSA／SHA1withRSA／SHA256withRSA／SHA1withDSA／SHA256withDSA／SHA512withDSA／ECDSA等。

### [数字证书](https://www.liaoxuefeng.com/wiki/1252599548343744/1304227968188450)

摘要算法用来确保数据没有被篡改，非对称加密算法可以对数据进行加解密，签名算法可以确保数据完整性和抗否认性，把这些算法集合到一起，并搞一套完善的标准，这就是数字证书。

数字证书就是集合了多种密码学算法，用于实现数据加解密、身份认证、签名等多种功能的一种安全标准。

数字证书采用链式签名管理，顶级的Root CA证书已内置在操作系统中。

数字证书存储的是公钥，可以安全公开，而私钥必须严格保密。

## [多线程](https://www.liaoxuefeng.com/wiki/1252599548343744/1255943750561472)

### [多线程基础](https://www.liaoxuefeng.com/wiki/1252599548343744/1304521607217185)

Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；

一个线程对象只能调用一次`start()`方法；

线程的执行代码写在`run()`方法中；

线程调度由操作系统决定，程序本身无法决定调度顺序；

`Thread.sleep()`可以把当前线程暂停一段时间。

### [创建新线程](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580710588449)

Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；

一个线程对象只能调用一次`start()`方法；

线程的执行代码写在`run()`方法中；

线程调度由操作系统决定，程序本身无法决定调度顺序；

`Thread.sleep()`可以把当前线程暂停一段时间。

### [线程的状态](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580742045730)

Java线程对象`Thread`的状态包括：`New`、`Runnable`、`Blocked`、`Waiting`、`Timed Waiting`和`Terminated`；

通过对另一个线程对象调用`join()`方法可以等待其执行结束；

可以指定等待时间，超过等待时间线程仍然没有结束就不再等待；

对已经运行结束的线程调用`join()`方法会立刻返回。

### [中断线程](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580767211554)

对目标线程调用`interrupt()`方法可以请求中断一个线程，目标线程通过检测`isInterrupted()`标志获取自身是否已中断。如果目标线程处于等待状态，该线程会捕获到`InterruptedException`；

目标线程检测到`isInterrupted()`为`true`或者捕获了`InterruptedException`都应该立刻结束自身线程；

通过标志位判断需要正确使用`volatile`关键字；

`volatile`关键字解决了共享变量在线程间的可见性问题。

### [守护线程](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580788183074)

守护线程是为其他线程服务的线程；

所有非守护线程都执行完毕后，虚拟机退出；

守护线程不能持有需要关闭的资源（如打开文件等）。

### [线程同步](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580844806178)

多线程同时读写共享变量时，会造成逻辑错误，因此需要通过`synchronized`同步；

同步的本质就是给指定对象加锁，加锁后才能继续执行后续代码；

注意加锁对象必须是同一个实例；

对JVM定义的单个原子操作不需要同步。

### [同步方法](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580867874849)

用`synchronized`修饰方法可以把整个方法变为同步代码块，`synchronized`方法加锁对象是`this`；

通过合理的设计和数据封装可以让一个类变为“线程安全”；

一个类没有特殊说明，默认不是thread-safe；

多线程能否安全访问某个非线程安全的实例，需要具体问题具体分析。

### [死锁](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580888846370)

Java的`synchronized`锁是可重入锁；

死锁产生的条件是多线程各自持有不同的锁，并互相试图获取对方已持有的锁，导致无限等待；

避免死锁的方法是多线程获取锁的顺序要一致。

### [使用wait和notify](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580911915042)

`wait`和`notify`用于多线程协调运行：

- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
- 必须在已获得的锁对象上调用`wait()`方法；
- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

### [使用ReentrantLock](https://www.liaoxuefeng.com/wiki/1252599548343744/1306580960149538)

`ReentrantLock`可以替代`synchronized`进行同步；

`ReentrantLock`获取锁更安全；

必须先获取到锁，再进入`try {...}`代码块，最后使用`finally`保证释放锁；

可以使用`tryLock()`尝试获取锁。

### [使用Condition](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581033549858)

`Condition`可以替代`wait`和`notify`；

`Condition`对象必须从`Lock`对象获取。

### [使用ReadWriteLock](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581002092578)

使用`ReadWriteLock`可以提高读取效率：

- `ReadWriteLock`只允许一个线程写入；
- `ReadWriteLock`允许多个线程在没有写入时同时读取；
- `ReadWriteLock`适合读多写少的场景。

### [使用StampedLock](https://www.liaoxuefeng.com/wiki/1252599548343744/1309138673991714)

`StampedLock`提供了乐观读锁，可取代`ReadWriteLock`以进一步提升并发性能；

`StampedLock`是不可重入锁。

### [使用Concurrent集合](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581060812834)

使用`java.util.concurrent`包提供的线程安全的并发集合可以大大简化多线程编程：

多线程同时读写并发集合是安全的；

尽量使用Java标准库提供的并发集合，避免自己编写同步代码。

### [使用Atomic](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581083881506)

使用`java.util.concurrent.atomic`提供的原子操作可以简化多线程编程：

- 原子操作实现了无锁的线程安全；
- 适用于计数器，累加器等。

### [使用线程池](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581130018849)

JDK提供了`ExecutorService`实现了线程池功能：

- 线程池内部维护一组线程，可以高效执行大量小任务；
- `Executors`提供了静态方法创建不同类型的`ExecutorService`；
- 必须调用`shutdown()`关闭`ExecutorService`；
- `ScheduledThreadPool`可以定期调度多个任务。

### [使用Future](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581155184674)

### [使用CompletableFuture](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581182447650)

### [使用ForkJoin](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581226487842)

### [使用ThreadLocal](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581251653666)

[Maven基础](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945359327200)

[Maven介绍](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301146648610)

[依赖管理](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301178105890)

[构建流程](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301196980257)

[使用插件](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301217951777)

[模块管理](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301243117601)

[使用mvnw](https://www.liaoxuefeng.com/wiki/1252599548343744/1305148057976866)

[网络编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945371526048)

[网络编程基础](https://www.liaoxuefeng.com/wiki/1252599548343744/1305163149082658)

[TCP编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1305207629676577)

[UDP编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1319099802058785)

[发送Email](https://www.liaoxuefeng.com/wiki/1252599548343744/1319099923693601)

[接收Email](https://www.liaoxuefeng.com/wiki/1252599548343744/1319099948859426)

[HTTP编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1319099982413858)

[RMI远程调用](https://www.liaoxuefeng.com/wiki/1252599548343744/1323711850348577)

[XML与JSON](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945389334784)

[XML简介](https://www.liaoxuefeng.com/wiki/1252599548343744/1305161429418018)

[使用DOM](https://www.liaoxuefeng.com/wiki/1252599548343744/1320414976409634)

[使用SAX](https://www.liaoxuefeng.com/wiki/1252599548343744/1320418577219618)

[使用Jackson](https://www.liaoxuefeng.com/wiki/1252599548343744/1320418596093986)

[使用JSON](https://www.liaoxuefeng.com/wiki/1252599548343744/1320418650619938)

[JDBC编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1255943820274272)

[JDBC简介](https://www.liaoxuefeng.com/wiki/1252599548343744/1305152088703009)

[JDBC查询](https://www.liaoxuefeng.com/wiki/1252599548343744/1321748435828770)

[JDBC更新](https://www.liaoxuefeng.com/wiki/1252599548343744/1321748475674658)

[JDBC事务](https://www.liaoxuefeng.com/wiki/1252599548343744/1321748500840481)

[JDBC Batch](https://www.liaoxuefeng.com/wiki/1252599548343744/1322290857902113)

[JDBC连接池](https://www.liaoxuefeng.com/wiki/1252599548343744/1321748528103458)

[函数式编程](https://www.liaoxuefeng.com/wiki/1252599548343744/1255943847278976)

[Lambda基础](https://www.liaoxuefeng.com/wiki/1252599548343744/1305158055100449)

[方法引用](https://www.liaoxuefeng.com/wiki/1252599548343744/1305207799545890)

[使用Stream](https://www.liaoxuefeng.com/wiki/1252599548343744/1322402873081889)

[创建Stream](https://www.liaoxuefeng.com/wiki/1252599548343744/1322655160467490)

[使用map](https://www.liaoxuefeng.com/wiki/1252599548343744/1322402942287906)

[使用filter](https://www.liaoxuefeng.com/wiki/1252599548343744/1322402956967969)

[使用reduce](https://www.liaoxuefeng.com/wiki/1252599548343744/1322402971648033)

[输出集合](https://www.liaoxuefeng.com/wiki/1252599548343744/1322656099991586)

[其他操作](https://www.liaoxuefeng.com/wiki/1252599548343744/1322403061825570)

[设计模式](https://www.liaoxuefeng.com/wiki/1252599548343744/1264742167474528)

[创建型模式](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319090782242)

[工厂方法](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319170474017)

[抽象工厂](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319134822433)

[生成器](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319155793953)

[原型](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319195639841)

[单例](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319214514210)

[结构型模式](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319233388578)

[适配器](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319245971489)

[桥接](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319266943009)

[组合](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319283720226)

[装饰器](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319302594594)

[外观](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319346634785)

[享元](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319417937953)

[代理](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319432618017)

[行为型模式](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319453589538)

[责任链](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319474561057)

[命令](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319491338273)

[解释器](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319508115489)

[迭代器](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319524892705)

[中介](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319541669922)

[备忘录](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319562641441)

[观察者](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319577321505)

[状态](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319592001569)

[策略](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319606681634)

[模板方法](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319636041762)

[访问者](https://www.liaoxuefeng.com/wiki/1252599548343744/1281319659110433)

[Web开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945497738400)

[Web基础](https://www.liaoxuefeng.com/wiki/1252599548343744/1304265903570978)

[Servlet入门](https://www.liaoxuefeng.com/wiki/1252599548343744/1304265949708322)

[Servlet开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1266264743830016)

[Servlet进阶](https://www.liaoxuefeng.com/wiki/1252599548343744/1328705066500130)

[重定向与转发](https://www.liaoxuefeng.com/wiki/1252599548343744/1328761739935778)

[使用Session和Cookie](https://www.liaoxuefeng.com/wiki/1252599548343744/1328768897515553)

[JSP开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1266262958498784)

[MVC开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1266264917931808)

[MVC高级开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1337408645759009)

[使用Filter](https://www.liaoxuefeng.com/wiki/1252599548343744/1266264823560128)

[修改请求](https://www.liaoxuefeng.com/wiki/1252599548343744/1328976435871777)

[修改响应](https://www.liaoxuefeng.com/wiki/1252599548343744/1328976456843298)

[使用Listener](https://www.liaoxuefeng.com/wiki/1252599548343744/1304266123771937)

[部署](https://www.liaoxuefeng.com/wiki/1252599548343744/1304266260086817)

[Spring开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1266263217140032)

[IoC容器](https://www.liaoxuefeng.com/wiki/1252599548343744/1266265100383840)

[IoC原理](https://www.liaoxuefeng.com/wiki/1252599548343744/1282381977747489)

[装配Bean](https://www.liaoxuefeng.com/wiki/1252599548343744/1282382145519649)

[使用Annotation配置](https://www.liaoxuefeng.com/wiki/1252599548343744/1282382596407330)

[定制Bean](https://www.liaoxuefeng.com/wiki/1252599548343744/1308043627200545)

[使用Resource](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383017934882)

[注入配置](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383225552930)

[使用条件装配](https://www.liaoxuefeng.com/wiki/1252599548343744/1308043874664482)

[使用AOP](https://www.liaoxuefeng.com/wiki/1252599548343744/1266265125480448)

[装配AOP](https://www.liaoxuefeng.com/wiki/1252599548343744/1310052352786466)

[使用注解装配AOP](https://www.liaoxuefeng.com/wiki/1252599548343744/1310052317134882)

[AOP避坑指南](https://www.liaoxuefeng.com/wiki/1252599548343744/1339039378571298)

[访问数据库](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383540125729)

[使用JDBC](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383699509281)

[使用声明式事务](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383642886177)

[使用DAO](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383605137441)

[集成Hibernate](https://www.liaoxuefeng.com/wiki/1252599548343744/1266263275862720)

[集成JPA](https://www.liaoxuefeng.com/wiki/1252599548343744/1282383789686817)