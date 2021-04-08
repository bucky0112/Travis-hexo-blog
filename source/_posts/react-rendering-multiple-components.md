---
title: 在 JSX 使用迴圈
tags:
  - jsx
  - react
  - map
  - component
date: 2020-10-13 00:20:42
categories: React 學習系列
keywords:
  - jsx
  - react
  - map
  - component
decription: React 如何 Render 多個 Component
---
使用 Component 就是可以像積木一樣一直組裝，而且不會互相干擾資料，因為每個都是獨立的狀態。不過如果需要大量放置 Component 的話，那麼就可以試試看迴圈，本篇就來講解怎麼在 JSX 中使用迴圈。
<!--more-->

## Render 多個 Component

在一般如果數量不大時，想要顯示多個 Component，比較直覺的作法可能就像堆積木一樣，一個一個組裝就可以了。

```js
ReactDOM.render(
  <div>
    <Counter />
    <Counter />
    <Counter />
    <Counter />
    <Counter />
  </div>,
  document.querySelector("#root")
);
```

<iframe height="265" style="width: 100%;" scrolling="no" title="React counter use css class" src="https://codepen.io/bucky0112/embed/PozqrEO?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/PozqrEO'>React counter use css class</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

但是如果是大量的 Component 的話，總不可能這樣做，而在 JavaScript 中就會想到可以使用迴圈來處理。
但是，一般的 for 迴圈在 JSX 中沒辦法有回傳值，所以在實例中，會使用的是 `map()` 來操作。

## 透過 map 來操作迴圈顯示資料

1. 生成一個多個元素的陣列
```js
const counters = Array.from({ length: 6 });
// [undefined, undefined, ..., undefined]
```

2. 透過 `map()` 來執行迴圈，並帶入 key 值
```js
{counters.map((item, i) =>
  <Counter key={i} />
)}
```

最後可以看看 CodePen 完整的樣式
<iframe height="265" style="width: 100%;" scrolling="no" title="React counter use css class" src="https://codepen.io/bucky0112/embed/GRqJbzQ?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/GRqJbzQ'>React counter use css class</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>