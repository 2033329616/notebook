---
title: C++学习笔记 
tags: 日积月累,C++
grammar_cjkRuby: true
---
2018-2-3
[TOC]

### **1. C++的头文件和源文件**
`.h`头文件只需预处理，`.cpp`才需要编译，一般在头文件里声明一个类及类的的方法，在源文件里实现函数的**定义**与**实现**，这样可以将声明和定义分开，利于构建大规模的程序。
`hello.h`文件如下：
```cpp
#pragma once

#include "iostream"
#include "string"
using namespace std;

class Hello
{
public:                   //共有函数及成员定义的关键词
	Hello();             //声明构造函数，构造函数最好不要有默认参数
	~Hello();            //声明析构函数
	void talk(int number=0);    //声明共有成员函数,参数给出默认值  

private:            //私有函数及成员定义的关键词
	string content; //声明一个字符串变量
	int num;
};
```
`hello.cpp`文件如下：
```cpp
#include "hello.h"

//类的定义与实现
Hello::Hello()      //构造函数定义
{
	content = "这是私有成员变量";      //在构造函数中为成员变量赋值
	cout << "这是构造函数" << endl;

}

Hello::~Hello()       //析构函数定义
{
	cout << "这是析构函数" << endl;
}

void Hello::talk(int number)    //成员函数定义
{
	num = number;
	cout <<"content:"<< content << endl;  //头文件中包含了string才能使用该句
	cout << "num:" << num << endl;
}
```
主函数main.cpp如下：
```cpp
#include "iostream"
#include "hello.h"

using namespace std;

int main()
{
	Hello robot;      //实例化一个对象
	robot.talk(233);  //调用成员函数，默认参数为0
	return 0;
}
```
