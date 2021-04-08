---
title: 如何在 JSX 中加入 CSS 樣式？
tags:
  - react
  - jsx
  - css
date: 2020-09-27 22:12:19
categories: React 學習系列
keywords:
  - react
  - jsx
  - css
decription: 如何在 JSX 中加入 CSS 樣式？
---
在 React 中，什麼東西都可以直接寫在 JavaScript 裡面，就連 CSS 樣式也可以，雖然有點奇怪，不過我在 Vue 也是會這樣寫 inline-style 的，只是 Vue 還會寫在 template 中，而 React 則是寫在 JSX 裡面。
<!--more-->

## 把 CSS 樣式變成物件寫法？

目前的理解是，感覺什麼都可以寫成變數來操作，就連 CSS 樣式也可以。
我寫了一個點擊器範例：
<iframe height="265" style="width: 100%;" scrolling="no" title="React count demo" src="https://codepen.io/bucky0112/embed/QWNoYXg?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/QWNoYXg'>React count demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

接著試著把 CSS 樣式寫成物件，寫法是這樣：
```js
// 定義一個 style變數
const numStyle = {
  margin: '0 50px 0 50px', // 如果不是純數字，就要用字串格式
  color: '#EE6464', 
  fontSize: 60, // 加 px 就要使用字串，這裡 px 可用可不用
}
```

要注意的地方：
1. 由於這裡是物件，不是 CSS，所以每一個屬性結束都是 「 **,** 」 符號，而不是 「 **;** 」 符號。
2. 每一個屬性都要是小駝峰，例如 `font-size` 就要寫成 `fontSize`。

接著把它寫進 JSX 中，用一個大括號把變數名稱包起來就可以了：
```js
<div className="num" style={ numStyle }>0</div>
```

## CSS 樣式的另一種寫法

如果一個 CSS 樣式蠻多的話，寫成一個物件是蠻方便的，還可以一直加設定進去，但是如果沒有很多的話，或是想要邊寫邊加的話，也是可以直接寫成 inline-style，例如：
```css
.plus {
	font-size: 60px;
	color: #ffff;
}
```

寫成這樣：
```js
<div className="plus" style={{ fontSize: 60, color: '#ffff' }}>+</div>
```

寫法變成 Vue 的 template 了！？
其實只是在 JSX 的 CSS 樣式裡加上一個物件而已，還蠻直覺的，很有趣。