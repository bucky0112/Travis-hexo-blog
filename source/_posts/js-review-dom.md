---
title: JS 基礎回顧 - DOM
tags:
  - DOM
  - javascript
  - w3HexSchool
date: 2020-05-20 17:09:33
categories: JS 基礎回顧
keywords:
- DOM
decription: 如何使用 DOM，最後做出一個 BMI 計算機。
---
想像一下在夾娃娃機中，我們操作著爪子，在機台中抓取我們想要的東西。
而這樣的概念，有點像是我們要怎麼樣來藉由 JavaScript 操作網頁畫面的 HTML 元素，文件物件模型（Document Object Model, DOM）就可以幫助我們來做這件事。
<!--more-->

那麼我們有哪些爪子可以使用呢？
一般常使用的有：

* `getElementById()`
* `getElmentsByClassName()`
* `querySelector()`
* `querySelectorAll()`

下方有兩塊 div，如果想改變它們的文字內容的話，就可以透過 DOM 來操作。

```
<div class = "doll" id = "doll-1">娃娃1</div>
<div class = "doll" id = "doll-2">娃娃2</div>
```

來看看個別使用的方法：

## getElementById()
---

```
document.getElementById('doll-1').textContent = '彌豆子';

var tanjiro = document.getElementById('doll-2');
tanjiro.innerHTML = '炭治郎'
```

可以透過 `document.getElementById('ID名稱')` 再接 `textContent` 或是 `innerHTML` 去變更文字。
或是宣告一個變數，指定為 `document.getElementById('ID名稱')`，再使用變數接 `textContent` 或是 `innerHTML` 去變更文字。

## getElmentsByClassName()
---

```
var allDolls = document.getElementsByClassName('doll');
var length = allDolls.length;
for (var i = 0; i < length; i++) {
  allDolls[i].textContent = '炭治郎'
}
```

`getElmentsByClassName()` 的用法就不太一樣，是用在一次改變全部相同的元素。

上面的是比較早期的作法，接下來的兩種方法是比較現代大部分在使用的。

## querySelector()
---

跟 `getElementById()` 方法類似，只是選取的方式不太一樣，`querySelector()` 在選擇 ID 的時候比較像 CSS 的類別選取器的方式，例如：

```
var nezuko = document.querySelector('#doll-1');
nezuko.textContent = '彌豆子'
```

像這樣就可以將原本的內容改為想要的文字。

## querySelectorAll()
---

跟 `getElmentsByClassName()` 差不多的用法，這次把 `<span>` 中改成想要的文字：

```
<div class = "doll" id = "doll-1">
  <span></span>
</div>
<div class = "doll" id = "doll-2">
  <span></span>
</div>
```

```
var tanjiro = document.querySelectorAll('.doll span');

for (var i=0; i<tanjiro.length; i++) {
  tanjiro[i].textContent = '炭治郎'
}
```

就可以把全部同名的 class 改成想要的文字。

如果想透過 `getElmentsByClassName()` 把兩個不同的 class 改成個別不同的內容的話，也是辦得到的，但先來看一下是怎麼做到的。

用 `console.log(tanjiro)` 看一下用 `getElmentsByClassName()` 控制的 DOM 會是什麼內容？

![Image](https://i.imgur.com/lK7RzsV.png)

結果會出現陣列，所以這樣就可以用控制陣列的方式來改變該筆內容：

```
var tanjiro = document.querySelectorAll('.doll span');

tanjiro[0].textContent = '竈門';
tanjiro[1].textContent = '炭治郎'
```

## 練習用 DOM 來做一個 BMI 計算機
---

目前已經知道如何使用 DOM 來操作網頁的元素了，那麼就試著做出一個 BMI 計算機。
下方是做出來的成品：

<iframe height="265" style="width: 100%;" scrolling="no" title="BMI 計算機 (DOM 練習)" src="https://codepen.io/bucky0112/embed/xxwBxKV?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/xxwBxKV'>BMI 計算機 (DOM 練習)</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

HTML 部份：

```
<h1>BMI 計算機</h1>
<div class="inputData">
  <label for="bodyHeight">身高</label>
  <input type="number" id="bodyHeight" min="0">公分
  <label for="bodyWeight">體重</label>
  <input type="number" id="bodyWeight" min="0">公斤
</div>

<input type="submit" value="計算">

<h2>BMI值：</h2>
<p id="calResult"></p>
<span id="bmiResult"></span>
```

### 綁定輸入欄位

首先思考如何做出當按下計算按鈕時，能夠將輸入的資料給顯示出來。

先在按鈕部份加上一個事件（事件部份之後會再提到），這裏用到的是 `onclick`，並綁定一個 function，可以做呼叫並做裡面的動作：

```
<input type="submit" value="計算" onclick="getBody()">
```

接著要讓輸入的資料能夠顯示，先試著讓輸入的資料用 `alert` 顯示看看：

```
function getBody() {
  var height = document.querySelector("#bodyHeight").value;
  var weight = document.querySelector("#bodyWeight").value;
  alert(height + weight)
}
```

有顯示資料，但是變成字串相加，所以要讓資料變成數字。

### 個別函式處理資料

再用一個 function 去處理資料，並帶入 BMI 計算公式：

```
function bmiCal(height, weight) {
  var h = parseInt(height) / 100;  // parseInt() 資料轉成數字
  var w = parseInt(weight);
  return (w / (h * h)).toFixed(1); // toFixed(1) 算到小數點第 1 位
}
```

接著再 `alert` 一次：

```
function getBody() {
  var height = document.querySelector("#bodyHeight").value;
  var weight = document.querySelector("#bodyWeight").value;
  alert(bmiCal(height, weight))
}
```

這次就可以顯示算出的數字了。

### 讓 BMI 值顯示在網頁上

既然可以算出資料，接著就讓資料顯示出來。
在 `getBody()` 中綁定 DOM，並讓資料透過 `textContent` 顯示：

```
var calResult = document.querySelector("#calResult");
calResult.textContent = bmiCal(height, weight);
```

### 加入條件顯示是否正常

都有計算結果就可以在上面做出其他的效果，這邊讓計算結果符合以下條件：
如果超過 24 就顯示過胖，低於 18.5 顯示過瘦，其他就正常，就可以這樣寫：

```
var bmiResult = document.querySelector("#bmiResult");
if (bmiCal(height, weight) > 24) {
    bmiResult.textContent = "過胖";
  } else if (bmiCal(height, weight) < 18.5) {
    bmiResult.textContent = "過瘦";
  } else {
    bmiResult.textContent = "正常";
  }
```

### 判斷是否輸入資料

如果沒有輸入資料的話，在這樣計算結果會出錯，所以要加入判斷式，判斷是否有輸入資料：

```
if (height !== "" && weight !== "") {
    calResult.textContent = bmiCal(height, weight);
  } else {
    alert("請輸入資料");
}
```