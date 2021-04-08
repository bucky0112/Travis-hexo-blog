---
title: JSX 變數與表達式使用
tags:
  - react
  - jsx
  - 變數
  - 表達式
date: 2020-09-26 22:33:07
categories: React 學習系列
keywords:
  - react
  - jsx
  - 變數
  - 表達式
decription: JSX 變數與表達式使用
---
經過上一回的 JSX 超入門，這一次一樣入門，不過從變數以及表達式開始。
<!--more-->
變數怎麼使用？
先定義一個變數出來，例如 `const word = 'PS5'` ，接著在 `ReactDOM` 的第一個參數中使用，只要在變數名稱的外面包上一個大括號就可以使用了，例如：
<iframe height="265" style="width: 100%;" scrolling="no" title="React variable" src="https://codepen.io/bucky0112/embed/VwaRxwb?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/VwaRxwb'>React variable</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 表達式使用
而在大括號中，除了可以放入變數以外，也可以放入表達式（關於表達式與陳述式這個話題，以後會專門開一個文章來說明）。

```js
const num = 100;
ReactDOM.render(
  <p>PS5 今天賣 {num * 100} 台了</p>,
  document.querySelector("#root")
); // 在畫面上就會呈現 PS5 今天賣 10000 台了
```

JSX 的變數大致上就是這樣使用了，使用起來跟 Vue 的感覺有點像，雖然目前還不太習慣，總之就繼續練習下去吧！