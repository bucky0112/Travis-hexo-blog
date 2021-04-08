---
title: React 運用邏輯運算子解決 JSX 問題
tags:
  - react
  - logical operators
  - jsx
date: 2020-10-01 22:22:49
categories: React 學習系列
keywords:
  - react
  - logical operators
  - jsx
decription: React 運用邏輯運算子解決 JSX 問題
---
在 React 前一個筆記中，有試著做出一個 Counter，並且有加入判斷條件：如果數字為 0 的話，就沒辦法繼續按減號。還有另一種作法是：如果數字為 0 的話，減號按鈕就消失不能按，不過實做的話，會出現一些問題，這篇來看看需要解決什麼問題。
<!--more-->

## JSX 不能使用陳述式

設定數字大於 0，減號顯示，按照邏輯可能會這樣寫：

```js
const Count = () => {
  return (
    if (count > 0) {
      <div
        className="plus"
        onClick={() => setCount(count - 1)}
      >+</div>
    }
  );
}
```

但是這樣做會違反 JSX 的規則，也就是 JSX 只能使用表達式，不能使用陳述式。
如果要解決這個問題的話，可以使用邏輯運算子。

## 什麼是邏輯運算子？

根據 [MDN 官網](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators#%E9%82%8F%E8%BC%AF%E9%81%8B%E7%AE%97%E5%AD%90)解釋，邏輯運算子通常被用於布林值的判斷，使用的符號有 `&&`、`||`、`!`，要注意的是在判斷會自動轉型，例如  null、0、undefined、NaN、空字串，都會被轉換成 false，看以下例子：

```js
// 假設 && 前面的值為 true 的話，就回傳後面的值 
const a = 'cat' && 'dog'; // 'dog'
const b = true && false; // false
const c = null && 1; // null
const d = 0 && 2; // 0
const e = undefined && 'yes'; // undefined
const f = NaN && 'cat'; // NaN;
const g = '' && 'dog'; // '';
```

```js
// 假設 || 前面的值為 true 的話，就回傳前面的值 
const a = 'cat' || 'dog'; // 'cat'
const b = true || false; // true
const c = null || 1; // 1
const d = 0 || 2; // 2
const e = undefined || 'yes'; // 'yes'
const f = NaN || 'cat'; // 'cat';
const g = '' || 'dog'; // 'dog';
```

```js
// 判斷 ! 後面的值是 true 的話，回傳 false，反之回傳 true
const a = !'cat'; // false
const b = !true; // false
const c = !null; // true
const d = !0; // true
const e = !undefined; // true
const f = !NaN; // 'true
const g = !''; // true
```

## 運用邏輯運算子來編譯畫面

前面提到設定數字大於 0，減號顯示，這邊再加上如果數字小於 10 的話，加號才能顯示，所以可以這樣寫：

```js
// 當 count >= 10，就顯示加號
{count >= 10 || (
  <div className="plus" onClick={() => setCount(count + 1)}>
    +
  </div>
)}

// 當 count > 0，就顯示減號
{count > 0 && (
  <div className="minus" onClick={() => setCount(count - 1)}>
    -
  </div>
)}
```

下面是完整的 CodePen，可以玩玩看：

<iframe height="265" style="width: 100%;" scrolling="no" title="React count demo" src="https://codepen.io/bucky0112/embed/NWNZddN?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/NWNZddN'>React count demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>