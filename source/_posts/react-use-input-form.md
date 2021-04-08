---
title: React 的表單輸入運用
tags:
  - react
  - form
  - usestate
  - hook
date: 2020-10-21 22:04:03
categories: React 學習系列
keywords:
  - react
  - form
  - usestate
  - hook
decription: 利用 useState 來運用表單輸入
---
在 Vue 中使用 v-model 來做表單輸入時非常好用，那麼在 React 中可以怎麼運用呢？本篇文章將會介紹該怎麼利用 Hook 來做類似的效果。

<!--more-->

## 利用 **useState** 綁定數值

首先，先看一下完成的程式：

<iframe height="265" style="width: 100%;" scrolling="no" title="React 的表單輸入運用" src="https://codepen.io/bucky0112/embed/BazQBQr?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/BazQBQr'>React 的表單輸入運用</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

當輸入數字時，另一個顯示的欄位就會即時顯示輸入的數字，那麼就一步一步地來看是如何做出來的。

1. 載入 useState 方法

   ```js
   const { useState } = React;
   ```

2. 使用 useState 方法

   ```js
   // [state, '改變 state 的方法'] = useState(預設值)
   const [inputNum, setInputNum] = useState(0);
   ```

3. 定義事件處理器

   ```js
   // 在 React 中，一般會使用 handle 開頭命名事件處理器
   const handleInputNum = (e) => {
     const { value } = e.target; // 透過 e.target.value 取出輸入內容
     setInputNum(value); // 再經由 setInputNum 即時更新 inputNum 的 state
   };
   ```

4. 在輸入欄位綁定 onChange 事件，並把 state 帶入 value

   ```react
   <input
     type="number"
     onChange={handleInputNum}
     value={inputNum}
     min="0"
   />
   ```

5. 讓顯示區域同步更新數值

   ```react
   <input type="number" value={inputNum} />
   ```



以上就是利用 useState 這個 hook 來運用表單的例子，當然也可以做其他的運用，例如在 `value={inputNum * 2}` 做乘 2 倍的運算。

