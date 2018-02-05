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
#pragma once       //预编译一次，防止出现重复包含头文件的情况
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
运行结果如下：

![运行结果][1]


### **2. C++中的L和_T的作用**
在一个字符串前加 `L`表示将ANSI字符串转换为unicode的字符串，每个字符占两个字节
```cpp
strlen("abc") = 3;
strlen(L"asd") = 6;
```
 _T宏可以把一个引号引起来的字符串，根据你的环境设置，使得编译器会根据编译目标环境选择合适的（Unicode还是ANSI）字符处理方式，如果你定义了UNICODE，那么_T宏会把字符串前面加一个L。这时 _T("ABCD") 相当于 L"ABCD" ，这是宽字符串。如果没有定义，那么_T宏不会在字符串前面加那个L，_T("ABCD") 就等价于 "ABCD"  
 
 ### **3.C++调用Python函数的方法**
 这里使用vs2015编译程序，首先要设置项目的属性，把python的include目录和libs目录包含到项目中，设置如下：
 
 ![项目的属性设置][2]
 python程序如下：
 ```python
 # python
def convertStrToNum(string):
	result = eval(string)
	return result
 ```
 这里的python程序当做模块来对待，文件夹名称为strToNum，文件名strToNum.py，在同一个文件夹下有__init__.py文件(内容可为空)来表示模块。
 
 在C++程序中调用python函数时注意：python语句写成模块的形式；将python模块的路径添加到系统中，否则c++程序无法找到python模块，最好使用相对路径
 
 c++程序如下
 ```cpp
 #include "iostream"
#include "Python.h"
#include "string"
#include "cstring"
using namespace std;

int main()
{
	Py_Initialize();
	//PyRun_SimpleString("x=eval('2 + 3*2')");    //运行单个python语句
	//PyRun_SimpleString("print(x)");
	//PyArg_Parse(x, "i");

	// 将Python工作路径切换到待调用模块所在目录，一定要保证路径名的正确性
	string path = ".\\strToNum";                  //相对路径，windows下
	string chdir_cmd = string("sys.path.append(\"") + path + "\")";
	const char* cstr_cmd = chdir_cmd.c_str();
	PyRun_SimpleString("import sys");
	PyRun_SimpleString(cstr_cmd);
	PyRun_SimpleString("import os");
	PyRun_SimpleString("print(os.getcwd())");
	PyRun_SimpleString("print(sys.path)");
	
	PyObject *pModule = NULL;
	PyObject *pFunc = NULL;
	PyObject *pResult = NULL;
	double result = 0;

	pModule = PyImport_ImportModule("strToNum");
	if (!pModule) // 加载模块失败
	{
		cout << "[ERROR] Python get module failed." << endl;
		return 0;
	}
	cout << "[INFO] Python get module succeed." << endl;

	pFunc = PyObject_GetAttrString(pModule, "convertStrToNum");
	if (!pFunc || !PyCallable_Check(pFunc))  // 验证是否加载成功
	{
		cout << "[ERROR] Can't find funftion (test_add)" << endl;
		return 0;
	}
	cout << "[INFO] Get function (test_add) succeed." << endl;

	PyObject *pArgs = PyTuple_New(1);               //新建数组保存参数
	PyTuple_SetItem(pArgs, 0, Py_BuildValue("s", "1 + 2*4.3 -1"));
	//PyObject *pArgs = Py_BuildValue("23");
	pResult = PyEval_CallObject(pFunc, pArgs);
	PyArg_Parse(pResult, "d", &result);  //转换为double
	cout << "result:" << result << endl;

	Py_Finalize();      //释放资源
	return 0;
}
 ```
 
 
 
 
 
 
 
 
 
 
 

  [1]: ./images/result.jpg ""

  [2]: ./images/c++%E8%B0%83%E7%94%A8python_3.jpg ""