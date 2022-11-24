---
title: Node.js exports 模組 
catalog: true
date: 2022-11-24 15:26:59
subtitle: Node.js 的 module.exports 和 export
header-img: export.jpg
tags:
- JavaScript
- 整理文章
- 學習
categories:
- JavaScript 學習
---
# Node.js 中的 module.exports 和 export
Node.js 裡的 module.exports (CommonJS 語法) 跟 export (ESM 語法) 容易混淆，之前實習寫的筆記也不知道丟去哪裡了，所以來記錄一下兩者的差異跟背後原理。
此文章是整理一些前輩的教學，參考網址在底下。

## 在 Node.js 使用模組
Node.js 有三種型別的模組：Core 模組、第三方模組、本地模組
- Core 模組: 跟著 Node.js 一起安裝的模組，例如 HTTP 模組
- 第三方模組: 通常使用 npm 額外安裝的模組，例如 Express 模組
- Local 模組: 本文重點，自己建立用於執行各種操作的模組

## Local Module Format
伺服器端兩個廣泛使用的匯出和匯入模組：CommonJS (CJS) 格式 和 ESM (ES6) 格式
* CommonJS: module.exports / exports 搭配 require
* ESM: export / export default 搭配 import

而 Node 應用的就是 CJS 規範，根據這個規範，每個文件就是一個模塊，有自己的作用域。
每個模塊內部，module 變量代表當前模塊，它的 exports 屬性（即 module.exports ）是對外的接口。加載某個模塊，其實是加載該模塊的 module.exports 屬性。

推薦去看 [這篇](https://segmentfault.com/a/1190000010426778) 和 [這篇文章](https://www.cnblogs.com/fayin/p/6831071.html)，解釋得很詳細。總結就是：
- require: Node.js 和 ES6 都支援
- export / import : 只有 ES6 支援的導出
- module.exports / exports: 只有 Node.js 支援的導出


## CommonJS Module
#### 首先是 module.exports 和 exports
在 node 執行一個文件時，會給這個文件內生成一個 exports 和 module，而 module 又有一個 exports 屬性，都指向同一塊 {} 內存區域。
```js
exports  = module.exports = {}
```

而 require 導出的內容是 module.exports 指向的內存塊內容，並不是 exports 指向的，雖然他們在文件中都指向同一塊內存，但**輸出還是以 module.exports 的指向為主**。

module.exports 和 exports 之間的區別就只是 exports 是 module.exports的引用，輔助後者添加內容用的。
可以盡量都用 module.exports 導出，然後用 require 導入，才不會搞混。


## ESM Module
#### 首先是導出 export 和 export default
在一個文件或模塊中，export、import 可以有多個，但 **export default 只能有一個**。
export 能直接導出變量表達式，像是 `export const a = '100';` ，而 export default 不行。

#### 至於導入也有分 import {a} from ... 和 import a from ...
通過 export 方式導出，在導入時要加 { }，export default 則不需要。
也可以用 `import * as testModule from ...` 做集合成對象導出。 


## 範例
最後來看一點例子吧。

1-1. module.exports 導出
```js
let x = 5;
let addX = function (value) {
    return value + x;
};
module.exports.x = x;
module.exports.addX = addX;
```

1-2. require 方法導入
```js
const example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6
```

2-1. export 方法導出
```js
var firstName = 'Mary';
var lastName = 'Anne';
var year = 1991;

export {firstName, lastName, year};
```

2-2. import 方法導入
```js
import { firstName, lastName, year } from './testEs6Export' ; 
```

### 參考資料
本文內容為以下參考資料的彙整
[Node JS 中的 module.exports by Isaac Tony](https://www.delftstack.com/zh-tw/howto/node.js/module.exports-in-node-js/)
[exports、module.exports和export、export default到底是咋回事 by 城南](https://segmentfault.com/a/1190000010426778)
[module.exports與exports，export與export default之間的關係和區別 by Liaofy](https://www.cnblogs.com/fayin/p/6831071.html)