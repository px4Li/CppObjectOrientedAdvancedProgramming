<p style="text-align: center;">面向对象高级编程（侯捷视频笔记）</p>

- [1. Object Based vs. Object Oriented](#1-object-based-vs-object-oriented)
- [2. Header中的防卫是声明](#2-header中的防卫是声明)
- [3. Header 的布局](#3-header-的布局)
- [4. class 的声明](#4-class-的声明)
- [5. class template 简介](#5-class-template-简介)
- [6. inline 函数](#6-inline-函数)
- [7. access level](#7-access-level)
- [8. constructor（ctor,构造函数）](#8-constructorctor构造函数)
- [9. ctor 可以有很多个-overloading](#9-ctor-可以有很多个-overloading)
- [10. constructor(ctor, 构造函数)被放在private区](#10-constructorctor-构造函数被放在private区)
- [11. const member function](#11-const-member-function)
- [12. pass/return by value vs. pass by reference(to const)](#12-passreturn-by-value-vs-pass-by-referenceto-const)
- [13. class body 外的各种定义](#13-class-body-外的各种定义)
- [14. operator overloading -1 member function](#14-operator-overloading--1-member-function)
- [15. operator overloading -2 no member function no this point](#15-operator-overloading--2-no-member-function-no-this-point)
  - [15.1. temp object typename()](#151-temp-object-typename)
- [16. 复习](#16-复习)
- [17. Classes 的两个经典分类](#17-classes-的两个经典分类)
  - [17.1. String class](#171-string-class)
  - [17.2. Big Tree](#172-big-tree)
  - [17.3. ctor和dtor](#173-ctor和dtor)
  - [17.4. copy construct](#174-copy-construct)
  - [17.5. copy assignment operator](#175-copy-assignment-operator)
  - [17.6. output](#176-output)
- [18. 栈与内存](#18-栈与内存)
  - [18.1. 动态分配所得的内存块（memory block）](#181-动态分配所得的内存块memory-block)
  - [18.2. 动态分配所得的array](#182-动态分配所得的array)
  - [18.3. array new 一定要搭配array delete](#183-array-new-一定要搭配array-delete)
  - [18.4. 复习String类的实现过程](#184-复习string类的实现过程)
  - [18.5. 进一步补充：](#185-进一步补充)
- [19. Composition, has-a](#19-composition-has-a)
  - [19.1. 复合关系下的构造和析构](#191-复合关系下的构造和析构)
- [20. Delegation Composition by reference](#20-delegation-composition-by-reference)
- [21. Inheritance, is-a](#21-inheritance-is-a)
  - [21.1. Inheritance关系下的构造和析构](#211-inheritance关系下的构造和析构)
- [22. Delegation + Inheritance](#22-delegation--inheritance)
- [23. Conversion Function](#23-conversion-function)
- [non-explicit-one-argument ctor](#non-explicit-one-argument-ctor)
- [Conversion Function vs. non-explicit-one-argument ctor](#conversion-function-vs-non-explicit-one-argument-ctor)
- [explicit-one-argument ctor](#explicit-one-argument-ctor)

## 1. Object Based vs. Object Oriented
- Object Based: 面对的是单一class的设计
- Object Oriented: 面对的是多重classes的设计，classes之间的关系。

## 2. Header中的防卫是声明

<details><summary>complex.h</summary>
<div>

```cpp
#ifndef __COMPLEX__ //如果曾经没有定义过COMPLEX这个名字的话
#define __COMPLEX__ //就把这个名字定义出来

//第二次Include的时候就不会进到主程序里来

//...主程序...

#endif
```
</div>
</details>

## 3. Header 的布局

<details><summary>头文件的布局</summary>
<div>

```cpp
#ifndef __COMPLEX__ //如果曾经没有定义过COMPLEX这个名字的话
#define __COMPLEX__ //就把这个名字定义出来

//forward declarations
#include<cmath>
class ostream;
class complex;
complex& __doapl(complex* ths, const complex& r);

//class declaratons
class complex{
    //...
};

//class definition
complex::function

#endif
```
</div>
</details>

## 4. class 的声明
<details><summary>类的声明</summary>
<div>

```cpp
class complex{ // ------ class head
//-----------------class body-----------------------
public: 
    complex(double r=0, double i=0): re(r), im(i){}
    complex& operator += (const complex&);
    double real() const {return re;}
    double imag() const {return im;}
private:
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
</div>
</details>

## 5. class template 简介
<details><summary>模板简介</summary>
<div>

```cpp
template<typename T>
class complex{ // ------ class head
//-----------------class body-----------------------
public: 
    complex(T r=0, T i=0): re(r), im(i){}
    complex& operator += (const complex&);
    T real() const {return re;}
    T imag() const {return im;}
private:
    T re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
```cpp
{
    //绑定模板类型
    complex<double> c1 (2.5, 1.5);
    complex<int> c2(2,6);
    ...
}
```
</div>
</details>

## 6. inline 函数


<details><summary>内联函数</summary>
<div>

- 函数在class body内定义完成，自动成为内联，不需要加关键字。
- 如果函数内简单，编译器有可能会编译成inline。
  
```cpp
class complex{ 
public: 
    complex(double r=0, double i=0): re(r), im(i){} //inline
    complex& operator += (const complex&);
    double real() const {return re;}//inline
    double imag() const {return im;}//inline
private:
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
- 不在class本体类定义的函数要加inline
```cpp
inline double imag(const complex& x){
    return x.imag();
}
```
</div>
</details>

## 7. access level
<details><summary>访问级别</summary>
<div>
  
```cpp
class complex{ 
public: 
    complex(double r=0, double i=0): re(r), im(i){} //inline
    complex& operator += (const complex&);
    double real() const {return re;}//inline
    double imag() const {return im;}//inline
private: // 数据的东西应该放在private内
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
报错！访问的是private
```cpp
{
    complex c1(2,1);
    cout << c1.re;
    cout << c1.im;
}
```
访问的是public
```cpp
{
    complex c1(2,1); //通过创造对象调用构造函数
    cout << c1.real();
    cout << c1.imag();
}
```
</div>
</details>

## 8. constructor（ctor,构造函数）
- 构造函数和类的名称相同
- 可以有默认实参
- 没有返回类型
- 初始化列表

<details><summary>构造函数</summary>
<div>
不带指针的类多半不用写析构函数

```cpp
class complex{ 
public: 
    complex(double r=0, double i=0)
    : re(r), im(i) //初始化列表，第一阶段初始值
    {}//第二阶段赋值
    complex& operator += (const complex&);
    double real() const {return re;}//inline
    double imag() const {return im;}//inline
private: // 数据的东西应该放在private内
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
直接赋值相对于上面效率低
```cpp
    complex(double r=0, double i=0)
    {re = r; im = i;}//赋值
```
```cpp
{
    complex c1(2,1); //通过创造对象调用构造函数
    cout << c1.real();
    cout << c1.imag();
}
```
</div>
</details>

## 9. ctor 可以有很多个-overloading
<details><summary>函数重载</summary>
<div>

```cpp
class complex{ 
public: 
    complex(double r=0, double i=0)//默认值
    : re(r), im(i) //初始化列表，第一阶段初始值
    {}//第二阶段赋值
    complex(): re(0), im(0){} //不能与上面的同时存在

    complex& operator += (const complex&);
    double real() const {return re;}//real函数返回实部
    double imag() const {return im;}//

    void real(double r){ re = r;} //设定实部
    // real函数编译后的名称是不一样的

private: 
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
```cpp
{
    complex c1；
    complex c2();
    ...
}
```
</div>
</details>

## 10. constructor(ctor, 构造函数)被放在private区
- 我不允许构造函数被外界创建对象时，把构造函数放在private区中。

<details><summary>Singleton单件模式</summary><div>
有一种需求把ctor放在private区

```cpp
class A{
public:
    static A& getInstance();
    setup(){...}
private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance(){
    static A a;
    return a;
}
```
```cpp
A::getInstance().setup();
```
</details></div>

## 11. const member function
<details><summary>常量成员函数</summary><div>

```cpp
class complex{
public:
    complex(double r = 0, double i = 0):re(r),im(i){}
    complex& operator += (const complex&);
    //这两个函数是要拿出来复数的实部虚部，不会改变这里面的数据
    double real() const{ return re;} 
    double imag() const{ return im;}
private:
    double re, im;
    friend complex& __doapl(complex*, const complex&);
};
```
```cpp
{
    complex c1(2,1);
    cout << c1.real();
    cout << c1.imag();
}
```
```cpp
{//const 出现在对象的前面
    const complex c1(2,1);//表示这个对象一定不能改变
    cout << c1.real(); //如果287行没有const，这里就会报错
    cout << c1.imag();
}
```
</details></div>

## 12. pass/return by value vs. pass by reference(to const)
- 尽量使用引用
- 如果传过去的引用不希望被改动的化加常量
- 传引用就像传指针那么的快
<details><summary>参数传递，返回值传递</summary><div>

```cpp
class complex{
public:
    complex(double r = 0, double i = 0):re(r),im(i){}
    complex& operator += const complex&;//return by reference, complex就是返回的类型
    double real() const{ return re;} 
    double imag() const{ return im;}

    int func(const complex& param){
        return param.re + param.im;
    }

private:
    double re, im;
    friend complex& __doapl(complex*, const complex&);   //__doapl函数声明为朋友
}
```
```cpp
ostream& operator << (ostream& os, const complex& x){
    return os << '(' << real(x) << ',' << imag(x) << ')';
}
```
- 在类之外可以访问private里的数据
- 相同class的各个对象互为友元
```cpp
inline complex& __doapl(complex* ths, const complex& r){
    ths->re += r.re;//我要拿complex里面的re
    ths->im += r.im;
    return* ths;
}
```
```cpp
{
     complex c1(2,1);//表示这个对象一定不能改变
     complex c2;

     c2.func(c1);//第二个对象的func函数处理第一个对象
}
```
</details></div>

- 数据一定放在private里
- 参数尽可能用引用传递，看状况加const
- 返回值也尽量用引用传递
- 在类的本体中的变量尽量加const
- 尽量使用构造函数的初始化列表

## 13. class body 外的各种定义
<details><summary>__doapl(do assignment plus)</summary><div>

```cpp
inline complex& __doapl(complex* ths, const complex& r){
    ths->re += r.re;//我要拿complex里面的re
    ths->im += r.im;
    return* ths;
}
```
</details></div>

## 14. operator overloading -1 member function
<details><summary>操作符重载</summary><div>

```cpp
{
    complex c1(2,1);
    complex c2(5);
    c2 += c1; // 操作符作用在左值上
}
```

所有的成员函数带有一个隐藏的参数，谁调用这个函数this就指向它
```cpp
//标准库的写法！！！
inline complex& //接收端//传递着无需知道接收者是以什么形式来接受
__doapl(complex* ths, const complex& r){
    ths->re += r.re;//我要拿complex里面的re
    ths->im += r.im;
    return *ths; //返回对象value//传递着
}
//不能把this放在参数列里写出来！！！！
inline complex& //当使用者做连串预算时
complex::operator += (this, const complex& r){
    return __doapl (this, r);
}
```

```cpp
{
    complex c1(2,1);
    complex c2(5);
    c2 += c1; // 
    c3 += c2 += c1;
}
```
</details></div>

## 15. operator overloading -2 no member function no this point
- 为了对付client的三种可能用法，需要对应开发三种函数
- 如果返回的时local Object, 就不能用return by reference
### 15.1. temp object typename()
<details><summary>临时对象</summary><div>

```cpp

inline complex
operator + (const complex& x, const complex& y){
    return complex(real (x) + real （y），imag (x) + imag (y)); //typename //创建一个临时对象来放加法的结果
}
```

```cpp
{
    int(7);
    complex c1(2,1);
    complex c2;
    complex();//临时对象
    complex(4,5);
    //在这一行就结束了
}
```
</details></div>

## 16. 复习
<details><summary>complex.h</summary><div>

```cpp
#ifndef __COMPLEX__
#define __COMPLEX__

class complex{
public:
    complex(double& r = 0, double& i = 0):re(r),im(i){}
    complex& operator += (const complex&);
    complex& operator << (ostream&, const complex&);
    double real() const {return re;}
    double imag() const {return im;}
private:
    double re, im;
    friend complex& __doaple(complex*, const complex&);
};
#endif
```
</details></div>

<details><summary>complex.cpp</summary><div>

```cpp
inline complex& __doapl(complex* ths, const complex& r){
    ths->re += r.im;
    ths->im += r.im;
    return *ths;
} 
inline complex& complex::operator += (const complex& r){
    return __doapl(this, r);
}

inline complex operator + (const complex& x, const complex& y){
    return complex(real (x) + real（y），imag (x) + imag (y));
}

inline complex operator + (const complex& x, double y){
    return complex(real (x) + y，imag (x));
}

inline complex operator + (double x, const complex& y){
    return complex(x + real （y），imag (y));
}

inline complex& complex::operator << (ostream& os, const complex& x){
    return os << "(" << real(x) << "," << imag(x)<< ")";
}
```
</details></div>

## 17. Classes 的两个经典分类
- Class without pointer members
- Class with pointer members

### 17.1. String class

<details><summary>string.h</summary><div>

```cpp
#ifndef __MYSTRING__
#define __MYSTRING__

class String{

};
String::function()
Global-function()
#endif
```
</details></div>
<details><summary>string-test.cpp</summary><div>

```cpp
int main(){
    String s1();
    String s2("Hello);
    String s3{s1}; //拷贝构造！！！
    cout << s3 << endl;
    s3 = s2; //拷贝赋值！！！
    cout << s3 << endl;
}
```
</details></div>

### 17.2. Big Tree
<details><summary>三个特殊函数</summary><div>
class带指针，就本能用默认的拷贝构造函数，应该自己写拷贝构造函数，拷贝赋值函数，析构函数。

class with pointer 多半要用动态内存分配。
在未定义构造函数时，系统会调用默认的拷贝函数，这个就叫浅拷贝。当数据成员中没有指针，浅拷贝是可行的；如果有指针，则会指向同一地址，当对象结束时，会调用两次析构函数，从而导致指针悬挂。

```cpp
class String{
public:
    String(const char* cstr{});//构造函数
    String(const String& str);//拷贝构造
    String& operator = (const String& str);//拷贝赋值
    ~String();//析构函数
    char* get_c_str() const{return m_data}
private:
    char* m_data;
}
```
</details></div>

### 17.3. ctor和dtor
<details><summary>构造函数和析构函数</summary><div>

```cpp
inline String::String(const char* cstr{}){//深拷贝
    if(cstr){
        m_data = new char[strlen(cstr)+1]; 
        strcpy(m_data, cstr);
    }
    else{
        m_data = new char[1];
        *m_data = '\0';
    }
}
inline String::~String(){
    delete[] m_data;
}
```
</details></div>

### 17.4. copy construct
<details><summary>深拷贝</summary><div>

```cpp
inline String::String(const String& str){
    m_data = new char[strlen(str.m_data) + 1];
    strcpy(m_data, str.m_data);
}
```
```cpp
{
    String s1{"Hello"};
    String s2(s1);
}
```
</details></div>

### 17.5. copy assignment operator
<details><summary>拷贝赋值函数</summary><div>

```cpp
inline String& String::operator = (const String& str){
    if(this == &str) //检查这两个指针是否指向同一个地方
        return *this; //检测自我赋值
    
    delete[] m_data;
    m_data=new char[strlen(str.m_data)+1];
    strcpy(m_data, str.m_data);
    return *this;
}
```
</details></div>

### 17.6. output
<details><summary>函数output</summary><div>

```cpp
#include<iostream.h>
ostream& operator<<(ostream& os, const String& str){
    os << str.get_c_str();
    return os;
}
```
```cpp
{
    String s1("hello ");
    cout << s1;
}
```
</details></div>

## 18. 栈与内存
- stack: 它存在于某作用域（scope）的一块内存空间（memory space）。例如当你调用函数，函数本身即会形成一个stack用来放置它所接收的参数，以及返回地址。

在函数本体（function body）内声明的任何变量，其所使用的内存块都取自上述stack。

- Heap：或称system heap，是指由操作系统提供的一块global内存空间，程序可动态分配（dynamic allocated)从某中获得若干区块（blocks）。
<details><summary>stack和heap</summary><div>

```cpp
class Comples{};

{
    Complex c1(1,2);// -> c1 所占用的空间来自stack，当c1离开scope之后会自动消失。这种作用域内的object，被称为auto object
    Complex *p = new Complex(3); //Complex（3）是个临时对象，其所占用的空间是以new自heap动态分配而得，并由p指向。
}
```
</details></div>

<details><summary>static local objects 的生命周期</summary><div>

```cpp
{
    static Complex c2(1,2); // c2 被称为static object，其生命在作用域结束之后仍然存在，直到整个程序结束。
}
```
</details></div>

<details><summary>global objects 的生命周期</summary><div>

```cpp
class Complex{};
Complex c3(1,2); //c3 被称为global object，其生命在整个程序结束之后才结束。你也可以把它视为一种static object，其作用域是整个程序。

int main{
    
}
```
</details></div>

<details><summary>heap objects 的生命周期</summary><div>

```cpp
class Complex{};

{
    Complex* p = new Complex;

    delete p;//其生命在这结束
}//如果没有delete，当离开这个作用域之后这个指针p会自动消亡，但会留下来那块空间
```
</details></div>

<details><summary>new：先分配memory，再调用ctor</summary><div>

```cpp
Complex* pc = new Complex(1,2);

编译器转化为：
Complex* pc;
1. void* mem = operator new(sizeof(Complex));//分配内存, operator new 是一种函数// sizeof(8byte)
2. pc = static_cast<Complex*>(mem);//转换类型
3. pc->Complex::complex(1,2);//构造函数

1. -> 其内部调用malloc(n)
3. -> Complex::Complex(pc, 1,2); //构造函数是成员函数，所以有this指针
```

```cpp
class Complex{
public:
// pc 指向一个动态内存空间{double，double}
    complex(double r = 0, double i = 0):re(r),im(i){} // 3.构造函数
private:
    double re;
    double im;
};
```

```cpp
String* ps = new String("Hello");

String* ps;
1. void* mem = operator new(sizeof(String));//分配内存
2. pc = static_cast<String*>(mem);//转换类型
3. pc->String::String("Hello");//构造函数

1. -> 其内部调用malloc(n)
3. -> String::String(pc, "Hello");
```
</details></div>

<details><summary>delete: 先调用析构函数，再释放memory</summary><div>

```cpp
Complex* pc = new Complex(1,2);
delete pc;

编译器转化为：
Complex* pc;
1. Complex::~Complex(pc); //析构函数
2. operator delete(pc); //释放内存 //operator delete也是一个特殊函数

2. -> 其内部调用free(pc)
```

```cpp
class String{
public:
    ~String(){delete[] m_data;} // --> 1.析构函数， 2. 删除m_data
private:
    char* m_data;
};
```
</details></div>

### 18.1. 动态分配所得的内存块（memory block）

![dynamic memory block](images/memory_block.png)

### 18.2. 动态分配所得的array

![array dynamic memory block](images/memory_block2.png)

### 18.3. array new 一定要搭配array delete

![new and delete](images/array_new_delete.png)

### 18.4. 复习String类的实现过程
<details><summary>String Class</summary><div>

```cpp
class String{
public:
    String(const char* cstr{0});
    String(const String& str);
    String& operator= (cosnt String& str);
    ~String();
    char* get_c_str() const {return m_data};
private:
    char* m_data;
};
```
</div></details>

<details><summary>define construct function</summary><div>

```cpp
inline String::String(const char* cstr{0}){
    if(cstr){
        m_data = new char[strlen(cstr)+1];//+1 is for \0
        strcpy(m_data, cstr);
    }
    else{
        m_data = new char[1];
        *m_data = '\0';
    }
}

String::~String(){
    delete[] m_data;
}
```
</div></details>

<details><summary>copy construct function</summary><div>

```cpp
inline String::String (const String& str){
    m_data = new char[strlen(str.m_data) + 1];
    strcpy(m_data, str.m_data);
}
```
</div></details>

<details><summary>copy assignment operator function</summary><div>

```cpp
inline String& String::operator= (const String& str){
    if(this == &str){
        return this;
    }
    delete[] m_data;
    m_data=new char[strlen(str.m_data)+1];
    strcpy(m_data, str.m_data);
    return *this;
}
```
</div></details>

### 18.5. 进一步补充：

<details><summary>static expand</summary><div>

```cpp
class Account{
public:
	static double m_rate;//静态数据的声明
	static void set_rate(const double& ) { m_rate = x;}//静态函数没有this指针，只能处理静态的数据
};
double Account::m_rate = 8.0;//静态数据的定义

int main(){
	Account::set_rate(5.0);//通过class name 调用
	Account a;
	a.set_rate(7.0);//通过object 调用
}
```

</div></details>


<details><summary>Singleton单件模式</summary><div>

```cpp
class A{
public:
    static A& getInstance(return a;);//让外界访问private函数的接口，调用这里面的函数
    setup(){...}
private:
    A();//不让外界创建构造函数的对象
    A(const A& rhs);
    static A a; //就只有一个静态对象
};
```

</div></details>

<details><summary>Meyers Singleton</summary><div>

```cpp
class A{
public:
    static A& getInstance();
    setup(){...}
private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance(){
    static A a;//把这个放在函数里的好处在于，只有调用这个函数时才会生成这个静态对象。直至程序结束。
    return a;
}
```

</div></details>

<details><summary>function template</summary><div>
template

```cpp
template <class T>
inline const T& min(const T& a, const T& b){
	return b < a ? b ： a;
}
```
class
```cpp
class stone{
public:
	stone (int w, int h, int we):_w(w), _h(h), _weight(we){}
	bool operator< (const stone& rhs) const {
		return _weight < rhs._weight;
	}
private:
	int _w, _h, _weight;
}；
```
main
```cpp
{
	stone r1(2,3), r2(4,3), r3;
	r3 = min(r1, r2);//编译器会对函数模板进行参数推导（argument deduction)所以不用<class>
}
```

</div></details>


<details><summary>using directive</summary><div>

```cpp
using namespace std;
```
</div></details>


<details><summary>using declaration</summary><div>

```cpp
using std::cout;

```
</div></details>

## 19. Composition, has-a

<details><summary>复合</summary><div>

```cpp
template <class T, class Sequence = deque<T>>
class queue{
protected:
	Sequence c;
public:
	//以下完全利用c的操作函数完成
	bool empty()
	size_type size() const {return c.size();}
	reference front() {return c.front;}
	reference back(){return c.back();}
	//deque 是两端可进出，queue是末端进前端出（先进后出）
	void push(const value_type& x) {c.push_back(x);}
	void pop(){c.pop_front();}

};
```
</div></details>

<details><summary>Adapter Method</summary><div>
queue 拥有 deque，queue的所有功能都让deque来做
两者的生命周期是一样的。同步。
queue --> deque

```cpp
template <class T>
class queue{//容器中拥有另一个deque容器
protected:
	deque<T> c;
public:
	//以下完全利用c的操作函数完成
	bool empty()
	size_type size() const {return c.size();}
	reference front() {return c.front;}
	reference back(){return c.back();}
	//deque 是两端可进出，queue是末端进前端出（先进后出）
	void push(const value_type& x) {c.push_back(x);}
	void pop(){c.pop_front();}

};
```
</div></details>

### 19.1. 复合关系下的构造和析构

Container --> Component

- 构造由内而外
Container的构造函数首先调用Component的default构造函数，然后执行自己。

```cpp
Container::Container(): Component(){};

```

- 析构由外而内
Container的析构函数首先执行自己，然后才调用Component的析构函数。

```cpp
Container::~Container(){ ~Component()};
```
## 20. Delegation Composition by reference

<details><summary>委托</summary><div>
用指针相连叫委托
生命周期不同步。
String --> StringRep
String.hpp

```cpp
class StringRep;
class String{
public:
	String();
	String(const char* s);
	String(const String& s);
	String& operator=(const String& s);
	~String();
private:
	StringRep* rep;//point to implimentation(pimpl)即使这里用指针我们也说这是Composition by reference
};
```

String.cpp
```cpp
#include "String.hpp"
namespace{
class StringRep{
	friend class String;
	StringRep(const char* s);
	~StringRep();
	int count;
	char* rep;
	};
}
String::String(){}
```
</div></details>

## 21. Inheritance, is-a
<details><summary>继承</summary><div>

```cpp
struct _List_node_base{
	_List_node_base* _M_next;
	_List_node_base* _M_prev;
};
template<class _Tp> struct _List_node : public _List_node_base{
	_Tp _M_data;
};
```
</div></details>

### 21.1. Inheritance关系下的构造和析构
- 构造由内而外
Derived的构造函数首先调用Base的default构造函数，然后才执行自己。

```
Derived::Derived():Base(){};
```
- 析构由外而内
Derived的析构函数首先执行自己，然后才调用Base的析构函数。

```
Derived::~Derived(){};
```
base class的dtor必须是virtual,否则会出现undefined behavior

## 22. Delegation + Inheritance
<details><summary>Composite Method</summary><div>

```cpp
class Primitive: public Component{
public:
	Primitive(int val):Component(val){}
};
```
```cpp
class Compnent{
int value;
public:
	Component(int val){value = val;}
	virtual void add (Component*){}
};
```
```cpp
class Composite: public Component{
	vector<Component*>c;
public:
	Composite(int val):Component(val){
	c.push_back(elem);
	}
};
```
</div></details>

![prototype method](images/prototype_method.png)
<details><summary>Prototype Method</summary><div>
Inherited class

```cpp
#include <iostream>
enum imageType{
LAST, SPOT
};
class Image{
public:
	virtual void draw() = 0;
	static Image* findAndClone(imageType);
protected:
	virtual imageType returnType() = 0;
	virtual Image* clone() = 0;
	// As each subclass of Image is declared, it registers its prototype
	static void addPrototype(Image* image){
		_prototypes[_nextSlot++] = image;
	}
private:
	//addPrototype() saves each registered prototype here
	static Image* _prototypes[10];
	static int _nextSlot;
};
Image* Image::_prototypes[];
int Image::nextSLot; //definition

```
```cpp
//client calls this public static member function when it needs an instance of an image subclass
Image* Image::findAndClone(imageType type){
	for(int i{}; i<_nextSlot; i++)
		if(_prototypes[i]->returnType() == type)
			return _prototypes[i]->clone();
}

```
Derived class

```cpp
class LandSatImage: public Image{
public:
    imageType returnType(){
        return LSAT;
    }
    void draw(){
        cout << "LandSatImage::draw" << _id << endl;
    }
    //When clone() is called, call the one-argument ctor with a dummy
    Image* clone(){
        return new LandSatImage(1);
    }
protected:
    //This is only called from clone()
    LandSatImage(int dummy){
        _id = _count++;
    }

private:
    //Mechanism for initializing an Image subclass - this causes the defult ctor to be called, which registers the subclass's prototype
    static LandSatImage _landSatImage;
    // This is only called when the private static data member is inited 
    LandSatImage(){
      addPrototype(this);
    }

    //Nominal "state" per instance mechanism
    int _id;
    static int _count;
};
//Register the subclass's prototype
LandSatImage LandSatImage::_landSatImage;
//Initialize the "state" per instance mechanism
int LandSatImage::_count = 1;

```

```cpp
class SpotImage: public Image{
public:
    imageType returnType(){
        return SPOT;
    }
    void draw(){
        cout <<"SpotImage::draw" << endl;
    }
    Image* clone(){
        return new SpotImage(1);
    }
protected:
    SpotImage(int dummy){
        _id = _count++;
    }
private:
    static SpotImage _spotImage;
    SpotImage(){
        addPrototype(this);
    }
    int _id;
    static int _count;
};
SpotImage SpotImage::_spotImage;
int SpotImage::_count = 1;
```
</div></details>


## 23. Conversion Function
<details><summary>转换函数</summary><div>

```cpp
class Fraction{
public:
    Fraction(int num, int den=1)
    : m_numerator(num), m_denominator(den){}
    operator double() const{
        return (double)(m_numerator / m_denominator);
    }
private:
    int m_numerator;
    int m_denominator;
}
```
```cpp
Fraction f(3,5);
double d = 4 + f;//调用double（）将f转换为0.6 
```
</div></details>

## non-explicit-one-argument ctor
<details><summary>只具有一个实参</summary><div>

```cpp
class Fraction{
public:
    Fraction(int num, int den=1)
    : m_numerator(num), m_denominator(den){}
    Fraction operator+(const Fraction& f){
        return Fraction(...);
    }
private:
    int m_numerator;
    int m_denominator;
}；
```
```cpp
Fraction f(3,5);
double d2 = f + 4; //调用non-explicit ctor将4转换为4/1
```
</div></details>

## Conversion Function vs. non-explicit-one-argument ctor
<details><summary>并存Error</summary><div>

```cpp
class Fraction{
public:
    Fraction(int num, int den=1)
    : m_numerator(num), m_denominator(den){}
    operator double() const{
        return (double)(m_numerator / m_denominator);
    }
    Fraction operator+(const Fraction& f){
        return Fraction(...);
    }
private:
    int m_numerator;
    int m_denominator;
}；
```
```cpp
Fraction f(3,5);
double d2 = f + 4; //Error
```
</div></details>

## explicit-one-argument ctor
<details><summary>并存Error</summary><div>

```cpp
class Fraction{
public:
    explict Fraction(int num, int den=1)
    : m_numerator(num), m_denominator(den){}
    operator double() const{
        return (double)(m_numerator / m_denominator);
    }
    Fraction operator+(const Fraction& f){
        return Fraction(...);
    }
private:
    int m_numerator;
    int m_denominator;
}；
```
```cpp
Fraction f(3,5);
double d2 = f + 4; //Error conversion from double to Fraction
```
</div></details>

## Pointer-like classes关于智能指针

shared_pt多个指针指向相同的对象。shared_ptr使用引用技术，每一个shared_ptr的拷贝都指向相同的内存。每使用他一次，内部的引用计数加1，每析构一次，内部的引用计数减1，直到减为0时，自动删除所指向的堆内存。shared_ptr内部的引用计数时线程安全的，但是对象的读取需要加锁。
<details><summary>shared pointer</summary><div>

```cpp
template<class T>
class shared_ptr{
public:
	T& operator* () const{ return *px;}
	T* operator-> () const{return px;}
	shared_ptr(T* p): px(p){}
private:
	T* px;// px --> T object
	long* pn;
};
```
</div></details>

<details><summary>iterator</summary><div>
```cpp
template<class T, class Ref, class Ptr>
struct __list_iterator{
	typedef __list_iterator<T, Ref, Ptr> self;
	typedef Ptr pointer;
	typedef Ref reference;
	typedef __list_node<T>* link_type;
	link_type node;
	bool operator== (const self& x) const {return node==x.node;}
	bool operator!= (const self& x) const {return node!=x.node;}

	reference operator*() const {return (*node).data;}
	pointer operator->() const {return &(operator*());}//return &((*node).data)

	self& operator++(){node = (link_type)((*node).next; return node;}
	self operator++(int){self tmp = *this; return tmp;}
	self& operator--(){node = (link_type)((*node).perv); return node;}
	self operator--(int){self tmp = *this; --*this; return tmp;}
};
```
```cpp
template<class T>
struct __list_node{
	void* prev;
	void* next;
	T data;
};
```
```cpp
list<Foo>::iterator ite;

*ite; //获得一个Foo的对象，调用*重载，返回解引用后的node.data的值
ite->method();//迭代器调用Foo类中的method函数
	      //相当于（*ite).method();
             //相当于（&(*ite))->method();
```
</div></details>

## Function-like classes仿函数
<details><summary>仿函数</summary><div>

```cpp
template <class T>
struct identity{
	const T& coperator() (const T& x) const {return x;}
};
template<class Pair>
struct select1st{
	const typename Pair::first_type& operator()(const Pair& x) const {return x.first;}
};
template <class Pair>
struct select2nd{
	const typename Pair::second_type& operator() (const Pair& x) const {return x.second;}
};

```
```cpp
template <class T1, class T2>
struct pair{
	T1 first;
	T2 second;
	pair(): first(T1()), second(T2()){}
	pair(const T1& a, const T2& b)
		: first(a), second(b){}
};
```
</div></details>

## member template
<details><summary>成员模板</summary><div>
```cpp
template<class T1, class T2>
struct pair{
	typedef T1 first_type;
	typedef T2 second_type;
	T1 first;
	T2 second;
	pair(): first(T1()), second(T2()){}
	pair(const T1& a, const T2& b): first(a), second(b){}

	template<class U1, class U2>
	pair(const pair<U1, U2>& p): first(p.first), second(p.second){}
};
```
```cpp
class fish{};
class samon: public fish{};

class bird{};
class chicken: public bird{};

pair<samon, chicken> p;
pair<fish, bird> p2(p);

pair<fish, bird> p2(pair<samon, chicken>());

```
```cpp
template<class T1, class T2>
struct pair{
	T1 fish;
	T2 bird;
	pair(): fish(T1()), bird(T2()){}
	pair(const T1& a, const T2& b): fish(a), bird(b){}

	template<class U1, class U2>
	pair(const pair<U1, U2>& p): fish(p.fish), bird(p.bird){}
}; 
```

</div><details>

<details><summary>shared_ptr member template</summary><div>
```cpp
template<typename _Tp>
class shared_ptr: public __shared_ptr<_Tp>{
	template<typename _Tp1>
	explicit shared_ptr(_Tp1* __p): __shared_ptr<_Tp>(__p){}
};

```
```cpp
Base1* ptr = new Derived1; //up-cast

shared_ptr<Base1> sptr(new Derived1);
```
</div><details>

## specialization 模板特化

<details><summary>模板特化</summary><div>
模板泛化
```cpp
template<class Key>
struct hash{};

```
模板特化
```cpp
template<>
struct hash<char>{
	size_t operator()(char x) const {return x;}
};

template<>
struct has<int>{
	size_t operator()(int x) const{return x;}
};

template<>
struct hash<long>{
	size_t operator()(long x) const{return x;}
};
```
</div><details>

## partial specialization

<details><summary>个数上偏特化模板</summary><div>

```cpp
template<typename T, typename Alloc=...>
class vector{
...
};
```
```cpp

template<typename Alloc=...>
class vector<bool, Alloc>{ // T绑定bool

};
```
</div></details>

<details><summary>范围上偏特化模板</summary><div>
```cpp
template <typename T>
class C{
//如果不用指针就用这个模板
};
```
```cpp
template <typename U>
class C<U*>{ //如果用指针就用这个模板

};
```
```cpp
C<string> obj1; //使用没有指针的模板
C<string*> obj2;
```
</div></details>


<details><summary>双模板参数</summary><div>
```cpp
template<typename T, template <typename T> class Container>
class XCLs{
private:
	Container<T> c;
public:
...
};
```
```cpp
template<typename T>
using Lst = list<T, allocate<T>>;
```
```cpp
x XCls<string, list> mylst1;
o XCls<string, Lst> mylst2;
```
</div></details>

## 确认支持的c++版本
<details<summary>确认</summary><div>
```cpp
#include <iostream>
int main(){
	std::cout << __cplusplus << std::endl;
}
```
</div></details>

## variadic templates(since C++11)
<details><summary>可变模板参数</summary><div>
```cpp
void print(){}

template<typename T, typename... Types>
void print(const T& firstArg, const Types&... args){
	cout << firstArg << endl;
	print(args...);
}
```
```cpp
print(7.5,"hello",bitset<16>(377),42);
```
```
7.5
hello
0000000101111001
42
```
</div><details>

## reference
<details><summary>参考</summary><div>
```cpp
int x{};//初始化x为0
int* p = &x; //把x的地址带入p中，而p是一个指向int的指针
int& r = x; //r代表x。r是一个对x的参考
int x2 = 6;
r = x2; //r不能重新代表其他变量。现在改变了r的值，同时也改变了x为5.

```
</div></details>
## reference的常见用途
<detals<summary>常见用途</summary><div>
```cpp
void func1(cls* pobj){pobj->xxx();}
void func2(cls vobj){vobj.xxx();}
void func3(cls& robj){robj.xxx();}

cls obj;
func1(&obj);
func2(obj);
func3(obj);
```
```
参考通常不用于声明变量，而是用于参数类型和返回类型的描述。

```
以下被视为“same signature”相同签名，二者不能同时存在：
```cpp
double imag(const double& im) {}
double imag(const double im) ... {}//ambiguity 歧义
//当调用函数是编译器不知是哪个函数
//如果在...处加入const,两者则不是相同签名
```
</div></details>
