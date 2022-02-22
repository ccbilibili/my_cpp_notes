# 侯捷C++面向对象（上）

## 头文件与类的声明

- 类的经典分类：不带指针（比如复数）和带指针（比如string）

- C++代码的基本形式

  ![image-20211218233845722](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211218233845722.png)

- 头文件中的防卫式声明：（写的任何一个头文件都应该加这么一个防卫式声明）

  ```c++
  #ifndef __COMPLEX__
  #define __COMPLEX__
  ...
  ...
  #endif
  ```

  第一次引用才定义，定义过则不过重复引用，就不再进入...那一部分了。也可以避免包含头文件时的顺序问题。

  ![image-20211219001419728](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219001419728.png)

- 头文件的布局：

  ![image-20211219001945401](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219001945401.png)

- class的声明（declaration）

  ![image-20211219162531455](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219162531455.png)

class template ( 模板)

![image-20211219002554215](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219002554215.png)

- 内联函数（inline)

函数在class body内完成，运行较快，尽量选择这种方式（除非函数太复杂，没法内联）。但最后是否真的inline是由编译器决定的。

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222172727744.png" alt="image-20211222172727744" style="zoom:50%;" />

[C++](http://c.biancheng.net/cplus/) 用 inline 关键字较好地解决了函数调用开销的问题。

在 C++ 中，可以在定义函数时，在返回值类型前面加上 inline 关键字。如：

```C++
inline int Max (int a, int b)
{
    if(a >b)
        return a;
    return b;
}
```

增加了 inline 关键字的函数称为“内联函数”。内联函数和普通函数的区别在于：当编译器处理调用内联函数的语句时，不会将该语句编译成函数调用的指令，而是直接将整个函数体的代码插人调用语句处，就像整个函数体在调用处被重写了一遍一样。

有了内联函数，就能像调用一个函数那样方便地重复使用一段代码，而不需要付出执行函数调用的额外开销。很显然，使用内联函数会使最终可执行程序的体积增加。以时间换取空间，或增加空间消耗来节省时间，这是计算机学科中常用的方法。

## 构造函数（constructor)

函数名称必须和class的名称相同，且没有返回类型

![image-20211219180803933](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219180803933.png)

**不带指针的class基本都不用写析构函数**

因为有重载（overloading）的存在，所以可以有多个同名函数（它们其实在编译器看来函数名称是不一样的）

重载经常会被用在构造函数，但如果已经有包含默认实参的构造函数，再写以下构造函数，就冲突了：

![image-20211219181553089](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219181553089.png)

![image-20211219181808976](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219181808976.png)

把构造函数放在private里面，经常被用于单例模式，不让外界来构造对象

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211219182354582.png" alt="image-20211219182354582" style="zoom: 50%;" />

- const 常量成员函数

  用 const 修饰函数也就是标识函数不会修改数据。如果函数不会改变里面的数据内容，一定要加const。

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211220173926149.png" alt="image-20211220173926149" style="zoom:50%;" />

### 创建对象的两种方式（自己整理的）

1、不含指针，直接调用构造函数   A a(参数)

2、使用指针  A* a=new A()-----注意要有delete

## 参数传递与返回值

- 传引用其实就相当于传指针

**参数传递以及返回值尽量传引用，不传值（速度更快，效率更高）**

但是如果是传引用的话，如果在函数里改了，那原有的东西也会改了（因为指向的其实是同一个东西）

如果不想让改，需要加const。

**&有两个意思 放在变量左边是取地址， 比如 scanf("%d",&a)，放在右边则是传引用**

- friend（友元）

  自由取得friend的private成员，并且不破坏封装性

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222161856020.png" alt="image-20211222161856020" style="zoom:50%;" />

相同class的各个objects互为友元

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222162206803.png" alt="image-20211222162206803" style="zoom: 50%;" />

### 在写一个class时要注意的事项：

1、数据要private；

2、函数参数和返回值尽量传reference；

​	什么时候返回值不能传reference？假如函数体内要凭空产生一个东西，那么这个东西本体的作用域是局部的，不能返回引用。因为当函数结束时，这个东西本体已经死掉了，传回去的引用没有任何意义。反之，如果函数体内是对传入的参数东西做修改，那么返回引用即可。

3、根据情况，加const；

4、构造函数尽量不要写成赋值的那种形式，写成初始化的形式。

```C++
complex (double r = 0, double i = 0): re (r), im (i) { }
```

## 操作符重载与临时对象

操作符重载一定是作用在左边的东西上的

1、成员函数-操作符重载

**所有的成员函数一定都带有一个隐藏的参数this。**谁调用这个函数，谁就是this

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222174154036.png" alt="image-20211222174154036" style="zoom:50%;" />

传递者无需知道接收者是以reference还是value（object）形式接收的。这也是为什么上面 return *ths返回的是一个object，但返回值类型却是complex&

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222225407787.png" alt="image-20211222225407787" style="zoom:50%;" />

但是返回类型不能写成void，如果是void三个连加的就不能用了

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222225746975.png" alt="image-20211222225746975" style="zoom:50%;" />

2、非成员函数-操作符重载

这种属于全局函数

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222230709414.png" alt="image-20211222230709414" style="zoom:50%;" />

创建临时对象  typename();

注意：临时对象运行到代码的下一行这个对象就不见了

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222231228870.png" alt="image-20211222231228870" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211222233007633.png" alt="image-20211222233007633" style="zoom:50%;" />

C++中“.”和“->”的使用区别：

这两个符号都是C++成员运算符，主要用于确定类对象和成员之间的关系，用于引用类、结构和共用体的成员。

点运算符“.”应用于实际的对象，箭头运算符“->”与一个指针对象的指针一起使用。

```C++
class A
{
public:
	int a = 0;
};
int main()
{
	A b;
	A *p = &b;
	b.a; //类类型的对象访问类的成员
	p->a; //类类型的指针访问类的成员
}
```



## 复数的代码

complex.h文件

```C++
#ifndef __MYCOMPLEX__
#define __MYCOMPLEX__

class complex
{
public:
	complex(double r = 0, double i = 0) :re(r), im(i) {}
    //这里的是成员函数
	complex& operator += (const complex&);
	double real() const { return re; }
	double imag() const { return im; }
private:
	double re, im;
	friend complex& _doapl(complex*, const complex&);//友元函数可以直接调用private数据
};
inline complex& _doapl(complex* ths, const complex& r) {
	ths->re += r.re;
	ths->im += r.im;
	return *ths;
}

//这个是成员函数，所以函数名前面要加类名::
inline complex& complex::operator+=(const complex& r) {
	return _doapl(this, r);
}

inline double
real(const complex& x)
{
	return x.real();
}
inline double
imag(const complex& x)
{
	return x.imag();
}
//为什么要用全局函数？因为不仅仅只有复数加复数，也可能实数和复数相加。（如果写为成员函数，那只能应付复数加复数）
//加完得到的数是一个新产生的东西，所以返回值不能用引用，要用complex，return by value
inline complex operator + (const complex& x, const complex& y) {
	//要新创建一个对象(临时的）
	return complex(real(x)+ real(y),imag(x)+imag(y));
}
//复数+实数
inline complex operator + (const complex& x, double y) {
	//要新创建一个对象(临时的）
	return complex(real(x) + y, imag(x));
}
#endif   //__MYCOMPLEX__
```

test.cpp文件

```C++
#include <iostream>
#include "complex.h"

using namespace std;

//这里注意不能写void，不然没法连续输出（连续输出的时候需要左值，所以要返回ostream的引用）
ostream&
operator << (ostream& os, const complex& x)
{
	return os << '(' << real(x) << ',' << imag(x) << ')';
}

int main()
{
	complex c1(2, 1);
	complex c2(4, 9);
	cout << c1 << endl;
	cout << c2 << endl;
	cout << c1 + c2 << endl;
	cout << (c1 += c2) << endl;
	cout << (5 + c2) << endl;
	return 0;
}
输出：
(2,1)
(4,9)
(6,10)
(6,10)
(9,9)
```

## VS安装位置

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211223223545682.png" alt="image-20211223223545682" style="zoom:50%;" />





## 一些自己搜补充资料

### C++指针

```c++
#include <iostream>
 
using namespace std;
 
int main ()
{
   int  var = 20;   // 实际变量的声明
   int  *ip;        // 指针变量的声明
 
   ip = &var;       // 在指针变量中存储 var 的地址
 
   cout << "Value of var variable: ";
   cout << var << endl;
 
   // 输出在指针变量中存储的地址
   cout << "Address stored in ip variable: ";
   cout << ip << endl;
 
   // 访问指针中地址的值（指针的基类型）
   cout << "Value of *ip variable: ";
   cout << *ip << endl;
 
   return 0;
}

输出：
Value of var variable: 20
Address stored in ip variable: 0xbfc601ac
Value of *ip variable: 20
```

1、C++空指针NULL

在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。赋为 NULL 值的指针被称为空指针。NULL 指针是一个定义在标准库中的值为零的常量。

```c++
   int  *ptr = NULL;
   cout << "ptr 的值是 " << ptr ptr 的值是 0
       
if(ptr)     /* 如果 ptr 非空，则完成 */
if(!ptr)    /* 如果 ptr 为空，则完成 */
```

2、C++指针 vs 数组

一个指向数组开头的指针，可以通过使用指针的算术运算或数组索引来访问数组。请看下面的程序：

```C++
#include <iostream>
using namespace std;
const int MAX = 3;
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int  *ptr;
 
   // 指针中的数组地址
   ptr = var;
   for (int i = 0; i < MAX; i++)
   {
      cout << "var[" << i << "]的内存地址为 ";
      cout << ptr << endl;
 
      cout << "var[" << i << "] 的值为 ";
      cout << *ptr << endl;
 
      // 移动到下一个位置
      ptr++;
   }
   return 0;
}

结果：
var[0]的内存地址为 0x7fff59707adc
var[0] 的值为 10
var[1]的内存地址为 0x7fff59707ae0
var[1] 的值为 100
var[2]的内存地址为 0x7fff59707ae4
var[2] 的值为 200
```

另外值得注意的是：

把指针运算符 * 应用到 var 上是完全可以的，但修改 var 的值是非法的。这是因为 **var 是一个指向数组开头的常量**，不能作为左值。

由于一个数组名对应一个指针常量，只要不改变数组的值，仍然可以用指针形式的表达式。例如，下面是一个有效的语句，把 var[2] 赋值为 500：

```C++
*(var + 2) = 500;
```

3、普通数组和指针数组

普通数组：

```C++
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
 
   for (int i = 0; i < MAX; i++)
   {
      cout << "Value of var[" << i << "] = ";
      cout << var[i] << endl;
   }
   return 0;
}
输出：
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```

指针数组：

int *ptr[MAX]; 在这里，把 ptr 声明为一个数组，由 MAX 个整数指针组成。因此，ptr 中的每个元素，都是一个指向 int 值的指针。

```C++
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int *ptr[MAX];
 
   for (int i = 0; i < MAX; i++)
   {
      ptr[i] = &var[i]; // 赋值为整数的地址
   }
   for (int i = 0; i < MAX; i++)
   {
      cout << "Value of var[" << i << "] = ";
      cout << *ptr[i] << endl;
   }
   return 0;
}
输出：
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```

4、指向指针的指针（多级间接寻址）

![image-20211221213508373](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20211221213508373.png)

例如，下面声明了一个指向 int 类型指针的指针：

```
int **var;
```

当一个目标值被一个指针间接指向到另一个指针时，访问这个值需要使用两个星号运算符

```C++
int main ()
{
    int  var;
    int  *ptr;
    int  **pptr;
 
    var = 3000;
 
    // 获取 var 的地址
    ptr = &var;
 
    // 使用运算符 & 获取 ptr 的地址
    pptr = &ptr;
 
    // 使用 pptr 获取值
    cout << "var 值为 :" << var << endl;
    cout << "*ptr 值为:" << *ptr << endl;
    cout << "**pptr 值为:" << **pptr << endl;
    return 0;
}
输出均为3000
```

5、传递指针给函数

允许您传递指针给函数，只需要简单地声明函数参数为指针类型即可。

下面的实例中，我们传递一个无符号的 long 型指针给函数，并在函数内改变这个值：

```C++
#include <iostream>
#include <ctime>
using namespace std;
// 在写函数时应习惯性的先声明函数，然后在定义函数
void getSeconds(unsigned long *par);
 
int main ()
{
   unsigned long sec;
   getSeconds( &sec );
   // 输出实际值
   cout << "Number of seconds :" << sec << endl;
   return 0;
}
 
void getSeconds(unsigned long *par)
{
   // 获取当前的秒数
   *par = time( NULL );
   return;
}
输出：Number of seconds :1294450468
```

注意，数组名称作为指针也可以当参数。

**表示指针的数组名即第一个数组元素的地址**

6、从函数返回指针

必须声明一个返回指针的函数，如下所示：

```C++
int * myFunction()
{...}
```

**另外，C++ 不支持在函数外返回局部变量的地址，除非定义局部变量为 static变量。**

### C++引用（reference）

引用变量是一个别名，也就是说，它是某个已存在变量的另一个名字。一旦把引用初始化为某个变量，就可以使用该引用名称或变量名称来指向变量。

引用 vs 指针的区别：

1. 不存在空引用。引用必须连接到一块合法的内存。
2. 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
3. 引用必须在创建时被初始化。指针可以在任何时间被初始化。

```
int i = 17;
int&  r = i;
double& s = d;
```

在这些声明中，& 读作引用。因此，第一个声明可以读作 "r 是一个初始化为 i 的整型引用"，第二个声明可以读作 "s 是一个初始化为 d 的 double 型引用"。

```C++
int main ()
{
   // 声明简单的变量
   int    i;
   double d;
 
   // 声明引用变量
   int&    r = i;
   double& s = d;
   
   i = 5;
   cout << "Value of i : " << i << endl;
   cout << "Value of i reference : " << r  << endl;
 
   d = 11.7;
   cout << "Value of d : " << d << endl;
   cout << "Value of d reference : " << s  << endl;
   
   return 0;
}
输出：
Value of i : 5
Value of i reference : 5
Value of d : 11.7
Value of d reference : 11.7
```

1、把引用作为函数参数

```C++
#include <iostream>
using namespace std;
 
// 函数声明
void swap(int& x, int& y);
 
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
 
   cout << "交换前，a 的值：" << a << endl;
   cout << "交换前，b 的值：" << b << endl;
 
   /* 调用函数来交换值 */
   swap(a, b);
 
   cout << "交换后，a 的值：" << a << endl;
   cout << "交换后，b 的值：" << b << endl;
 
   return 0;
}
 
// 函数定义
void swap(int& x, int& y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
   return;
}
输出：
交换前，a 的值： 100
交换前，b 的值： 200
交换后，a 的值： 200
交换后，b 的值： 100
```

2、把引用作为返回值

通过使用引用来替代指针，会使 C++ 程序更容易阅读和维护。C++ 函数可以返回一个引用，方式与返回一个指针类似。

当函数返回一个引用时，则返回一个指向返回值的隐式指针。这样，函数就可以放在赋值语句的左边。

```C++
#include <iostream>
 
using namespace std;
 
double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
 
double& setValues(int i) {  
   double& ref = vals[i];    
   return ref;   // 返回第 i 个元素的引用，ref 是一个引用变量，ref 引用 vals[i]
 
 
}
 
// 要调用上面定义函数的主函数
int main ()
{
 
   cout << "改变前的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
 
   setValues(1) = 20.23; // 改变第 2 个元素
   setValues(3) = 70.8;  // 改变第 4 个元素
 
   cout << "改变后的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
   return 0;
}
输出：
改变前的值
vals[0] = 10.1
vals[1] = 12.6
vals[2] = 33.1
vals[3] = 24.1
vals[4] = 50
改变后的值
vals[0] = 10.1
vals[1] = 20.23
vals[2] = 33.1
vals[3] = 70.8
vals[4] = 50
```

当返回一个引用时，要注意被引用的对象不能超出作用域。所以返回一个对局部变量的引用是不合法的，但是，可以返回一个对静态变量的引用。

```C++
int& func() {
   int q;
   //! return q; // 在编译时发生错误
   static int x;
   return x;     // 安全，x 在函数作用域外依然是有效的
}
```



## 三大函数：拷贝构造、拷贝复制、析构

对于不含指针的class类，这些函数不用管

这三大函数都不需要加const，因为会对对象进行修改

对于字符串的设计，一般就是里面包含一个指针，当需要内存的时候才创建具体的字符串内容--->动态分配内存-->对象死亡之前要进行析构，把动态分配的内存释放掉

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220118161058782.png" alt="image-20220118161058782" style="zoom: 33%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220118175759598.png" alt="image-20220118175759598" style="zoom:33%;" />

也就是说具体的字符串内容是不属于对象本身的。

疑惑1：
都是构造字符串，s1 s2为啥不用删掉，p要删掉？

答案：
s1和s2是对象， 在退出作用域的时候会自动调用析构函数释放内存
p是指针， 退出作用域的时候释放了指针本身的内存，但是没有释放指针指向的地址的内存，delete来释放指针指向地址的内存

疑惑2：

delete后面为什么加了[]？

答案：

因为m_data是数组，如果是单个的，就不用加[]

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220118183142786.png" alt="image-20220118183142786" style="zoom: 33%;" />

深拷贝则需要写拷贝构造函数

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220118190653815.png" alt="image-20220118190653815" style="zoom:33%;" />

- 拷贝赋值函数则分三步：把左侧的清空，左侧腾出一个和右侧一样大小的空间，把右侧的东西拷贝到左侧

注意，要写上检测自我赋值的情况，不仅仅是为了效率，更是为了正确性

## 堆、栈与内存管理

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220122231625459.png" alt="image-20220122231625459" style="zoom:50%;" />

在函数function body内声明的变量，内存来自于stack
heap是操作系统提供的一块global内存空间，可通过new进行动态分配内存
stack object在作用域结束后被自动清理（也就是析构函数会自动被调用），而heap object仍然存在

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220122231728501.png" alt="image-20220122231728501" style="zoom: 50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220122231815247.png" alt="image-20220122231815247" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220122231923695.png" alt="image-20220122231923695" style="zoom:50%;" />

static：
在变量声明前加static关键字，即为static object，其在作用域结束后仍然存在，直到整个程序结束
global object也可看作一种static object，作用域是整个程序

注意内存泄露问题：

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220122232049872.png" alt="image-20220122232049872" style="zoom:50%;" />


new被编译器转化为三个步骤：
1.调用malloc分配内存
2.static_cast类型转换（因为第一步操作得到的指针是point to void，需要转换成我们需要类型的指针）
3.调用构造函数

delete为两个步骤:
1.调用析构函数
2.调用free释放内存

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220124002216702.png" alt="image-20220124002216702" style="zoom:50%;" />

array new 一定要搭配 array delete
delete 和 delete[] 都会将其所指的内存块全部释放
区别在于delete[]会对数组内的每一个元素调用析构函数 而delete只对第一个元素调用
如果数组元素为带有指针成员的类，使用delete会造成memory leak

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220124002420180.png" alt="image-20220124002420180" style="zoom:50%;" />

## String类的代码实现

string.h

```C++
#ifndef __MYSTRING__
#define __MYSTRING__

class String
{
public:                                 
   String(const char* cstr=0);                     
   String(const String& str);                    
   String& operator=(const String& str);         
   ~String();                                    
   char* get_c_str() const { return m_data; }
private:
   char* m_data;
};

#include <cstring>
//构造函数
inline
String::String(const char* cstr)
{
   if (cstr) {
      m_data = new char[strlen(cstr)+1];
      strcpy(m_data, cstr);
   }
   else {   
      m_data = new char[1];
      *m_data = '\0';
   }
}

//析构函数
inline
String::~String()
{
   delete[] m_data;
}

//拷贝赋值函数
inline
//为什么要加返回值？为了应对连续赋值的情况
String& String::operator=(const String& str)
{
    //拷贝赋值一定要关注到是否有自我赋值
    //来源端是传进来的，目的端是本身this
   if (this == &str)
      return *this;

   delete[] m_data;
   m_data = new char[ strlen(str.m_data) + 1 ];
   strcpy(m_data, str.m_data);
    //注意，返回值的时候直接返回东西就好，不用管返回值类型是引用还是对象，用什么来接不关这里的事，这里return东西就好
   return *this;
}

//拷贝构造函数
inline
String::String(const String& str)
{
   m_data = new char[ strlen(str.m_data) + 1 ];
   strcpy(m_data, str.m_data);
}

#include <iostream>
using namespace std;
ostream& operator<<(ostream& os, const String& str)
{
   os << str.get_c_str();
   return os;
}

#endif

```

string_test.c

```C++
#include "string.h"
#include <iostream>

using namespace std;

int main()
{
  String s1("hello"); 
  String s2("world");
    
  String s3(s2);
  cout << s3 << endl;
  
  s3 = s1;
  cout << s3 << endl;     
  cout << s2 << endl;  
  cout << s1 << endl;      
}
```

## static的补充

成员函数加上static后，就是没有this指针了，如果要处理数据，只能处理静态的数据（因为没有this指针就找不到具体的对象）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126000438600.png" alt="image-20220126000438600" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126000724752.png" alt="image-20220126000724752" style="zoom:50%;" />

### 利用static实现单例模式

1、饿汉式

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126001803370.png" alt="image-20220126001803370" style="zoom:50%;" />

2、懒汉式（最优）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126001906617.png" alt="image-20220126001906617" style="zoom:50%;" />

类模板（class template)和函数模板（function template，C++中的算法函数基本都是这种形式，比如min,max）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126002801917.png" alt="image-20220126002801917" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126002905727.png" alt="image-20220126002905727" style="zoom:50%;" />

命名空间（namespace)

标准库中所有的东西都被封在了std里

## 类与类之间的关系（继承、复合、委托）

### 1、**composition（复合）**：

属于has a的关系

queue完全可以由deque的部分功能完成

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126224242033.png" alt="image-20220126224242033" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126224312117.png" alt="image-20220126224312117" style="zoom:50%;" />

构造由内到外，析构从外到内

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126225104460.png" alt="image-20220126225104460" style="zoom:50%;" />

### 2、**委托（delegation）**

属于：用指针的复合（composition by reference)

注意，两边的生命周期不同

这是很经典的做法，左边当接口（handle)，右边是具体实现(body)。这样右侧可以很灵活，不受左侧的影响

#### PIMPL设计模式

（private implementation或pointer to implementation）也称为handle/body idiom，一篇写的挺好的文：https://blog.csdn.net/clh01s/article/details/74784328

可利用指针来解决：

1、降低编译依赖、提高重编译速度

```
   a、因为指针对于32为的系统来说大小是4，64的系统来说是大小是8，这是相对稳定的。

   b、即使类B发生改变，指针的大小也不会发生改变。文件a.h也不需要重编译

   c、利用指针以后a.h不需要包含b.h，只需要进行前向声明。

   d、main.cpp包含了a.h但a.h中没有包含b.h,不依赖于b.h

   e、假如类B有子类，可以在在运行期间通过指针调用B类的子类，进行调用实现多态的功能。
```

2、接口和实现分离

```
      通过使用指针，其所指的类的实现进行分离了，不关心类B的实现，指针的大小是固定的。
```

3、降低模块的耦合度

```C++
// file a.h
 class B;//前向声明
 class A {
 public:
    A(){}
	~A(){}
 	void Fun();
        B* b_;
};

// file a.cpp
#include "a.h"
#include “b.h"//b.h只需要包含一次
A::A() : px_( new A ) {
}
A::~A() {
 delete b_;
  b_ = 0;

}
void A::Fun() {
   b_->Fun();
}

// file main.cpp
#include “a.h” //没有包含b.h
int main(void)
{
 A a;
 a.Fun();
}

```

![image-20220126234313889](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126234313889.png)

### 3、**继承（inheritance）**

表示is a

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220126235131092.png" alt="image-20220126235131092" style="zoom:50%;" />

子类的对象里面有父类的成分

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127001016622.png" alt="image-20220127001016622" style="zoom:67%;" />

#### 虚函数

继承经常与虚函数结合着使用

- 父类成员函数有三种形式：

1. 纯虚函数：没有定义，因为父类不知道怎么定义它，子类一定要Override它
2. 非纯虚函数：有定义，继承它的子类可以override它、
3. 非虚函数：有定义，且子类不能override它

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127215852988.png" alt="image-20220127215852988" style="zoom:50%;" />

#### **<u>Template Method（模板方法）设计模式</u>**

把关键动作延缓到子类去完成——叫做Template Method（模板方法）设计模式。经常会用于框架设计中，先把固定的部分写好，然后把未设定的内容写成虚函数，以后由子类来实现

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127231037141.png" alt="image-20220127231037141" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127232151848.png" alt="image-20220127232151848" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127234115342.png" alt="image-20220127234115342" style="zoom:50%;" />



#### **<u>观察者模式(Observer)</u>**

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127234258287.png" alt="image-20220127234258287" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220127234322720.png" alt="image-20220127234322720" style="zoom:50%;" />

#### **<u>Composite复合物设计模式</u>**

（比如设计文件系统）

![image-20220128212626516](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220128212626516.png)



#### **<u>Prototype（原型）设计模式</u>**

原型模式的原理很简单，将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象克隆自己来实现创建过程。

> **原型模式（Prototype）：**使用原型实例指定创建对象的种类，并且通过拷贝这些原 型创建新的对象。原型模式是一种对象创建型模式。

问题：写父类框架的时候，不知道以后子类的类名。

![image-20220128224904384](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220128224904384.png)

每一个子类都有一个构造函数，构造函数里面把自己add到父类框架里。
每个子类里面还有一个clone函数，这样父类在得到子类对象后就可以通过这个子类对象调用clone函数，得到子类的若干个副本。

为什么不能把clone设置成static?因为用static需要知道类名，而父类在写的时候不知道子类类名

![image-20220128225009863](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220128225009863.png)

注意，static的数据和函数要在类外进行声明。

全局变量和静态变量初始化时会自动被设置为0。如果们声明全局变量，那么他在运行前会变成全0。意思就是全局变量和静态变量可以不用赋初值，局部变量必须赋初值

![image-20220128225106576](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220128225106576.png)

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220128225134619.png" alt="image-20220128225134619" style="zoom:67%;" />

# 侯捷C++面向对象（下）

## 转换函数

作用：把这种东西转换为别的东西

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201215142760.png" alt="image-20220201215142760" style="zoom:50%;" />

## non-expicit-one-argument ctor

作用：把别的东西转换为这种东西

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201220425547.png" alt="image-20220201220425547" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201222042691.png" alt="image-20220201222042691" style="zoom:50%;" />

 什么是隐式自动转换？

```C++
class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
{  
public:  
    char *_pstr;  
    int _size;  
    CxString(int size)  
    {  
        _size = size;                // string的预设大小  
        _pstr = malloc(size + 1);    // 分配string的内存  
        memset(_pstr, 0, size + 1);  
    }  
    CxString(const char *p)  
    {  
        int size = strlen(p);  
        _pstr = malloc(size + 1);    // 分配string的内存  
        strcpy(_pstr, p);            // 复制字符串  
        _size = strlen(_pstr);  
    }  
    // 析构函数这里不讨论, 省略...  
};  
  
    // 下面是调用:  
    CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
    CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数, 错误为: “CxString”: 没有合适的默认构造函数可用  
    CxString string4("aaaa"); // 这样是OK的  
    CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
    CxString string6 = 'c';   // 这样也是OK的, 其实调用的是CxString(int size), 且size等于'c'的ascii码  
    string1 = 2;              // 这样也是OK的, 为CxString预分配2字节的大小的内存  
    string2 = 3;              // 这样也是OK的, 为CxString预分配3字节的大小的内存  
    string3 = string1;        // 这样也是OK的, 至少编译是没问题的, 但是如果析构函数里用free释放_pstr内存指针的时候可能会报错, 完整的代码必须重载运算符"=", 并在其中处理内存释放
```

上面的代码中, "CxString string2 = 10;" 这句为什么是可以的呢? 在C++中, 如果的构造函数只有一个参数时, 那么在编译的时候就会有一个缺省的转换操作:将该构造函数对应数据类型的数据转换为该类对象. 也就是说 "CxString string2 = 10;" 这段代码, 编译器自动将整型转换为CxString类对象, 实际上等同于下面的操作:

```C++
CxString string2(10);  
或  
CxString temp(10);  
CxString string2 = temp; 
```

C++中的explicit关键字只能用于修饰只有一个参数的类构造函数, 它的作用是表明该构造函数是显示的, 而非隐式的。

explicit关键字的作用就是防止类构造函数的隐式自动转换。

```C++
class CxString  // 使用关键字explicit的类声明, 显示转换  
{  
public:  
    char *_pstr;  
    int _size;  
    explicit CxString(int size)  
    {  
        _size = size;  
        // 代码同上, 省略...  
    }  
    CxString(const char *p)  
    {  
        // 代码同上, 省略...  
    }  
};  
  
    // 下面是调用:  
    CxString string1(24);     // 这样是OK的  
    CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数  
    CxString string4("aaaa"); // 这样是OK的  
    CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
    CxString string6 = 'c';   // 这样是不行的, 其实调用的是CxString(int size), 且size等于'c'的ascii码, 但explicit关键字取消了隐式转换  
    string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
    string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
    string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载 
```

explicit关键字只需用于类内的单参数构造函数前面。由于无参数的构造函数和多参数的构造函数总是显示调用，这种情况在构造函数前加explicit无意义。

 google的c++规范中提到explicit的优点是可以避免不合时宜的类型变换，缺点无。所以google约定所有单参数的构造函数都必须是显示的，只有极少数情况下拷贝构造函数可以不声明称explicit。例如作为其他类的透明包装器的类。
　　effective c++中说：被声明为explicit的构造函数通常比其non-explicit兄弟更受欢迎。因为它们禁止编译器执行非预期（往往也不被期望）的类型转换。除非我有一个好理由允许构造函数被用于隐式类型转换，否则我会把它声明为explicit，鼓励大家遵循相同的政策。

## 让一个类像指针-智能指针

里面一定有一个普通的指针，所以要有*和->的功能

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201223930920.png" alt="image-20220201223930920" style="zoom:50%;" />

容器里的迭代器也属于智能指针的范畴

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201224828287.png" alt="image-20220201224828287" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201224855952.png" alt="image-20220201224855952" style="zoom:50%;" />

## 让一个类像函数（仿函数，重载()）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201230337534.png" alt="image-20220201230337534" style="zoom:50%;" />

## namespace经验谈

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220201230818825.png" alt="image-20220201230818825" style="zoom:50%;" />

## 模板

### 1、类模板

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205142916609.png" alt="image-20220205142916609" style="zoom:50%;" />

### 2、函数模板

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205143044288.png" alt="image-20220205143044288" style="zoom:50%;" />

### 3、成员模板

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205143115380.png" alt="image-20220205143115380" style="zoom:67%;" />

![image-20220205143146020](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205143146020.png)

![image-20220205143204194](C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205143204194.png)

### 模板特化

泛化指的是类型很宽泛，等实际用的时候再确定具体的

特化则与泛化相反

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205161842019.png" alt="image-20220205161842019" style="zoom:50%;" />

### 模板偏特化

1、个数上的偏

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205163818367.png" alt="image-20220205163818367" style="zoom: 50%;" />

2、范围上的偏

以前是任意类型，现在缩小类型范围，比如是指向任意类型的指针

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205164226310.png" alt="image-20220205164226310" style="zoom:50%;" />

### 模板模板参数

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205172520732.png" alt="image-20220205172520732" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205172539076.png" alt="image-20220205172539076" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205172557612.png" alt="image-20220205172557612" style="zoom:50%;" />

## C++11的一些新特性

### 数量不定的模板参数

分为1个+1包

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205181445628.png" alt="image-20220205181445628" style="zoom:50%;" />

### auto

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205182256187.png" alt="image-20220205182256187" style="zoom:50%;" />

### for（遍历容器）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220205231208737.png" alt="image-20220205231208737" style="zoom: 50%;" />

## 从内存的角度去看变量，有3种（值，指针、引用）

1、值本身 value

2、它的指针。指针就是地址的一种形式，占据4个字节

3、它的引用。引用可以理解成别名，reference在声明的时候一定要有初值，并且以后不能再变了（不能再代表别的东西）。引用的字节大小和它代表的东西的大小一样，地址也相同（但其实这是编译器营造的假象）

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220206001037192.png" alt="image-20220206001037192" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220206001138243.png" alt="image-20220206001138243" style="zoom:50%;" />

reference就是一种漂亮的point，一般不会用于声明变量，多用于参数的传递和返回值上。

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220206002354583.png" alt="image-20220206002354583" style="zoom:50%;" />

## 对象模型（object model)

### vptr与vtbl（虚机制，多态）

vptr（虚指针）和 vtbl（虚表，虚表里放的都是指针，指向虚函数）

只要类里面有虚函数，就会多一个指针vptr

子类中有父类的part，继承了父类的数据，以及函数的调用权

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207001154281.png" alt="image-20220207001154281" style="zoom: 50%;" />



C++编译器看到一个函数，有两种考量，是静态绑定还是动态绑定

静态绑定：通过对象来调用，比如a.func()；call ***地址，也就是调用一个固定的地址

动态绑定：动态绑定的条件有：1、通过指针调用； 2、向上转型；3、调用的是虚函数

这种虚机制，就是多态

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207230554816.png" alt="image-20220207230554816" style="zoom:50%;" />

### this

通过一个对象调用一个函数，那个对象的地址就是this指针

模板方法就用到了动态绑定

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207223433889.png" alt="image-20220207223433889" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207230703462.png" alt="image-20220207230703462" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207230728102.png" alt="image-20220207230728102" style="zoom:50%;" />

## 谈谈const

const放置的位置：

1、放在成员函数后面——告诉编译器这个函数不改变data

2、放在对象前面——告诉编译器data members不能改变

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207233011022.png" alt="image-20220207233011022" style="zoom: 50%;" />

const对象不能调用non-const函数，会报错

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220207233119981.png" alt="image-20220207233119981" style="zoom:67%;" />

## new和delete的重载

可以重载全局的new和new[]

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000733392.png" alt="image-20220208000733392" style="zoom:50%;" />

也可以重载成员的new和new[]

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000753308.png" alt="image-20220208000753308" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000808165.png" alt="image-20220208000808165" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000831007.png" alt="image-20220208000831007" style="zoom: 50%;" />



<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000632473.png" alt="image-20220208000632473" style="zoom:67%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208000702097.png" alt="image-20220208000702097" style="zoom:50%;" />

也可以重载new()

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208001924057.png" alt="image-20220208001924057" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208001951135.png" alt="image-20220208001951135" style="zoom:50%;" />

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208002010692.png" alt="image-20220208002010692" style="zoom:50%;" />

重载new() 一个标准库的使用例子，为了在构造的时候，在string内容外包上一个计数器reference

<img src="C:\Users\zcc\AppData\Roaming\Typora\typora-user-images\image-20220208002422681.png" alt="image-20220208002422681" style="zoom:50%;" />

# STL

STL4个基本组件

1、容器

- 顺序容器：向量（vector)，双端队列(deque)，列表容器（list）
- 关联容器：集合（set），多重集合（multiset），映射(map)，多重映射（multimap）

2、迭代器

3、函数对象

4、算法



标准库以头文件的形式表现









