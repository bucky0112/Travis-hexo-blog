---
title: ES6 好用語法 - 箭頭函式
tags:
  - ES6
  - 箭頭函式
  - arrow function
date: 2020-09-19 17:40:54
categories: ES6 好用語法
keywords: 
  - ES6
  - 箭頭函式
  - arrow function
decription: ES6 中好用語法，箭頭函式的使用。
---
在 ES6 中，有許多好用的語法可以使用，這篇就來介紹箭頭函式。
<!--more-->

## 前言

最近買了一本書，書名是「**從 Hooks 開始讓你的網頁 React 起來**」。其實我本來就一直對 React 很有興趣，雖然後來跑去學了 Vue.js，不過我內心對 React 還是有一份憧憬，而且也想要了解這兩者有什麼不同之處，所以就開始來試著學看看吧。

React 是目前 JavaScript 三大框架中之一，雖然它比較像是一個用來渲染 UI 組件的函式庫而不是一個框架 (Framwork)，不過在其生態圈下有許多好用的函式庫，所以要說它是框架也是可以的。

React 雖然很強大，但是要入門的門檻還是有的，至少你的 ES6 語法要夠熟悉，所以想要寫一些文章來熟悉一下我的 ES6，那麼就開始吧！

## 箭頭函式 (Arrow Function)

### 一般函式的寫法
函式一般在 JavaScript 中的寫法可能會是這樣:

```js
function callName(name) {
  return `my name is ${name}`;
}

callName('Bucky');
```
### 箭頭函式的寫法

* 而箭頭函式的用法寫法如下:

```js
const callName = (name) => {
  return `my name is ${name}`;
}

callName('Bucky');
```

* 如果只有一個參數的話，則可省略小括號:

```js
const callName = name => {
  return `my name is ${name}`;
}

callName('Bucky');
```

* 如果只是要直接回傳一個值的話，就可以更精簡了，可以把大括號跟 return 去掉:

```js
const calName = name => `my name is ${name}`;

callName('Bucky');
```

#### **箭頭函式特別注意的地方**

* 回傳物件

如果是要回傳一個物件的話，就要注意一下用法，在物件的大括號外面包一個小括號就可以了:

```js
const myInfo = () => ({
  name: 'Bucky',
  age: 18,
  skill: 'JavaScript',
});

myInfo();
```

* 箭頭函式**沒有 arguments**

看一下一般函式在 arguments 的使用：
```js
function testScore() {
  console.log(arguments);
}

testScore(60, 59, 68, 44, 92, 85); // 得到 // [60, 59, 68, 44, 92, 85]
```
如果用箭頭函式的話，則是會報錯：
```js
const testScore = () => {
  console.log(arguments);
}

testScore(60, 59, 68, 44, 92, 85); // arguments is not defined

// 如果要使用的話，可以使用其餘參數：
const testScore = (...arg) => {
  console.log(arg);
}

testScore(60, 59, 68, 44, 92, 85); // [60, 59, 68, 44, 92, 85]
```

* 沒有自己的 this

一般函式的操作：
```js
var name = 'Cris';
var data = {
  name: 'Bucky',
  callName() {
    console.log(`1st this is ${this.name}`);
    setTimeout(function(){
      console.log(`2nd this is ${this.name}`);
    }, 50);
  }
}
data.callName();
// 得到 "1st this is Bucky" "2nd this is Cris"
```

使用箭頭函式：
```js
var name = 'Cris';
var data = {
  name: 'Bucky',
  callName: () => {
    console.log(`1st this is ${this.name}`);
    setTimeout(() => {
      console.log(`2nd this is ${this.name}`);
    }, 50);
  }
}
data.callName(); // 兩個都會指向 Cris

// 解決方法是，在 method 不要使用 arrow function
var data = {
  name: 'Bucky',
  callName() {
    const self = this;
    console.log(`1st this is ${this.name}`);
    setTimeout(() => {
      console.log(`2nd this is ${self.name}`);
    }, 50);
  }
}
data.callName(); // 兩個都會指向 Bucky
```

* 可以使用箭頭函式的時機

前面提到的雷都是關於 this，那麼有沒有適合箭頭函式的時機呢？
是有的，如果是在 立即函式 (IIFE) 的話，就可以正常運作，也不用另外宣告變數來代替 this，例如：
```js
const data = {
  name: 'Bucky',
  callName() {
    console.log(`1st this is ${this.name}`);
    (() => { console.log(`2nd this is ${this.name}`); })()
  }
}
data.callName(); // 兩個都會指向 Bucky
```
### 練習

學了一點皮毛後，那麼來練習一下吧～
把以下的所有函式都改成箭頭函式看看：

```js
// 題目：1
function sum(a, b) {
  var c = a + b;
  return c;
}
console.log(sum(5, 6));

// 題目：2
function square(num) {
  return num * num;
}1
var d = square(5);
console.log(d);

// 題目：3
setTimeout(function() {
  console.log('延遲 10 毫秒');
}, 10);

// 題目：4
var arr = [1, 2, 3];
var arr2 = arr.map(function(item, i) {
  return item * 2;
});
console.log(arr2);

// 題目：5
var obj = {
  fn: function fn2(a) {
    return a * a;
  }
}
console.log(obj.fn(10));
```

以下是我的答案：

```js
// 1. Ans:
const sum = (a, b) => a + b;

console.log(sum(5, 6));

// 2. Ans:
const square = num => num * num;
const d = square(5);
console.log(d);

// 3. Ans:
setTimeout(() => console.log('延遲 10 毫秒'), 10);

// 4. Ans:
const arr = [1, 2, 3];
const arr2 = arr.map((item, i) => item * 2);
console.log(arr2);

// 5. Ans:
const obj = {
  fn: (a) => a * a,
}

console.log(obj.fn(10));
```