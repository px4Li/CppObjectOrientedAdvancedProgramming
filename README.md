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
    Complex c1(1,2);// -> c1 所占用的空间来自stack，当c1离开scope之后会自动消失。
    Complex *p = new Complex(3); //Complex（3）是个临时对象，其所占用的空间是以new自heap动态分配而得，并由p指向。
}
```
</details></div>