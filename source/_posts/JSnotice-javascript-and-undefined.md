---
title: JS notice： JavaScript 與 undefined
tags:
  - JavaScript
  - undefined
date: 2020-05-11 17:21:49
categories: JavaScript 的怪奇物語
keywords:
- undefined
- not defined
decription: undefined 與 not defined 的差異
---
本篇文章會稍微講解 undefined 與 not defined 的差異。
<!--more-->
## undefined
---

先來看一個例子：

```
var a = "Hello World";
console.log(a)  // "Hello World"
```

當宣告一個變數 a，並指定一個值給它時，再用 `console.log` 印出 a，不意外的得到賦予它的值。

但如果把值去掉，再次看 a 會得到什麼？

```
var a;
console.log(a)  // undefined
```

結果得到 undefined。

## not defined
---

那麼如果把 var 跟值去掉的話，只留下 a 會發生什麼事？

```
a;
console.log(a);
```

會發生錯誤，錯誤訊息為 a is not defined。
這邊則會有一個疑問，not defined 跟 undefined 是不同的嗎？

這個部份有在 [執行環境：創造與提升](https://bucky0112.github.io/bucky0112.github.io/2020/04/24/%E5%9F%B7%E8%A1%8C%E7%92%B0%E5%A2%83-%E5%89%B5%E9%80%A0%E8%88%87%E6%8F%90%E5%8D%87/) 提過，**所有 JavaScript 的變數，在一開始都會被設定為 undefined，而 undefined 是 JavaScript 內建的一個特殊的值，表示這個值尚未被定義**

這邊可以用判斷式來測試一下：

```
var a = "Hello World";
console.log(a);

if (a === undefined) {
  console.log('a is undefined') 
} else {
  console.log('a is defined')
}
```

結果賦予 a 值的情況下，會出現 'a is defined'。
而把值去掉的話：

```
var a;
console.log(a);

if (a === undefined) {
  console.log('a is undefined') 
} else {
  console.log('a is defined')
}
```

則是會出現 'a is undefined'。

## undefined 與 not defined 差在哪？
---

接著把 var 去掉，只留下 a，則會出現錯誤，a is not defined。
這是因為執行環境被創造時，沒有在記憶體中找到 a。
而宣告 `var a` 時，a 在創造階段就被放進記憶體中，雖然沒有賦予它值，但是確實先給它一個特殊的初始值 undefined，已經在記憶體中佔據了一席之地。

## 那麼可以設定值為 undefined 嗎？
---

理論上是可以的，來測試看看：

```
var a = undefined;
console.log(a);

if (a === undefined) {
  console.log('a is undefined')
} else {
  console.log('a is defined')
}
```

雖然 a 給它值，可能想說會是 'a is defined'，但是最後的結果會是 'a is undefined'。
所以這邊就會有問題，萬一想要除錯的話，可能會發生混亂，所以這裏需要注意不要給予賦予變數 undefined 這個特殊關鍵字。