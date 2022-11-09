---
title: 學習 C++ const
catalog: true
date: 2022-11-09 14:44:32
subtitle: const 修飾不能改動的實例
header-img: const.jpg
tags:
- C++
- 整理文章
- 學習
categories:
- C++ 學習
---
# const in C++

const 用在兩個地方，一個是修飾普通變數和指標，另一個是修飾函數。

---

## 修飾變數和指標
### 1. 修飾變數
const 修飾變數有兩種寫法，兩種寫法所代表的意思是一樣的，指 const 修飾類型為 TYPE 的變數 a 是不可變的。
- const TYPE a;
- TYPE const a;


### 2. 修飾指標
修飾指標較為複雜，要考慮到他修飾的是指向的內容還是指標本身。
1. const char *ptr;
2. char const *ptr;
3. char * const ptr;
4. const char* const ptr;

1 和 2 代表 ptr 的內容為常量不可變；3 代表 ptr 指針本身為常量不可變；4 代表指標本身和內容兩者皆為常量不可變。

區別方法：
沿著 * 號切割，如果 const 位於 * 的左側，則 const 是修飾指標所指向的變數；如果 const 位於 * 的右側，const 就是修飾指標本身。 

---

## 修飾函數
### 1. 修飾函數參數
函數中不能修改參數的值。例如 `void function(const char* a);`

### 2. 修飾函數返回值
參數返回的內容或指標不可以修改。
例如 `const int * fun()`，
儲存函數返回時要加上const `const int *ptr = fun();`，即指標內容不可變。

`int* const fun()` 代表指針本身不可變，
呼叫時也要加上 const `int * const ptr = fun();`。

### 3. 修飾成員函數
該成員函數只能取得資料，不可以改變資料 (read-only)，也不能呼叫任何不是 const 的 member function。
const 放在函數宣告的最後：`void function()const;`。


[geeksforgeeks](https://www.geeksforgeeks.org/const-member-functions-c/) 參考程式碼
```cpp
// Example: of Constant member function
#include<iostream>
using namespace std;

class Demo
{
	int x;

	public:
	
	void set_data(int a)
	{
		x=a;
	}

	int get_data() const    //constant member function
	{
		++x;    // Error while attempting to modify the data member
		return x;
	}

};


main()
{
	Demo d;
	d.set_data(10);
	cout<<endl<<d.get_data();

	return 0;
}
```

此外，const 物件只能呼叫 const function。
> When a function is declared as const, it can be called on any type of object. Non-const functions can only be called by non-const objects.

--- 

### 參考資料
本文內容為以下參考資料的彙整
[const 放置位置的意義](https://blog.xuite.net/tsai.oktomy/program/65131235#)
[Cpp - const member function](https://kwcheng0119.pixnet.net/blog/post/43190324-cpp---const-member-function)
[Const member functions in C++ - geeksforgeeks](https://www.geeksforgeeks.org/const-member-functions-c/) 