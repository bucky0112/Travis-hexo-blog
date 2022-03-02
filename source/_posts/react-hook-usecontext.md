---
title: 使用 useContext 處理跨元件狀態
date: 2022-03-02 16:09:10
tags:
  - react
  - hook
  - useContext
categories: React 學習系列
keywords:
  - react
  - hook
  - useContext
decription: 使用 useContext 處理跨元件狀態
---

寫了這麼久 React，發現我的筆記都沒整理過，趁最近比較不忙來清理一下筆記 XD
這次要分享的是跨元件處理狀態的 hook -- useContext。

<!--more-->

## 使用情境

一般寫 React 時，如果需要將元件的狀態傳遞的話，如果是簡單的父子元件傳遞，可以用 props 來傳。但如果是很多層的情況，或是同層的子組件要互傳狀態的話，可能用 props 就不是這麼適合。當然也可以使用一些狀態管理的套件，例如：Redux（之後會找時間分享）。不過，其實 React 內建就有 useContext 這個 hook，這邊就來示範一下作法：

使用的情境是分別有兩個元件，有一個輸入框 - A，跟一個顯示輸入的文字 - B，如果 A 改變輸入的文字，那麼 B 就會即時顯示該文字。

## 初始設置

以下是原始的程式碼，可以看到分別有 InputText.js 跟 ShowText.js 兩個元件放在 App.js 中

```jsx
// App.js
import InputText from './InputText'
import ShowText from './ShowText'

export default function App() {

  return (
    <div className='App'>
      <InputText />
      <ShowText />
    </div>
  )
}

// InputText
const InputText = () => {

  return (
    <div>
      <input type="text" />
    </div>
  );
};

export default InputText;

// ShowText
const ShowText = () => {

  return (
    <div>
      <p>Hello World</p>
    </div>
  );
};

export default ShowText;
```

## 使用 createContext 建立共用狀態

首先 import createContext，並且設一個變數，然後 export 該變數，例如： AppContext，等等會給其他元件使用。

```jsx
import { createContext } from 'react'

export const AppContext = createContext()
```

接著在要傳遞狀態的父元件，在這個範例也就是 App.js 中，在最外層用 AppContext 將所有子元件包起來，並且設置 value。

```jsx
<AppContext.Provider value={{}}>
  <div>
    <InputText />
    <ShowText />
  </div>
</AppContext.Provider>
```

可以發現 value 是一個物件，裡面可以放入任何狀態，等等就可以讓其他元件使用，這邊用一個 useState 來處理狀態。

```jsx
import { createContext, useState } from 'react'

export default function App() {
  const [text, setText] = useState('Hello World')

  return (
    <AppContext.Provider value={{ text, setText }}>
      <div className='App'>
        <InputText />
        <ShowText />
      </div>
    </AppContext.Provider>
  )
}
```

## 利用 useContext 讓其他元件處理狀態

### InputText

首先 import useContext，並且參數帶入一開始建立的 AppContext，然後從裡面取出 setText 來變更狀態。

```jsx
import { useContext } from 'react'
import { AppContext } from './App'

const InputText = () => {
  const { text, setText } = useContext(AppContext)

  return (
    <div>
      <input type='text' value={text} onChange={(e) => setText(e.target.value)} />
    </div>
  )
}
```

### ShowText

一樣 import useContext，並且參數帶入一開始建立的 AppContext，然後從裡面取出 text 來顯示狀態。

```jsx
import { useContext } from 'react'
import { AppContext } from './App'

const ShowText = () => {
  const { text } = useContext(AppContext)

  return (
    <div>
      <p>{text}</p>
    </div>
  )
}
```

這樣本次範例就完成了，下面有 Demo 可以試玩看看 ⬇️

<iframe src="https://codesandbox.io/embed/usecontext-example-ciw09g?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="useContext-example"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
