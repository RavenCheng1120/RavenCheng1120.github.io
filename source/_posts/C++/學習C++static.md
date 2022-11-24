---
title: 學習 C++ static
catalog: true
date: 2022-11-01 18:56:14
subtitle: static 關鍵字
header-img: static.jpg
tags:
- C++
- 整理文章
- 學習
categories:
- C++ 學習
---

# C++ 的 static 關鍵字
Static 出現的位置與定義，分為以下五種
- static 出現在 variable 之前，且該 variable 宣告在某個 function 之中，為區域變數
- static 出現在 variable 之前，且該 variable 並不在 function 中，為全域變數
- static 出現在 function 之前
- static 出現在 class 的 member variable 之前
- static 出現在 class 的 member function 之前

## 1. static 位於區域變數之前
如果我們在 function 中某變數之前加上 static，該變數就不會因為 function 結束而消失。作用跟全域變數很像，但不會給其他 cpp 檔案存取到變數，而且建構實例化是在程式執行到 static 那一行才開始。

```cpp=
#include <iostream>

void MyFunction(){
    static int num = 0;
    num++;
    std::cout << num << std::endl;
}
```
如果在 main() 呼叫 MyFunction() 五次，會回傳 
1
2
3
4
5


## 2. static 位於全域變數之前
static 放在全域變數之前，表示該變數無法被其他 cpp 檔用 `extern` 取用。用意是要讓全域變數只限定在該檔案內，而不是整個程式中， link 的過程中也不會有名字衝突的問題。

補充一下：**extern**，假如有一個變數要在多個檔案之間共用，extern 可以告訴 compiler 此變數的存在，但是並不是由目前的檔案做宣告。使用方法是`extern int a;`。

```cpp
#include <iostream>

static int num = 0;
```

## 3. static 位於 function 之前
static 放在函式之前，表示在 cpp 檔裡該函式無法被其他 cpp 檔用 `extern` 呼叫，跟前面 static 全域變數是差不多的。


## 4. static 位於 class 的 member variable 之前
static 的 member variable 是靜態成員變數 (static member variable)，他不屬於任何一個實體 (instance)，代表所有的實體共享該變數。
最經典的範例就是用來數這個 class 總共生成了多少個 instance。

```cpp
#include <iostream>
using namespace std;

class Entity {
private:
    static int num;

public:
    Entity() {
        num++;
        cout << "Number = " << num << endl;
    }
};

int Entity::num = 0; // initializing the static int

int main() {
    Entity e1;
    Entity e2;
    Entity e3;
    return 0;
}
```
輸出結果是
```
Number = 1
Number = 2
Number = 3
```

## 5. static 位於 class 的 member function 之前
跟 member variable 的概念相似，這種 function 叫做靜態成員函式 (static member function)，他不屬於任何一個實體 (instance)，代表不需要任何實體就可以呼叫該類別的成員函式。

```cpp
#include <iostream>
using namespace std;

class Entity {
private:
    static int num;

public:
    Entity() {
        num++;
        cout << "Number = " << num << endl;
    }

    static int GetNum(){
        return num;
    }
};

int Entity::num = 0; // initializing the static int

int main() {
    cout << "Call function: " << Entity::GetNum() << endl;
    Entity e1;
    Entity e2;
    Entity e3;
    cout << "Call function: " << Entity::GetNum() << endl;
    return 0;
}
```
輸出結果是
```
Call function: 0
Number = 1
Number = 2
Number = 3
Call function: 3
```

### 參考資料
[C/C++ static 的 5 種用法](https://shengyu7697.github.io/cpp-static/)
[C/C++ 中的 static, extern 的變數](https://medium.com/@alan81920/c-c-%E4%B8%AD%E7%9A%84-static-extern-%E7%9A%84%E8%AE%8A%E6%95%B8-9b42d000688f)
[Local Static in C++ - Cherno](https://www.youtube.com/watch?v=f7mtWD9GdJ4&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=48&ab_channel=TheCherno)