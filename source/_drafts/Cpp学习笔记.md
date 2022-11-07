---
title: C++学习笔记
tags:
---

### cpp 基础 杂记

- 数组

``` cpp
// 定义
int score[10];
int score2[10] = { 100, 90,80,70,60,50,40,30,20,10 };
int score3[] = { 100,90,80,70,60,50,40,30,20,10 };
```
  - sizeof(arr) 获取整个数组所占空间
  - sizeof(arr[0]) 获取单个元素所占空间

- 指针

``` cpp
int  var = 1;

cout << &var << endl;

// 取地址

int*ip;// pointe 定义指针 内容为对应的地址 
ip = &var; // 设置ip为 var变量的指针

cout << *ip << endl; // 取值操作

```
其中数组比较特别，变量等同于数组的首项的地址

  - 指针与函数
``` cpp
//地址传递
void swap2(int * p1, int *p2)
{
	int temp = *p1;
	*p1 = *p2;
	*p2 = temp;
}
swap2(&a, &b); // & 取地址
// 地址传递可以做到修改传入的参数的值，默认情况下为值传递，不能做到对实参修改，但是地址传递可以

void bubbleSort(int * arr, int len)  //int * arr 也可以写为int arr[] 数组的地址也是名字
{ }
```

- 引用

``` cpp
int i = 17;   
int& r = i;
```
引用必须初始化，并且初始化后，不可以改变
此时 r变量和i变量相当是一样的，i的内容改变，r的内容也会改变，可以理解为变量的地址是一样的，赋值时相当于向同样的地址中存值

``` cpp
// 指针用法
void mySwap02(int* a, int* b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}
mySwap02(&a, &b);
// 引用对于函数
void mySwap03(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}
mySwap03(a, b); 
// 同样可以做到和指针一样的效果，对实参进行修改
```
引用的本质为指针常量，特殊用法，可以放到等号的左侧接受别的值，作为函数返回时，不能返回局部变量，但是可以返回静态变量引用，需要`static`修饰符

- 内存
  - C++中在程序运行前分为全局区和代码区
  - 代码区特点是共享和只读
  - 全局区中存放全局变量、静态变量、常量
  - 常量区中存放 const修饰的全局常量 和 字符串常量
- new 操作符可以手动开辟自己的数据空间(堆区)，通过delete释放


- 类 
可以通过`class`和`struct`定义类，默认权限`class`为私有，`struct`(结构体)为公共

class三种权限
  - 公共权限  public     类内可以访问  类外可以访问
  - 保护权限  protected  类内可以访问  类外不可以访问
  - 私有权限  private    类内可以访问  类外不可以访问

``` cpp
class Person
{
public:

	Person(int age)
	{
		//1、当形参和成员变量同名时，可用this指针来区分
		this->age = age; // this 为最后生成的对象
	}

	Person& PersonAddPerson(Person p)
	{
		this->age += p.age;
		//返回对象本身
		return *this;
	}

	int age;
};

// a->b  a 为指针
// a.b   a 为实体
```

- 友元
``` cpp
class Building
{
	//告诉编译器 goodGay全局函数 是 Building类的好朋友，可以访问类中的私有内容
	friend void goodGay(Building * building);

public:

	Building()
	{
		this->m_SittingRoom = "客厅";
		this->m_BedRoom = "卧室";
	}


public:
	string m_SittingRoom; //客厅

private:
	string m_BedRoom; //卧室
};
void goodGay(Building * building)
{
	cout << "好基友正在访问： " << building->m_SittingRoom << endl;
	cout << "好基友正在访问： " << building->m_BedRoom << endl;
}
// 可以访问到权限更大的属性
```