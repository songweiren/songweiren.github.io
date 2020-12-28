---
layout: post
title:  "C++ Template之多自定义参数实例化"
date:   2020-06-09 22:49:52 +0800
categories: C/C++
tags: C_or_CPP
---
简单写个示例：【VS2012编译运行通过】

头文件，定义一个类模板（含有虚函数）和需要用到的实例化参数类
```C++
#ifndef _PEOPLE_H
#define _PEOPLE_H
 
 
#include <iostream>
using namespace std;
 
class pq1;
class pq2;
 
template<class T,class D>
class people
{
public:
  T age;
  people()
  {}
  ~people()
  {}
  virtual void out()
  {
    cout<<"base"<<endl;
  }
  void print()
  {
    cout<<"base_print"<<endl;
  }
private:
 
};
 
class BlackP
{
public:
  BlackP();
  ~BlackP();
  void print();
  void b_print();
 
  static string area;
 
private:
 
};
 
BlackP::BlackP()
{
}
 
BlackP::~BlackP()
{
}
 
void BlackP::print()
{
  cout<<"b_self_print"<<endl;
}
void BlackP::b_print()
{
  cout<<"b_self_B_print"<<endl;
}
 
class  WhiteP
{
public:
   WhiteP();
  ~ WhiteP();
  void print();
  void w_print();
 
private:
 
};
 
 WhiteP:: WhiteP()
{
}
 
 WhiteP::~ WhiteP()
{
}
 
 void WhiteP::print()
 {
   cout<<"w_self_print"<<endl;
 }
 
 void WhiteP::w_print()
 {
   cout<<"w_self_W_print"<<endl;
 }
 
 class CommP
 {
 public:
   CommP();
   ~CommP();
 
 private:
 
 };
 
 CommP::CommP()
 {
 }
 
 CommP::~CommP()
 {
 }
#endif
```
CPP文件实例化类模板，并重写类模板中的虚函数
```C++
#ifndef _PEOPLE_H
#include"people.h"
#endif 
 
#include<string>
 
people<int,BlackP> p1;
people<double,WhiteP> p2;
 
string BlackP::area;
void people<int,BlackP>::out()
{
  cout<<"int_out"<<endl;
  //this->print();
  BlackP::area="africa";
  cout<<BlackP::area<<endl;//must include string.h if you want to cout string
 
  BlackP p;
  p.b_print();
}
void people<double,WhiteP>::out()
{
  cout<<"double_out"<<endl;
 
  WhiteP p;
  p.w_print();
}
 
 
int main()
{
  people<double,CommP> p0;
  p0.out();
  p0.print();
 
  p1.out();
  p1.print();
  
 
 
  p2.out();
 
  system("pause");
}
```