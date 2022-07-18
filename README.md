<p style="text-align: center;">面向对象高级编程（侯捷视频笔记）</p>

- [1. Object Based vs. Object Oriented](#1-object-based-vs-object-oriented)
- [2. Header中的防卫是声明](#2-header中的防卫是声明)
- [3. Header 的布局](#3-header-的布局)
- [4. class 的声明](#4-class-的声明)
- [5. class template 简介](#5-class-template-简介)
- [6. inline 函数](#6-inline-函数)
- [7. access level](#7-access-level)
- [constructor（ctor,构造函数）](#constructorctor构造函数)
- [ctor 可以有很多个-overloading](#ctor-可以有很多个-overloading)

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

## constructor（ctor,构造函数）
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

## ctor 可以有很多个-overloading
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