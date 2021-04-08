---
title: JS notice： 語法解析器、詞彙環境、執行環境
tags:
  - javascript
  - 語法解析器
  - 詞彙環境
  - 執行環境
categories: JavaScript 的怪奇物語
keywords:
  - javascript
  - JavaScript 全攻略：克服 JS 的奇怪部分
decription: Is a note about JavaScript Understanding the Weird Parts by Anthony Alicea
date: 2020-04-01 00:16:57
---

由於踩了不少 JavaScript 的雷，想說需要好好的重新認識一下，於是希望藉由 [JavaScript 全攻略：克服 JS 的奇怪部分](https://www.udemy.com/course/javascriptjs/) 這個系列，能夠更增進自己的實力，順便能夠培養寫文章的習慣，那麼就開始吧！Go！Go！
<!--more-->

## 語法解析器 (Syntax Parsers)
---

首先要先了解當電腦在執行你寫的 code，其實是看不懂你寫的文字是什麼意思，更精確地說是需要透過中間一個媒介去翻譯成電腦看得懂的東西。而這個媒介是是由人寫出來的程式就是語法解析器，又稱為編譯器（Compilers）。

![給電腦執行的其實是被轉換過，讓電腦看得懂要做什麼。](https://i.imgur.com/500vRoE.png)

**那麼編譯器是怎麼運作的呢？**

當你輸入一段文字，例如： console.log('Hello')，編譯器會一個字一個字地讀，當讀完整個詞時，得到關鍵字 `console.log('Hello')`，就會轉換給電腦看得懂的指令。

## 詞彙環境 (Lexical Environments)
---

>程式碼在程式中實際所在的位置

詞彙環境在於一些程式語言中，認為程式碼寫在哪裡是很重要的。（不是每個程式語言都這樣）因為它幫助語法解析器看你寫的程式碼，它的語法、它的單字做決定，例如：

```
function greeting () {
    var a = 'hello world';
}
```

所以語法解析器讀到 `var a = 'hello world'` ，它就會了解說，這一段的位置是在 `greeting()` 這個 function 裡面。

## 執行環境 (Execution Contexts)
---

一般在執行程式的時候，會有許多的詞彙環境，而執行環境會去管理哪一個要執行。

## 資料來源
---

[圖片來源](https://www.guru99.com/syntax-analysis-parsing-types.html)
[JavaScript 全攻略：克服 JS 的奇怪部分](https://www.udemy.com/course/javascriptjs/)