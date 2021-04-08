---
title: JS 基礎回顧 - function
tags:
  - function
  - 匿名函式
date: 2020-04-11 11:29:52
categories: JS 基礎回顧
keywords:
- function
- 函式宣告
- 匿名函式
- 回傳值
decription: 有關 function 中的宣告及運作、回傳值，及匿名函式。
---
在看 kuro 大神寫的 [0 陷阱！0 誤解！8 天重新認識 JavaScript！](https://www.tenlong.com.tw/products/9789864344130)（不是業配，這本書很棒）中提到，JavaScript 最核心也最容易被誤用的部份非函式（function）莫屬。我一開始在學 JS 時，碰到函式就覺得非常挫折，因此目前在整理這個筆記時，希望以後可以不用再踩坑？
<!--more-->

## 函式宣告
---

```
function sayhello() {
  console.log ('hi')
}

sayhello()  // 印出'hi'
```

如上面的例子可以看到，函式的宣告及呼叫的運作。

![Image](https://i.imgur.com/HRVJOZV.png)

一個宣告函式主要會包含三個部份：

* 函式的名稱（或是可能沒有名稱）。
* 小括號 () 中的部份是參數，可以帶入無限個。
* 大括號 {} 中的部份，是主要區塊，放重複執行的內容。

再看一個帶入參數的例子，如果想要得到正方形的面積的話，公式是邊長 x 邊長，所以在 function 中可以這樣寫：

```
function getSquareArea (side) {
  var answer = side * side
  console.log(answer)
}

getSquareArea(50)  // 引數帶入數值 50，答案得到 2500
```

當呼叫一個需要帶入資料的函式，像上面的例子，在函式名稱後的小括號 () 傳入使用的值，可以是變數或是數值，稱為**引數**。

### 回傳值
---

在建立 function，如果希望透過呼叫後可以得到回應結果，可以透過 return 來取得回傳值，例如：

```
function isPass (score) {
  return (score >= 60)     // 回傳是不是 60 分
}

var myTest = 59;          

if (isPass(myTest)) {      
  console.log('及格')      // 如果就印出及格
} else {
  console.log('不及格')    // 如果不是就印出不及格，最後結果是不及格
}
```

再看一個例子，試著計算三角形的面積，公式是底 x 高 / 2，這次引數以變數方式帶入，所以可以這樣寫：

```
function getTriangleArea (base, height) {
  return base * height * 0.5
} 

// 用變數帶入底10，高20
var b = 10;
var h = 20
console.log(getTriangleArea (b, h)) // 得到答案 100
```

還可以重複使用：

```
var area1 = getTriangleArea (50, 60);
var area2 = getTriangleArea (90, 200);

console.log(area1); // 得到答案 1500
console.log(area2)  // 得到答案 9000
```

## 匿名函式
---

在上一個函式中的寫法是：

```
function isPass (score) {
  return (score >= 60)   
}
```

而在另一種的寫法，透過`變數名稱 = function ([參數] {...})`,將一個函式透過 = 指定給某個變數：

```
// function 後面沒有名稱
var isPass = function (score) {
  return (score >= 60)
}
```

因為在變數型別中，除了基本型別以外的都是**物件型別**，所以可以被呼叫，自然也可以透過變數存入。

也由於在這個例子中，function 後面沒有名稱，所以是**匿名函式**。

如果要替它加上名稱也是可以，例如：

```
var isPass = function func(score) {
  return (score >= 60)
}
```

但是要注意的是，這個 func 只在**自己函式範圍**有效，看以下的例子：

```
var isPass = function func (score) {
  console.log(typeof func);  // 使用 typeof 可以判斷型別
  return (score >= 60)
}

console.log(typeof func);
```

上面的結果，第一個 typeof 判斷是 function，第二個則是出現 undefined，func 一旦不在自己函式範圍內，就不存在了。

不過如果想要取得這個 function 的名稱，也可以透過自定義的變數名稱，例如：

```
var isPass = function func (score) {
  console.log(typeof isPass);
  return (score >= 60)
}

console.log(typeof isPass)
```

2 個 console.log 結果都會顯示 function。
