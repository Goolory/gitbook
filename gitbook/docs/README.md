## 📚C++

### 🏷const

**作用**

1、修饰变量，说明该变量不可以被改变

2、修饰指针，分为指向常量的指针（pointer to const 如：const int * p ）const在 * 之前，不能通过 * P来修改所指向的内容去的内容；常量指针，简称常指针：int * const p, * 在const之前，不能改变指向的位置

```c++
char g[] = "hello";
const char* p1 = g;  //指针变量，不能通过*p1 改变内容，可以改变位置
char * const p2 = g;  //常量指针，不能改变指针指向的位置；可以改变内容
```

3、修饰引用，指向常量的应用，用于形参类型，避免了拷贝，又避免了函数对值得修改；

```c++
void function(const int& var);
```

4、修饰成员函数，该成员函数内不能改变成员变量。

```c++
class A{
 int func() const; 
}
```

### 🏷 static

| static作用       | 存储区 | 初始化     | 作用域                                                       | 备注                                                         |
| ---------------- | ------ | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义全局静态变量 | 静态区 | 默认初始化 | 仅在声明它的文件中可见，不被其他文件所用                     | 在main函数执行前就分配了空间                                 |
| 局部静态变量     | 静态区 | 默认初始化 | 局部作用域                                                   | 当执行离开局部作用域后，该变量没有被销毁，任然驻留在内存中。 |
| 静态函数         |        |            | 仅在声明它的文件中可见，不被其他文件所用                     | 不要在头文件中声明static的全局函数，<br />不要在cpp内声明非static的全局函数.<br />如果要在多个cpp中重复使用该函数，就把它声明到头文件中<br />否则cpp内部声明应该加上static修饰 |
| 类的静态成员     |        |            |                                                              | 静态成员是类的所有对象中共享的成员，而不是某个对象的成员.<br />静态数据成员只存储一处，对所有对象共用。 |
| 类的静态函数     |        |            | 静态函数与静态成员一样，<br />不是对象成员，引用时不需要对象名 | 引用格式：<类名>::<静态成员函数名>(<参数>);                  |

### 🏷 extern

1、可以置于变量或者函数前，以声明变量或者函数的定义在别的文件中，提示编译器在遇到次变量和函数时在其他模块中寻找其定义

2、链接指定，当他和"C"一起连用时，`extern "c" void fun(int a, int b)`;这 告诉编译器在编译这个fun按照c的规则去翻译函数名

与include相比，extern引用另一个文件的范围小，include可以引用另一个文件的全部内容

### 🏷 explicit关键字

C++中的explicit关键字只能修饰只有一个参数的类构造函数，作用是表名该构造函数是显示的，而非隐式的，跟它对应的另一关键字是implicit，意思是隐藏的，类的构造函数默认下即声明为implicit

参考https://blog.csdn.net/guoyunfei123/article/details/89003369

### 🏷this指针

1、`this` 指针是一个隐含于每一个非静态成员函数中的特殊指针。它指向调用该成员函数的那个对象

2、当对一个对象调用成员函数时，编译程序先将对象的地址赋值给 `this` 指针，然后调用成员函数，每次成员函数存取数据成员时，都隐式的使用 `this` 指针。

3、当一个成员函数被调用时，自动向它传递一个隐含的参数，该参数指向这个成员函数所在对象的指针。

4、`this` 指针被隐含的声明为： `ClassName *const this` ,这意味着不能给 `this` 指针赋值；在 `ClassName` 类的 `const` 成员中， `this` 指针的类型为 `const ClassName* const` ,这说明不能对 `this` 指针所指向的这种对象是不可修改的

5、 `this` 并不是常规变量，而是个右值，所以不能取得 `this` 的地址（不能 `&this` )

6、在以下场景中，经常需要显示的应用 `this` 指针：

​	1、为实现对象的链式引用；

​	2、为了避免对同一对象进行赋值操作；

​	3、实现一些数据结构时，如：list

### 🏷 inline 内联函数

* 相当于把内联函数里的内容写在调用内联函数处
* 相当于不用执行进入函数的步骤，直接执行函数体；
* 相当于宏，却比宏多了类型检查，真正具有函数特性；
* 内联一般不包含循环、递归等复杂操作
* 在类声明中定义的函数，除了虚函数其他函数都会自动隐式地当成内联函数。

####  编译器对inline函数的处理步骤

1、将inline函数体复制到inline函数调用点处；

2、为所用inline函数中的局部变量分配内存空间；

3、将inline函数的输入参数和返回值映射到调用方法的局部变量空间中；

4、如果inline函数有多个返回点，将其转变为inline函数代码块末尾的分支（使GOTO）

#### 优缺点

**优点**

1、内联函数和宏函数一样将在被调用处进行代码展开，省去了参数压栈，栈帧开辟与回收，结果返回等，从而提高程序的运行速度；

2、内联函数相比宏函数来说，在代码展开时，会做安全检查或自动类型转换，而宏定义不会；

3、在类中声明的同时定义的成员函数，会自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义不行

4、内联函数在运行时可以调试，而宏定义不可以

**缺点**

1、代码膨胀💥。内联是一代码膨胀为代价，消除函数调用带来的开销。

2、inline函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不行on-inline可以直接链接

#### 虚函数（virtual）可以是内联函数吗？

* 虚函数可以是内联函数，内联可以修饰虚函数的，但是当虚函数表现多态性时不能内联
* 内联是在编译期内联，而虚函数的多态性是在运行期。编译器无法知道运行期调用哪个代码，因此虚函数表现为多态性时不可内联。
*  `inline virtual`  唯一内联的时候是：编译器知道调用的对象是哪个类 （如 `Base::who()` ,这只有在编译器具有实际对象而不是对象的指针或引用时才会发生。

``` c++
#include<iostream>
using namespace std;
class Base
{
  public:
    inline virtual void who()
    {
      cout << "kkk\n";
    }
    virtual ~Base(){}
};
class Derived : public Base
{
  public:
    inline void who()  // 不写inline时隐式内联
    {
      cout << "kkk";
    }
};
int main()
{
  Base b;
  b.who();  // 此处的虚函数是who()，是通过类Base的具体对象b来调用，编译器就能确定了
  
  // 此处的虚函数是通过指针调用的，呈多态性，需要在运行时期才能确定，所以不能内联
  Base* ptr = new Derived();
  ptr->who();
  return 0;
}
```

### 🏷 assert()

断言，是宏，而非函数。assert宏的原型定义在 `<assert.h>` c语言 -- `<cassert>` C++

 作用：如果他的条件返回错误，则终止程序执行。可以通过定义 `NOEBUG` 来关闭，需要定义在 `include<assert.h>`  之前 

```c++
#define NDEBUG
#include<assert.h>
assert(p != nullptr);
```

### 🏷 sizeof()

* sizeof对数组，得到整个数组占空间大小

* sizeof对指针，得到指针本身大小

  ​	sizefof(空类) = 1；[类的大小——sizeof 的研究(1)](https://www.jianshu.com/p/5163a2678171)

  ​	因为一个空类也要实例化，所谓类的实例化就是在内存中分配一块地址，每个实例在内存中都有独一无二的的地址。同样空类也会被实例化

### 🏷 C/C++程序内存存储区域

1、**内存栈区**：存放函数参数，函数返回地址，函数内部变量，函数一些寄存器（由编译器自动分配释放）

2、**内存堆区**：存放new或malloc出来的对象（程序员分配释放）

3、**常数区**：存放局部常量和全局常量的值，函数指针

4、**静态区**：存放全局变量或者静态变量，虚函数表

5、**代码区**：二级制代码

c/c++不提供垃圾回收机制，因此需要对堆中的数据进行及时的销毁，防止内存泄漏，使用 `free, delete` 

### 🏷 野指针与悬空指针

野指针：wild pointer没有经过初始化的指针

悬空指针：最初指向的内存已经被释放了的指针

**危害** ：无论是野指针还是悬空指针，都是 *指向无效内存区域的指针*。访问“不安全可控”（invalid）的内存区域 <font color="red">导致“Undefined Behavior</font> 

#### 内存泄漏

简单地说就是申请了一块内存空间，使用完毕后没有释放掉。它的一般表现方式是程序运行时间越长，占用内存越多，最终用尽全部内存，整个系统崩溃。

由程序申请一块内存，没有指针指向它，那么这块内存就泄漏了

**如何避免内存泄漏**;

1、良好的编码习惯

2、MAT工具监测

### 🏷 new/malloc和free/delete

**首先 new对应的是delete-C++中使用， malloc对应free- c语言使用。free用来释放malloc出来的动态空间，delete用来释放new出来的空间**

**new与malloc区别**

1、new 和malloc都是在堆上开辟内存的

​	malloc只负责开辟内存，没有初始化功能，需要用户自己初始化；new 不但可以开辟内存，还可以进行初始化。

2、malloc是函数，开辟内存需要传入字节数，如 `malloc(100)` 表示在堆上开辟了100个字节的内存，返回void *，表示分配的堆内存的起始位置，因此malloc的返回值需要强制转换为所需类型；new是运算符，开辟的内存需要指定类型，返回指定类型的地址，无需强转

3、 malloc开辟内存失败返回NULL，new 开辟失败返回bad_alloc类型的异常，需要捕获异常才能判断内存是否开辟成功，new 运算符其实operator new 函数的调用，它的底层也是malloc来开辟内存，new它比malloc多的功能就是初始化功能，对应类型来说，就是调用构造函数

4、malloc用free释放，new用delete释放

#### malloc、calloc、realloc、alloca

1、malloc：申请指定字节数内存。申请到的内存中的初始值不确定

2、calloc：为指定长度的对象，分配能够容纳指定个数的内存。申请到的内存的每一bit都初始化为0

3、realloc：更改以前分配的内存长度（增加或减少）。当增加长度时，可能需将以前分配区的内容移动另一个足够大的区域，而新增区域内的初始化值则不确定

4、alloca：在栈上申请内存。程序在出栈的时候，会自动释放内存。但是需要注意的是，alloca不具备可移植性。

### 🏷 #pragma pack(n)

设定结构体、联合体以及类成员变量以n字节方式对齐

```c++
#pragma pack(push) //保存对齐状态
#pragma pack(4)  //设定为4字节对齐
static test
{
  char m1;
  double m4;
  int m3;
};
#pragma pack(pop)  // 恢复对齐状态 
```



### 🏷 c与C++区别

设计思想上:

C++是面向对象的语言，而C是面向过程的结构化编程语言

语法上：

C++具有封装、继承、多态性

C++与C相比，增加了许多类型安全功能，比如强制类型转换

C++支持范式编程，如模板类，函数模板等

### 🏷 cast转换

C++中四种类型转换： [原文](https://www.jianshu.com/p/5163a2678171)

1、const_cast

格式：`const int b = 0; int * p = const_cast<int* > (&b);`

用于将const变量转为非const

2、static_cast

static_cast的转换格式： `static_cast <type-id> (expression)`

a. 基本数据类型之间的转换，如int 转char

举例：

```c++
short value = 64;
int val = static_cast<int> (value);
// short是2个字节，int是4个字节，将short转成int之后，高位根据符号位补满
```

b. 用于类层次结构中，基类和子类之间的指针和引用的转换

> 当进行上行转换，子类的指针或引用转成父类表示，这是安全的
>
> 当进行下行转换，父类的指针或引用转成子类表示，这是不安全的，需要程序员保证

c. 用于各种隐式转换，比如非const转const, void*转指针等。

3、dynamic_cast

用于动态类型转换。只能用于含有虚函数的类，用于类层次间的向上或向下转换。只能转指针或引用。向下转化时，如果是非法的对于指针返回NULL，对于引用抛出异常

它通过判断在执行到该语句的时候变量的运行时类型和要转换的类型是否相同来判断是否能够进行转换

4、reinterpret_cast

几乎什么都可转，比如int转指针

### 🏷 指针与引用

1、指针有自己的一块空间，而引用只是一个别名

2、使用sizeof看，一个指针的大小是4，而引用则是被引用对象的大小

3、指针可以被初始化为NULL，而引用必须被初始化一个已有对象的引用

4、作为参数传递时，指针需要被解引用才可以进行对对象的操作，而直接对引用的修改会改变引用指向的对象；

5、指针在使用中可以指向其他的对象，但是引用只能对一个对象的引用

6、指针可以有多级指针（** p) ，而引用只有一级

7、指针和引用使用++运算符的意义不一样

8、如果返回多态内存分配对象或内存，必须使用指针，引用可能引起内存泄漏

### 🏷 C++中struct和class

总的来说，struct更适合看做一个数据结构的食堂，class更适合看成一个对象的实体

在C++中可以使用struct和class定义类，都可以继承。

区别在于:struct的默认继承权限和默认访问权限是public；

class的默认继承权限和默认访问权限是private

另外， class还可以定义模板类形参，如 `template<class T, int i>`

### 🏷 类成员的访问权限

1、public：类中、类外可以访问

2、protected：子类

3、private：成员函数可以访问

### 🏷 union联合

union是一种节省空间的特殊的类，一个union可以有多个数据成员，但是对外只表示一个数据成员。当某个数据成员被赋值后其他成员变成未定义状态。联合特点

* 默认访问控制符是public
* 可以包含构造函数、析构函数
* 不能含有引用类型的成员
* 不能继承、和被继承
* 匿名union在定义所在作用域可以直接访问成员
* 不能包含protected和private成员
* 全局匿名联合必须是静态（static）的

```c++
#include<iostream>
union UnionTest{
  UnionTest():i(10){};
  int i;
  double d;
};

static union{
  int i;
  double d;
};
int main()
{
  UnionTest u;
  union {
    int i;
    double d;
  };
  std::cout << u.i << std::endl; // 输出UnionTest的10
  ::i = 20;
  std::cout << ::i << std::endl; //输出全局静态匿名联合20
  i= 30;
  std::cout << i << std::endl;  //30
  return 0;
}
```

### 🏷 友元

友元关系不具有对称性。即A是B的友元，但B不一定是A的友元。友元关系也不具有传递性，即B是A的友元，C是B的友元，但C不一定是A的友元

### 友元函数

特点：友元函数是能够访问类中的私有成员的非成员函数。友元与普通函数一样，即在定义上和调用上与普通函数一样。

```c++
#include<iostream>
#include<cmath>
using namespace std;
class Point
{
  public:
    Point(double xx, double yy)
    {
      x = xx;
      y = yy;
    }
  void Getxy();
  friend double Distance(Point &a, Point &b);
  private:
    double x, y;
};
void Point::Getxy()
{
  cout<<x<<y<<endl;
}
double Distance(Point &a, Point &b)
{
  double dx = a.x - b.x;
  double dy = a.y = b.y;
  return sqrt(dx*dx + dy*dy);
}

int main(void)
{
  Point P1(3.0, 4.0), p2(6.0, 8.0);
  double d = Distance(p1, p2);
  cout << d;
  return 0;
}
```

在该程序中的Point类中声明了一个友元函数Distance()，函数名前加friend关键字。但是他的定义与普通函数是一样的，不需要指出所属类，但是它可以引用类中的私有成员。

#### 友元类

定义：友元除了函数以为，还可以是类，即一个类作为另一个类的友元。当一个类作为另一个类的友元时，这就意味着这个类的所有成员函数都是另一个类的友元函数，都可以访问另一个类的隐藏信息（包括私有成员和保护成员）

### 🏷 using 

**using 声明**：

一条 `using声明` 语句一次只能引入命名空间的一个成员。它使得我们可以清楚知道程序中所引用的到底是哪个名字， 如 `using namespace_name::name`

**构造函数的using声明**

在C++11 中，派生类能够重用其直接基类定义的构造函数

```c++
class Derived::Base
{
  public:
    using Base:base;
};
```

如上using声明，对应基类的每个构造函数，编译器都生成一个与之对应（形参列表完全相同）的派生类构造函。生成如下类型构造函数：

```c++
Derived(parms) : Base(args){}
```

**using指示**

使得某个特定的命名空间中所有名字都可见，这样我们无需再为它们添加任何前缀限定符了。

```c++
using namespace std;
```

尽量少使用using指示污染命名空间

> 一般，使用using命令比使用using编译命令更安全，这是由于它只导入了指定的名称。如果该名称与局部名称发送冲突，编译器会发出提示。using编译命令导入所有的名称，包括可能并不使用的名称。如果与局部名称发生冲突，则局部名称将覆盖名称空间版本，而编译器不会发出警告 。另外，名称空间的开放性意味着名称空间的名称可能分散在多个地方，这使得难以准确地知道添加了哪些名称。

少使用using指示

```c++
using namespace std;
```

多使用using声明

```c++
int x;
std::cin >> x;
```

或者

```c++
using std::cin;
using std::cout;
using std::endl;
int x;
cin >>x;
cout <<x<<endl;
```

### 🏷 :: 范围解析运算符

**分类**

1、全局作用域符(::name)：用于类型名称（类，类成员、成员函数、变量等），表示作用域为全局命名空间

2、类作用域（class::name) : 用于表示指定类作用域范围是具体某个类

3、命名空间作用域符（namespace::name)：用于表示指定类型的作用域范围是具体某个命名空间

```c++
int count = 11;  
class A{
  public: static int count;
};
int A::count = 21;
void fun()
{
  int count = 31;
  count = 32;
}

int main()
{
  cout<<::count;  //11
  A::count;  //21
  return 0;
}
```

### 🏷 enum枚举类型

限定作用域的枚举类型

```c++
enum class ioen{input, output, apppend};
```

不限定作用域的枚举类型

```c++
enum color (red, yellow, green);
enum {floatPrec = 6, doublePrec = 10};
```

### 🏷 decltype

作用于检查实体的声明类型或表达式的类型以及值分类

```c++
//尾置返回允许我们在参数列表之后声明返回类型
template<typename It>
auto fun(It beg, It end)->decltype(*beg)
{
  //处理序列
  return *beg; //返回序列中的一个元素的引用
}
//为了使用模板参数成员必须使用typename
template<typename It>
auto f(It beg, It end)->typename remove_reference<decltype(*beg)>>::type
{
  return *beg;  // 返回一个元素的拷贝
}
```

### 🏷 引用

右值：当一个对象被当做右值时，用的是对象的内容，**右值要么是字面常量，要么是在表达式求值过程中创建的对象**

左值：当一个对象被当做左值时，**用的是对象的身份**

右值引用（&&）

左值引用（&）

```c++
int i = 32;
int &r = i;             //正确，r引用i
int &&rr = i;           //错误，不能将一个右值引用到左值上
int &r2 = i*32;         //错误，i*32是一个右值
const int &r3 = i*32;   //正确，可以将一个const引用绑定到一个右值上
int &&r4 = i*32;       //正确，将r4绑定到右值
```

### 🏷 智能指针

智能指针的作用是管理一个指针，解决因申请空间而忘记释放，造成内存泄漏的问题。

因为智能指针是一个类，当超出类的作用域时，类会自动调用析构函数，释放资源。不需要手动释放

1、auto_ptr(C++98的方案，cpp11弃用)

采用所有权模式。

```c++
auto_ptr<string> p1 (new string ("safasdf"));
auto_ptr<string> p2;
p2 = p1; // 不会报错
```

此时p2剥夺了p1的所有权，但是当程序运行时访问p1会报错。所以存在内存崩溃的问题。

2、unique_ptr（替换auto_ptr)

unique采用独占式拥有或严格拥有概念，保证同一时间内只有一个智能指针可以指向该对象，它对于避免资源协议特别有用

```c++
auto_ptr<string> p1 (new string ("safasdf"));
auto_ptr<string> p2;
p2 = p1; // 会报错
```

另外unique_ptr还有更聪明的地方，当程序视图将一个unique_ptr赋值给另一个是，如果员unique_ptr是一个临时右值，则允许这么做。如果原unique_ptr存在一段时间，编译器禁止

```c++  
unique_ptr<string> p1 (new string("adf"));
unique_ptr<string p2;
p2 = p1;   //not allowed
unique_ptr<string> p3;
p3 = unique_ptr<string>(new string("sdf"));   //allowed
```

3、shared_ptr

实现共享式坐拥。多个智能指针可以指向相同对象，该对象和其相关资源会在”最后一个引用被销毁时“释放。它使用计数机制来表明资源被几个指针共享。可以通过成员函数use_count()来查看资源的所有者个数。除了可以通过new来构造，还可以通过传入auto_ptr, unique_ptr, weak_ptr来构造。当我们调用release()时，当前指针会释放资源所有权，计数器减一，当计数器等于0时，资源被释放

Share_ptr是为了解决auto_ptr在对象所有权上的局限性

**成员函数**：

use_count返回引用计数个数

unique返回是否独占所有权（use_count=1)

swap交换两个shared_ptr对象

reset释放内部对象的所有权或拥有对象的变更，会引起原有对象的引用计数的减少

get返回内部对象（指针）。

4、weak_ptr

是一个不控制对象生命周期的智能指针。指向一个shared_ptr管理的对象，进行该对象的内存管理的是那个shared_ptr，weak_ptr只是提供对管理对象的一个访问手段。weak_ptr设计的目的是为了配合shared_ptr工作。它只可以从一个shared_ptr或另一个weak_ptr对象构造，它的构造和析构都不会引起计数的变化。

weak_ptr是用来解决shared_ptr相互引用引起的死锁问题。如果两个shared_ptr指针相互引用，那么两个指针的引用计数永不归零，资源永不释放。它是对对象的一种若引用。

```c++
class B;
class A
{
  public:
      shared_ptr<B> pb_;
      ~A();
};
class B
{
  public:
      shared_ptr<A> pa_;
      ~B();
};
void func()
{
  shared_ptr<B> pb(new B());
  shared_ptr<A> pa(new A());
  pb->pa_ = pa;
  pa->pb_ = pb;
}
int main()
{
  fun();
  return 0;
}
```

fun中pa,pb相互引用



