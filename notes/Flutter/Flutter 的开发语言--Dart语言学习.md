### 一. 前言

Flutter使用的是Dart语言，我们先来了解Dart的一些基础特性，便于后面Flutter的开发。

### 二. 变量声明
举例：
```
 var name = '小明';
 var age = 18;

 String nameStr = "小红";
 int ageInt = 18;

 dynamic address = '北京';
   
 Object money = 100;
```
总共有四种：

1. **var**
```
var name = '小明';
var age = 18;
```
使用 **var** 来声明变量，不需要特别指定变量的数据类型。
> 注意：为什么 **var** 声明的变量不需要指定数据类型，而能定义所有的变量呢？  
因为 **var** 存储的是值的对象的引用，而不是直接存储的值。

2. 明确的数据类型
```
String nameStr = "小红";
int ageInt = 18;
```
在声明变量的时候，就使用明确的数据类型。
>Dart 支持以下的数据类型：
- int：**整数，范围为 -2^63 到 2^63 - 1。比如：int x = 1；
- double：**浮点数，64位（双精度）浮点数。比如：double y = 1.1111；
- num：**是数字类型，既可以表示整数，也可以表示浮点数，具体看赋的值。比如：num x = 1;//x是整数，num y = 1.1111;//y是浮点数；
- String：**字符串，Dart 字符串采用UTF-16编码，可以使用单引号或双引号来创建字符串。比如：String s = 'test' 或 String = “test”；
- bool：**布尔值。比如：bool isBoy = true；
- List、Set、Map**
- Runes：**表示采用 UTF-32 的字符串，用于显示 Unicode 因为 Dart 字符串是 UTF-16，所以需要 Runes 这个特殊语法转换一下。

3. **dynamic**
 ```
dynamic address = '北京';
```
**dynamic** 表明数据类型是动态可变的。它和 **var** 一样，可以定义任何变量，但是不同的是，**var** 一旦赋值后，就不能改变数据类型了，但是 **dynamic** 可以，比如：
如果 **var** 这样使用，就会有编译错误:
```
var a = 'test';
a = 1;
```
如果 **dynamic** 能够通过编译，但是会在运行时报错:
```
dynamic a = "test";
a = 1;
```

4. **Object**
```
Object money = 100;
```
**Dart** 里所有东西都是对象，是因为 **Dart** 的所有东西都继承自 **Object**，因此 **Object** 可以定义任何变量，并且赋值后，也可以改变类型。
```
object a = 1;
a = "test";
```
>注意：一般都用Object代替dynamic，而不使用dynamic。

### 三. 修饰常量用的：**final** 和 **const**
在用 **final** 和 **const** 修饰不想改变的值时，需要注意一下几点：
- 使用 **final** 和 **const** 的时候可以把 **var** 省略
- **final** 和 **const** 变量只能赋值一次，而且只能在声明的时候就赋值
- **const** 是隐式的 **final**，在使用 **const** 的时候，如果变量是类里的变量，必须加 **static**，是全局变量时不需要加
>**final** 和 **const** 区别：  
**const** 是编译时常量，在编译时就初始化，值就确定了。  
**final** 是当类创建的时候才初始化。  

### 四. List、Set 和 Map
**List**  
```
//使用构造函数创建对象
var list = List<int>();
list.add(1);
list.add(2);

//通过字面创建对象，list的泛型参数可以从变量定义推断出来
var list2 = [1,2,3];

//没有元素，显示指定泛型参数为int
var list3 = <int>[];
list3.add(1);
list3.add(2);

var list4 = const[1,2];
//list4 指向的是一个常量，不能给它添加元素。也不能修改它
list4.add(3);//error
//list4 本身不是一个常量，所以它可以指向另一个对象
list4 = [4,5];//ok
```
**Set**  
Set<E>，E 表示 Set 里的数据类型，用大括号来赋值：
```
Set<String> set = {"aaa","bbb","cccc"};

var set = Set<String>();
      set.add('aaa');
      set.add('bbb');
```
**Map**  
```
//第一种写法：
Map map = new Map<String,int>();
//添加
map['a'] = 1;
map['b'] = 2;
//修改
map['a'] = 3;
      
//获取map值
int i = map['a'];


//第二种写法：
 Map<String,String> map2 = {
        "a":"aaa",
        "b":"bbb",
        "c":"ccc"
      };
```
### 五. 操作符
主要分为一下几类：
1. 算术运算符
2. 比较操作符
3. 类型判断符
4. 赋值操作符
5. 逻辑运算符
6. 按位与移位运算符
7. 条件运算符
8. 级联操作符
9. 其他操作符

 **1. 算术操作符**
- +：加
- -：减
- *：乘
- %：取余
- -var：负数
- ++/--：加1/减1
以上这些都和 Java 中的一样，不同的是 **除**：
- /：除，精确除法。比如var a = 3 / 2；结果 a 为 1.5
- ~/：整除。比如var a = 3 ~/ 2；结果 a 为 1

 **2. 比较操作符**
==、!=、>、<、>=、<= 这些比较操作符和 Java 中一样

 **3. 类型判断符**
- as：类型转换。注意：转换的对象不能为null。
- is：判断是否是某个类型，如果是的话，就返回 true。
- is!：判断是否不是某个类型，如果不是的话，就返回 true。

 **4. 赋值操作符**
- =：赋值操作符
- ??=：只有当变量为空的时候才能赋值

 **5. 逻辑运算符**
!、||、&& 这些逻辑符和 Java 一样

 **6. 按位与移位运算符**
- &：按位与，对于每一个比特位，只有两个操作数相应的比特位都是1时，结果才为1，否则为0
- |：按位或，对于每一个比特位，当两个操作数相应的比特位至少有一个1时，结果为1，否则为0
- ^：按位异或，对于每一个比特位，当两个操作数相应的比特位有且只有一个1时，结果为1，否则为0
- ~：按位非，反转操作数的比特位，即0变成1，1变成0
- <<：左移
- **>>**：右移

 **7. 条件运算符**
- "? :"： 同 Java 中 if else
- var1 ?? var2：如果 var1 为null，就返回 var2，否则返回 var1

 **8. 级联操作符**
- ..：允许你对同一对象进行一系列的操作。

 **9. 其他操作符**
- {}：函数调用
- []：访问列表
- .：访问成员变量
- ?. ：有条件的成员变量访问
### 六. 语句
在Java中常用的 **if else**，**switch**，**while** 和 **do while** 在 Dart 里面都支持。
### 七. 函数
在 Dart 中函数也是对象，函数的类型是 **Function**。
模版格式：
```
返回类型 函数名(函数参数){

}
```
**函数的参数：必选参数和可选参数**
- 必选参数是必填的
- 可选参数是选填的

**必须参数**
必选参数就是平时的方法定义的函数参数，比如：
```
 bool(String name,int age){
    
  }
```
**可选参数**
分为两类：
- 可选命名参数：使用 **{}** 包起来的参数是可选命名参数
- 可选位置参数：使用 **[]** 包起来的参数是可选位置参数
1. 可选命名参数 **{}**
可选参数的赋值必须是 **key : value** 这种格式，比如：
```
bool(String name,{int age,int id}){

  }
```
这里参数 age 就是可选命名参数。
同时还可以给命名参数加 **@required**，意思是这个也是必填参数。
```
bool(String name,{@required int age,int id}){

  }
```
2. 可选位置参数：**[]**
赋值和参数是一一对应的。
```
bool('小明',"123456");//不对，他是有顺序的
```
### 八. lambda表达式
> => 语句后面只能跟一行代码，而且这一行代码只能是一个表达式，而不能跟语句。表达式可以是函数、值。
```
void main() => runApp(MyApp());
```
等价于
```
void main(){
    return runApp(MyApp());//runApp() 返回的是 void
}
```
### 九. 异常
抛出异常：
```
throw Exception('put your error message here');
```
捕获异常：
```
try { 
   // ...
  // 捕获特定类型的异常
} on FormatException catch (e) { 
  // ...
 // 捕获特定类型的异常，但不需要这个对象
} on Exception {  
 // ..
 // 捕获所有异常
} catch (e) { 
 // ...
} finally { 
 // ...
}
```
跟 Java 不同的是，Dart 可以抛出任意类型的对象。
### 十. 类
Dart 中每个对象都是一个类的实例，所有类都继承自 Object。
```
class Test{
  int x;
  int y;

  Test(int x,int y){
    this.x = x;
    this.y = y;
  }
}
```
默认构造函数的写法就是使用类名作为函数名的构造函数，Dart 还有更简洁的写法，
```
Test(this.x,this.y);
```
另外，在创建实例的时候，可以不使用 **new**。
```
Test test = Test(1, 2);
```
### 总结
Dart 的简单学习到此就结束了，我们主要学习了变量声明、**final** 和 **const**、**List** **Set**和**Map**、操作符、语句、函数、lambda 表达式和异常。如果想要了解更多的 Dart 语法，可以去看官方文档（https://dart.dev/guides/language/language-tour）。
