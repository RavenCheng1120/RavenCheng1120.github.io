---
title: 學習 C++ Template
catalog: true
date: 2022-11-03 16:19:33
subtitle: 使用樣板，將資料型態參數化
header-img: template.jpg
tags:
- C++
- 整理文章
- 學習
categories:
- C++ 學習
---

# Templates in C++

以 template 方式建構 function 或 class 時，可以在執行時或宣告時才定義 data type。

編譯器透過類似巨集代換的方式，根據樣板內容產生實際的程式碼。參數型態可用關鍵字 class 或 typename 表示泛用型態。

## 1. Template function

```c++
#include <iostream>

template<typename T> // or template<class T>
void Print(T value){
  std::cout << value << std::endl;
}

int main(){
  Print<int>(5); // or Print(5);
  Print("Hello");
  Print(5.5f);
}
```

這樣子就不需要為了不同的參數型態，複製並 overload Print 函式。

當 Print() 被呼叫後，template 才會存在，所以如果 Print()裡面有錯誤，只要不呼叫，compile 就不會出錯。



## 2. Template class

```c++
#include <iostream>

template<typename T, int N>
class CreateArr
{
private:
	T m_arr[N];
public:
	void GetSize()const{
    std::cout<< N << std::endl;
    return Nl
  }
};

int main(){
  CreateArr<int, 6> myArr;
}
```

可以讓資料型態（T）和 array 的大小（N）在 compile 的時候才決定。



### 參考資料
本文內容為以下參考資料的彙整
[C++ -Template 的教學小筆記](https://husking-studio.com/cpp-template-01/)
[C++ Template 筆記](https://www.rocksaying.tw/archives/3641717.html)
[Templates in C++ - Cherno](https://www.youtube.com/watch?v=I-hZkUa9mIs&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=54&ab_channel=TheCherno)
