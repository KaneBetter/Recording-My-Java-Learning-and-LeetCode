# Java基础语法

# 数据类型转换、运算符、方法入门

## 数据类型转换

byte、short、char‐‐>int‐‐>long‐‐>float‐‐>double

## 运算符

- 算数运算符
- 赋值运算符
- 比较运算符
- 逻辑运算符
- 三元运算符

## 方法入门

- 方法:就是将一个功能抽取出来，把代码单独定义在一个大括号内，形成一个单独的功能。

```java
修饰符 返回值类型 方法名 (参数列表){ 代码...
return ; }
//修饰符: 目前固定写法 public static 。
//返回值类型: 目前固定写法 void ，其他返回值类型在后面的课程讲解。 
//方法名:为我们定义的方法起名，满足标识符的规范，用来调用方法。 
//参数列表: 目前无参数， 带有参数的方法在后面的课程讲解。 
//return:方法结束。因为返回值类型是void，方法大括号内的return可以不写。

 public static void methodName() { System.out.println("这是一个方法");
}
```

- 方法必须定义在一类中方法外 
- 方法不能定义在另一个方法的里面

> s=s+1进行两次运算 ， += 是一个运算符，只运算一次，并带有强制转换的特点， 也就是说 s += 1 就是 s = (short)(s + 1) ，因此程序没有问题编译通过，运行结果是2.

> b4 = b2 + b3 ， b2 和 b3 是变量，变量的值是可能变化的，在编译的时候，编译器javac不确定b2+b3的结果是什 么，因此会将结果以int类型进行处理，所以int类型不能赋值给byte类型，因此编译失败。

# 流程控制语句

- if else判断语句 
- switch选择语句 
- for循环语句 
- while循环语句

- do while循环语句 
- 跳出语句break，continue

## switch

```java
public static void main(String[] args){
	int i =5;
	switch(i){
		case 0:
			System.out.println("执行case0");
			break;
     case 1:
			System.out.println("执行case1");
			break;
	}
}
```

## for

```java
public static void main(String[] args){
	for(int x =0;x < 10; x++){
    System.out.println("Helloworld"+x)
  }
}
```

## while

```java
public static void main(String[] args) { 
   //while循环实现打印10次HelloWorld //定义初始化变量
	int i = 1;
	//循环条件<=10 
  while(i<=10){
		System.out.println("HelloWorld"); //步进
	i++;
	} 
}
```

# IDEA、方法

## 快捷键

快捷键 功能
 Alt+Enter 导入包，自动修正代码
 Ctrl+Y 删除光标所在行
 Ctrl+D 复制光标所在行的内容，插入光标位置下面 Ctrl+Alt+L 格式化代码
 Ctrl+/ 单行注释
 Ctrl+Shift+/ 选中代码注释，多行注释，再按取消注释 Alt+Ins 自动生成代码，toString，get，set等方法 Alt+Shift+上下箭头 移动当前代码行

```java
修饰符 返回值类型 方法名(参数列表){ 
	//代码省略...
	return 结果; }

修饰符: public static 固定写法
返回值类型: 表示方法运行的结果的数据类型，方法执行后将结果返回到调用者 参数列表:方法在运算过程中的未知数据，调用者调用方法时传递 return:将方法执行后的结果带给调用者，方法执行到 return ，整体方法运行结束
```

## 定义方法

- 需求:定义方法实现两个整数的求和计算。

- **明确返回值类型**:方法计算的是整数的求和，结果也必然是个整数，返回值类型定义为int类型。 
- **明确参数列表**:计算哪两个整数的和，并不清楚，但可以确定是整数，参数列表可以定义两个int类型的 变量，由调用者调用方法时传递

# 数组