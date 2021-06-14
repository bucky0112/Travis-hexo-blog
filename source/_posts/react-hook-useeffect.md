---
title: 使用 useEffect 來掌管元件作用
date: 2021-06-14 15:47:43
tags:
  - react
  - hook
  - useEffect
categories: React 學習系列
keywords:
  - react
  - hook
  - useEffect
decription: 介紹 React 的 Hooks 之一 - useEffect
---
可以取代 Class Component 的神兵利器之一，無疑是 useState 還有本篇介紹的 useEffect。
<!--more-->

## useEffect 基本運作的機制

在 Class Component 中，有著生命週期的 API，生命週期代表著一個元件從誕生到結束，在 Vue 也有相似的概念，每個階段有不同的 API 可以操作。

為了測試 `useEffect` 如何在元件中運作，下方寫了一個範例：

```jsx
import React, { useState, useEffect } from 'react'

export default function App() {
  console.log('first');
  const [msg, setMsg] = useState('default');
  useEffect(() => {
    console.log('useEffect');
  });

  return (
    <>
      {console.log('render')}
      <button onClick={() => setMsg('default')}>1</button>
      <button onClick={() => setMsg('change1')}>2</button>
      <button onClick={() => setMsg('change2')}>3</button>
      <h1>{msg}</h1>
    </>
  )
}
```

當每次 render 的時候，看到 console.log 的順序是，而且每點選不同按鈕顯示不同內容也是一樣：

```
'first';
'render';
'useEffect'
```

也就是不管頁面是第一次編譯或是重新編譯，`useEffect` 都會執行。

如果想要每當狀態改變時就執行 `useEffect` 的話，就加上第二個參數，並在陣列內給值，例如：

```jsx
useEffect(() => {
  console.log('useEffect');
}, [msg]);
```

如果參數沒有給值的話，則會變成只在首次編譯畫面時才會執行 `useEffect`。

這是因為參數內的值沒有改變，所以 `useEffect` 就不會執行；那麼如果在同一個頁面，由於值是一樣的，所以也是不會執行。

## 其他觸發方式

`useEffect` 還有其他運用的地方，例如想要在 state 的值改變前做其他事情的話，就可以在 `useEffect` 的函式中回傳一個函式：

```jsx
useEffect(() => {
  // do some thing
  return () => {
    // do another thing
  }
}, [])
```

下方做了一個範例：

<iframe src="https://codesandbox.io/embed/fervent-feistel-0rpjj?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="fervent-feistel-0rpjj"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

執行的順序是：

```jsx
useEffect(() => {
  (A)
	return () => {
    (B)
  }
},[])
```

* 當元件編譯完，會執行 (A)
* 當元件內的 state 改變而重新渲染的話，會先執行 (B) 再執行 (A)
* 當元件被移除時，會執行 (B)

## 適合使用 useEffect 的情境

由於 `useEffect` 會在編譯完執行的特性，所以也就非常適合拿來使用串接 API 取得資料時使用。

下方是使用 `useEffect` 來串接 API 的範例，使用的是 [政府資料開放平台](https://data.gov.tw/) 中的臺北市空品每小時監測資料，比對出士林區及臭氧的相關資訊：

<iframe height="265" style="width: 100%;" scrolling="no" title="useEffect-demo" src="https://codepen.io/bucky0112/embed/QWpJPNL?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/QWpJPNL'>useEffect-demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

