<p style="text-align: center;">面向对象高级编程（侯捷视频笔记）</p>

- [1. Object Based vs. Object Oriented](#1-object-based-vs-object-oriented)
- [2. Header中的防卫是声明](#2-header中的防卫是声明)
- [3. Header 的布局](#3-header-的布局)
- [4. class 的声明](#4-class-的声明)
- [5. class template 简介](#5-class-template-简介)

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