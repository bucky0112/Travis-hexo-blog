---
title: 執行環境：程式執行
tags:
  - javascript
  - 程式執行
date: 2020-05-19 17:31:40
categories: JavaScript 的怪奇物語
keywords:
- javascript
- JavaScript 全攻略：克服 JS 的奇怪部分
decription: JavaScript 執行程式碼的運作情形。
---

JavaScript 在產生執行環境時，會有兩個階段。

1. 第一階段是創造，會設定變數和函數到記憶體中。
2. 第二階段則是執行。

而這篇文章則會提到執行的部份。
<!--more-->

當在創造階段時，都已經設定好所有東西，到執行這個階段就很單純，只是逐行執行自己寫好的程式碼而已。進行編譯跟轉換後，讓電腦看得懂，這樣就結束了。

不免俗的來看一下程式碼，當執行以下的程式碼後，會如何運作呢？

```
function b() {
  console.log('Test');
}

b();            

console.log(a);

var a = 'Hello';

console.log(a)

```

首先呼叫 b()，沒有什麼問題，會執行 function 內的程式碼，印出 'Test'。

第一個 `console.log(a)`，由於在創造階段時，先給 a 一個 undefined，然後執行，所以就印出 undefined。

第二個 `console.log(a)`，因為有給 a 一個值，所以執行後，就印出 'Hello'。

再看其他例子，下面 4 個 `console.log` 會個別印出什麼？

```
var a;

console.log(a);

console.log(b);

var b = 'hello';

console.log(b);

console.log(c);
```

1. a 沒有值，所以 undefined。
2. b 到這行還沒有值，所以一樣先給 undefined。
3. b 這時候已經有給值了，所以印出 'hello'。
4. c 並沒有宣告變數，所以在記憶體中找不到 c，所以會是錯誤，c is not defined。