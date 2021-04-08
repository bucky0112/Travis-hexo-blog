---
title: JS 基礎回顧 - 變數與資料型別
tags:
  - javascript
date: 2020-04-10 17:39:39
categories: JS 基礎回顧
keywords: 
  - variable
  - data-type
decription: about variable and data-type of javascript
---
會寫這個系列，是想整理我之前的筆記，假如我哪天忘記的話，還可以讓我方便查找，順便加深自己的印象。
第一篇來回顧變數，看如何定義跟指定各種型別的用法。
<!--more-->

## 變數
---

![Image](https://i.imgur.com/cPGhwBt.png)

**變數**像是一個容器，假設一個容器貼着 coca_cola 的標籤，裏面裝的是叫可樂的東西，當接觸到瓶子，就可以拿裏面的東西，裏面的東西就是可樂。
用 JavaScript 的語言來說就是：

```
var coca_cola = "可樂"
```

**var = variable 的縮寫**
**coca_cola = 容器的標籤（變數名稱）**
**可樂 = 容器裡裝的內容（值）**

## 資料型別
---

### 基本型別

變數值的型別主要分成**基本型別**跟**物件型別**，這個部份先來討論基本型別：

* number 數字
* string 字串
* boolean 布林值
* undefined 
* null

例如：數字 7、字串 “hello”、布林值 true / false

#### 數字、字串

```
var name = "張無忌"; // 字串，可用單引號或雙引號把文字包起來
var power = 100000; // 數字，不需引號
```

如果要檢驗型別的話，可以使用 `typeof`

```
var num = “123”;
console.log(typeof num) // 會得到 string 
```

#### 布林值

```
var n = true;  // 布林值、布林值，不需引號
console.log(n) // a = true，所以結果是 true
```

#### undefined 跟 null

而 undefined 跟 null 比較特別，看以下的例子：

如果定義了一個叫做 hello 的變數，但沒有指定值，
這時候 hello 的值是 undefined 狀態

```
var hello   // undefined
```

如果定義 world 這個變數，指定為 null，
這時 world 的值就是 null 狀態

```
var world = null  // 
```

**undefined = 未指定變數的內容，未定義但存在**
**null = 不存在**

### 如果把不同型別放在一起？

同型別的值，例如數字放在一起，就會按照指示做加減乘除，但是如果不同型別放在一起會是如何？看以下的例子：

```
console.log(1 + 1); // 印出 2
console.log(1 + "1"); // 印出 11
console.log("hello" + 123); // 印出 hello123
console.log("hello" + true); // 印出 hellotrue
console.log(123 + true); // 印出 124
console.log(123 + false); // 印出 123
console.log(123 + null); // 印出 123
console.log("123" + null); // 印出 123null
console.log(123 + undefined); // 印出 NaN(Not a Number)
console.log("123" + undefined); // 印出 123undefined
```

### 等號

#### a + 1 = 1 是什麼情況?

```
var a = 1;
a = a + 1;
console.log(a);  // 2
```

= 不是等號，是指定
a = a + 1;是 a + 1 完再指定回 a

接著再看以下的情況：

#### 等號，不一定等於『等於』

```
var age = 8;
if (age = 10) {
  console.log('yes');
} else {
  console.log('no');
}                     // 結果會是 yes
```

等等，age 不是設定是 8 嗎？不等於 10 怎麼會是 yes 呢？
前面說過 = 不是等號，是指定的意思，如果是以上的情況，應該用 2 個等號或 3 個等號來做比較。

2 個等號 => 比較內容物是不是一樣。
3 個等號 => 除了比較內容物之外，還有比較是不是資料形別，資料形別是不是一樣。
在 if (age = 10)中，age = 10 就是指定 age 爲 10

```
console.log(1 == 1);     //  true
console.log(1 == '1');   //  true
console.log(1 === '1');  //  false
```

所以在判斷的時候，儘量可以的話就用三個等號會比較安全。