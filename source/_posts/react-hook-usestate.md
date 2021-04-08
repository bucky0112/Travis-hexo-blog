---
title: 使用 useState 來管理狀態
tags:
  - react
  - hook
  - useState
date: 2020-09-30 00:23:26
categories: React 學習系列
keywords:
  - react
  - hook
  - useState
decription: 介紹 React 的 Hooks 之一 - useState
---
前面提到當變數更新時，React 還不知道資料有更新。所以 React 提供了一個方法來監控而且管理資料，而這就是 React 的 Hooks 之一 - **useState**。
<!--more-->

![image-20201026231623174](https://i.imgur.com/OqnGEog.png)

## 什麼是 State

首先要先來了解 State 是什麼，才能 use 它。
根據 [React 官網](https://zh-hant.reactjs.org/docs/state-and-lifecycle.html)說到，State 類似 props，不過差別在於 State 是由 Component 私有控制的。可以把它當成是資料，
假設一個資料有兩種狀態，true 或 false，預設狀態是 false，當你使用它時，就會變成 true，這種狀態就是 state。

## 來試著使用 State Hook 吧

先來簡單瞭解一下什麼是 Hook：
> *Hook* 是 React 16.8 中增加的新功能。它讓你不必寫 class 就能使用 state 以及其他 React 的功能。 (來自 React 官網的解釋)

簡單來說 Hook 就是 React 中各項特殊 function，而本篇則是會介紹 **useState** 的用法。

## 開始使用 useState

下方有一個 Component，來試著使用 useState 改動畫面。
<iframe height="265" style="width: 100%;" scrolling="no" title="React Component" src="https://codepen.io/bucky0112/embed/JjXVMEK?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/JjXVMEK'>React Component</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

1. 從 React 中引入 useState。
```js
const { useState } = React;
```

2. 呼叫 useState，宣告一個 state，並回傳了 2 個值。第一個是變數，並帶入參數給 useState；第二個是更新變數的函式。
```js
// [變數, 更新變數的函式] = useState(變數預設值)
const [count, setCount] = useState(0);
```

3. 讀取 State。
```
<div className="num">{count}</div>
```

4. 透過點擊，就會呼叫 setCount 並傳入新的值，React 會 重新 render component，顯示新的 State 值。
```
<div className="plus"
	onClick={() => setCount(count + 1)}
>+</div>
```

完整的點擊器範例就完成了
<iframe height="265" style="width: 100%;" scrolling="no" title="React useState" src="https://codepen.io/bucky0112/embed/GRZaZEz?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/GRZaZEz'>React useState</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## useState 做了什麼？

我們透過 useState 建立一個**需要被即時監控的資料變數**，在這個範例中也就是 count，並呼叫了它提供的更新變數的函式來改變數值。
如果變數的值有變化，也確實有呼叫 setCount 的話，那麼 React 就會以資料驅動畫面更新，這就是 useState 幫我們做的事情。