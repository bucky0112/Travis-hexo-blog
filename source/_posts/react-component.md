---
title: 建立並使用 React Component
tags:
  - react
  - component
date: 2020-09-28 23:29:08
categories: React 學習系列
keywords:
  - react
  - component
decription: 建立並使用 React Component
---
在前端框架中，將原本 UI 拆分成獨立而且可以重複使用的程式碼，就是 Component（Angular 我不確定是不是，不過我在 Vue 蠻常用的）。
拆成 Component 的好處是，方便日後維護以及可以專注在個別程式碼的開發，還有最棒的是可以當作是一個樂高積木一樣重複使用組合。
<!--more-->

## 使用React Component

```js
// 把下面整個 .container 給包成 component
<div class="container">
	<div class="plus">+</div>
  <div class="num">0</div>
  <div class="minus">-</div>
</div>
======================================================
// 用一個 function 包起來
const Count = () => {
	return (
		<div className="container">
			<div className="plus">+</div>
  			<div className="num">0</div>
  			<div className="minus">-</div>
		</div>
	);
}
======================================================
// 因為是直接回傳一個值，可以直接省略 return
const Count = () => (
	<div className="container">
		<div className="plus">+</div>
  		<div className="num">0</div>
  		<div className="minus">-</div>
	</div>
);

// 把 Component 當成一個 HTML 標籤給包起來就可以運行了
ReactDOM.render(<Counter />, document.getElementById("root"));
```

### **需要注意的地方**
命名 Component 時，必須要使用大寫，如果用小寫的話，就會當成一般的 HTML 元素（我在這地方卡好久，一直找不到哪裡錯）。

## 如何重複使用 Component
 Component 厲害的地方就是可以一直重複使用，例如：

<iframe height="265" style="width: 100%;" scrolling="no" title="React Component" src="https://codepen.io/bucky0112/embed/rNebyqe?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/rNebyqe'>React Component</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## Component 的事件處理
前面雖然成功顯示多個 Component 了，但是該有的互動效果還沒處理，這邊就來示範怎麼處理。

* 使用變數
在 Component 中定義一個變數，並使用它：
```js
const Counter = () => {
  const countNum = 0; // 定義變數
  return (
    <div className="container">
      <div className="plus">+</div>
      {/* 使用變數 */}
      <div className="num">{ countNum }</div>
      <div className="minus">-</div>
    </div>
  );
}
```

* 綁定事件監聽
接著需要綁定一個事件監聽來執行動作，跟 原生 JavaScript 的 onclick 一樣，只是在 React 中使用的是 `onClick`，也跟 Vue 的 `@click` 差不多，然後需要再給它一個函式去執行：

```js
const Counter = () => {
  let countNum = 0;
  return (
    <div className="container">
      <div className="plus"
        {/* 當按 + 時，讓 countNum 加 1 */}
        onClick={() => {
          countNum = countNum + 1;
          console.log(countNum);
        }}
      >+</div>
      <div className="num">{ countNum }</div>
      <div className="minus">-</div>
    </div>
  );
}
```

雖然 console 顯示數字有更新，但是畫面卻沒有更新顯示。
接著加上 `{console.log("test render")}` 來測試是否每次更新資料，畫面會重新 render：
<iframe height="265" style="width: 100%;" scrolling="no" title="React Component" src="https://codepen.io/bucky0112/embed/JjXVMEK?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/JjXVMEK'>React Component</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

結果 "test render" 只有出現一次，還是沒有顯示更新的數字，原因是因為 React 並不知道資料有更新。
在下一次會介紹 React 的 Hook 可以幫助我們處理這個問題。