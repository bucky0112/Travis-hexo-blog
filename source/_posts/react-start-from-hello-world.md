---
title: React 從 JSX 開始 Hello World
tags:
  - react
  - jsx
  - hello world
date: 2020-09-23 17:37:50
categories: React 學習系列
keywords:
  - react
  - jsx
  - hello world
decription: 從 JSX 開始 Hello World
---
相信每個使用 React 的前端工程師都會聽過並學習使用 **JSX** 這個語法，雖然官方是說不一定要求使用 JSX，不過先來看一下它如何寫，也許就不會這麼排斥它了...吧？
<!--more-->
什麼是 JSX？
JAX 是一種 JavaScript 的語法擴充，官方推薦使用它來寫出使用者界面的外觀，而且 JSX 允許你使用 JavaScript 所有的功能。

## 從 Hello World 開始學 JSX

<iframe height="265" style="width: 100%;" scrolling="no" title="First React - Hello World" src="https://codepen.io/bucky0112/embed/BaKvPQx?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/BaKvPQx'>First React - Hello World</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

從上面的 CodePen 中，可以看到畫面顯示 Hello World，不過是怎麼動的呢？看一下程式碼：
```js
ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("root"));
```

各位觀眾，這就是 JSX 了，看起來 JavaScript 裡面的結構跟一般的 HTML 差不多，其實把它當成是威力加強版的 HTML 就可以了（跟 Koei 的某遊戲差不多）。
那麼他是怎麼運作的呢？
![A51F551D-26EB-43F7-AA39-70BF795A00A0](https://i.imgur.com/8SFMmD2.png)

首先看到它使用了 `ReactDOM.render` 這個語法，裡面有兩個參數：
1. JSX 的內容，要顯示在網頁上。
2. 綁定一個區塊，並顯示在上面。
所以這邊就是一個綁定在 `id="root"` 的 div 中，並顯示內容為 `<h1>Hello, world!</h1>`，而在 HTML 中，只需要寫 `<div id="root"></div>` 就可以成功顯示了。

![FA5C08EF-480A-41C3-A030-2A41B1C58C44](https://i.imgur.com/Wtcv3h2.png)

當然如果你直接在空白的 CodePen 上寫一樣的 Code 是沒有辦法運作的，一定要透過 React 跟 ReactDOM 這兩個套件還有 Babel 預處理器，所以要記得加上去，不然會跳錯喔～