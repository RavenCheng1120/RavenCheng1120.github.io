---
title: 在 Mac 的 VS Code 使用 C++
catalog: true
date: 2022-11-15 15:11:24
subtitle: 在 Mac 系統的 VS Code 中弄好 C++ 執行環境
header-img: vscode.jpg
tags:
- C++
- 學習
categories:
- C++ 學習
---
# 在 VS Code 使用 C++
### 1. 安裝 XCode 與 Visual Studio Code
官網安裝 VS Code: https://code.visualstudio.com/download
XCode 會提供所需的編譯器 compiler: Clang for XCode (macOS)

檢查是否安裝成功：
```
$ clang --version
Apple clang version 14.0.0 (clang-1400.0.29.202)
Target: x86_64-apple-darwin21.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin  
```


### 2. 在 VS Code 安裝 Extensions
點擊側邊的 Extensions 欄位，尋找下面三個套件安裝
- C/C++
- C/C++ extension for VS Code
- Code Runner
<img src="extensions.png" width="50%" alt="extensions"></img>


### 3. 建立專案
新增 helloworld.cpp，輸入以下程式碼
```cpp=
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
  vector<string> msg{"Hello", "C++", "World", "from", "VS Code!"};

  for (const string &word : msg)
  {
    cout << word << " ";
  }
  

  cout << endl;
}
```

### 4. 設定檔案 (使用 code runner 可跳過)
#### 4-1. c_cpp_properties.json
> 設定 Compiler 路徑以及 IntelliSense。

按下 fn 加 F1，選 C/C++: Edit Configurations (UI)
這裡可以設定 Compiler 路徑、intelliSense 模式、包含路徑（如果有使用 header files 但沒有放在 workspace 內，就要補充）等等
<img src="setting0.png" width="80%" alt="Edit Configurations"></img>
設定完成後，c_cpp_properties.json 會存在 .vscode 資料夾內。

另外，也可以不走以上步驟，按下 fn 加 F1，選 C/C++: Edit Configurations (JSON)，直接生成 Json 檔。
<img src="setting1.png" width="90%" alt="c_cpp_properties"></img>

c_cpp_properties.json 檔案內容如下
```Json
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "macFrameworkPath": [
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "macos-clang-x64"
        }
    ],
    "version": 4
}
```
編譯器路徑 compilerPath ，Extension 會指向系統安裝 C++ Compiler 的位置，如：/usr/bin/clang


#### 4-2. tasks.json
> Build 相關設定。

按下 fn 加 F1，輸入並選擇：Tasks: Configure Default Build Task。
<img src="setting2.png" width="90%" alt="Default Build Task"></img>

從 clang++ 建置使用中檔案
<img src="setting3.png" width="90%" alt="clang++"></img>

產出 task.json 檔，這裡可以更改 build 時使用的 args
```
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: clang++ 建置使用中檔案",
			"command": "/usr/bin/clang++",
			"args": [
				"-fcolor-diagnostics",
				"-fansi-escape-codes",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"detail": "編譯器: /usr/bin/clang++"
		}
	]
}
```
關於產出 launch.json（偵錯相關設定）和 task.json 要怎麼改，可以參考[這篇](https://ithelp.ithome.com.tw/articles/10228137)。



### 5. Run Code
按下右上角箭頭來執行程式
<img src="runCode.png" width="70%" alt="run code"></img>

如果遇到以下錯誤，代表 problemMatcher 沒有用正確的 C++ 版本進行判斷
```
test2.cpp:9:23: error: expected ';' at end of declaration
    vector<string> msg {"Hello", "C++", "World", "from", "VS Code", "and the C++ extension!"};
                      ^
                      ;
test2.cpp:11:29: warning: range-based for loop is a C++11 extension [-Wc++11-extensions]
    for (const string& word : msg)
                            ^
1 warning and 1 error generated.
```

就像直接用 `g++ helloworld.cpp`，沒有指定 C++ 版本的話，會使用 default version，就可能造成版本過低的問題。查看 default 版本的方法：https://stackoverflow.com/questions/46980383/how-to-determine-what-c-standard-is-the-default-for-a-c-compiler

解決辦法是開啟 setting：
<img src="codeSetting.png" width="70%" alt="open setting"></img>

尋找 code-runner: Executor Map，並進行編輯 settings.json (要有 code runner extensions 才能找到這個設定)
<img src="codeRunner.png" width="90%" alt="code Runner"></img>

尋找 cpp，並在 cd $dir && g++ 後方添加 -std=c++17，以指定 C++ 版本
<img src="codeRunnerJson.png" width="90%" alt="code Runner Json"></img>

儲存並重新 run code，問題就可以解決了。
<img src="result.png" width="100%" alt="Result"></img>


此外，另一種辦法是使用 terminal 呼叫 g++ 去編譯，而不是使用 VS Code 呼叫 clang++，整體流程會方便許多。可以參考 [GCC 編譯器基本使用教學與範例](https://blog.gtwang.org/programming/gcc-comipler-basic-tutorial-examples/)。


## Happy Coding!

--- 

### 參考資料
感謝以下文章的教學和幫助
[Mac VSCode Debugger always show error about ';' and ':'](https://shuwn.dev/2022/09/09/mac_vscode_debugger_always_show_error/)
[官方文件：Configure VS Code for Microsoft C++](https://code.visualstudio.com/docs/cpp/config-msvc)
[Day 28: 使用 VS Code 來開發 C++](https://ithelp.ithome.com.tw/articles/10228137)
