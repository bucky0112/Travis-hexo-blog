---
title: 了解 Redux 並實作一個範例
date: 2022-03-10 14:42:54
tags:
  - redux
categories: 好用函式庫介紹
keywords:
  - redux
decription: 了解 Redux 並實作一個範例
---

在開發網站時，有時狀態可能會傳來傳去，不好掌控狀態流，就可能會有 bug，這不是開發者樂見的，還好我們有 Redux 可以幫忙處理這個問題。

## 什麼是 Redux

Redux 是一個管理狀態的 JavaScript library，一般會跟 React 搭配使用。但其實可以運用在原生的 JavaScript，甚至在其他 JavaScript 框架也能使用。

## Redux 核心概念

可以參照下圖所示 ⬇️
![Image](https://i.imgur.com/9kz6RWV.png)

- Redux 的概念是所有 state 是來自建立的 store 統一管理，而 state 是唯讀，能夠確保不會隨意變更。
- 當某一個元件想要改變 state 時，必須要透過 dispatch 去發送 action。
- Reducer 是一個 pure function，會帶入 state 和 action，並且回傳一個新的 state，藉由這樣的動作去變異 store 中管理的 state。

## 實作範例

這篇會透過原生的 JavaScript 來實作一個加減點擊案例。

### 從 Redux 取出 creatStore，並建立一個 store 變數

```js
const { createStore } = Redux

const store = createStore()
```

### 建立預設狀態

```js
const initialState = {
  counter: 0
}
```

### 建立 reducer

帶入兩個參數，並將 state 指定爲剛剛建立的 initialState ，而 action 則是待會要 dispatch。

邏輯區加入加入 if...else，判斷等一下 dispatch 的 type 是什麼，並且 return 一個物件，最後如果不做處理的話 return 一個原始值：

```js
const counterReducer = (state = initialState, action) => {
  if (action.type === "plus") {
    return {
			...state
      counter: state.counter + 5
    }
  }

  return state
}
```

或是寫成 switch，然後寫好加減的方法

```js
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'plus':
      return {
        ...state,
        counter: state.counter + action.payload
      }
    case 'minus':
      return {
        ...state,
        counter: state.counter - 5
      }
    default:
      return state
  }
}
```

如果確定要代入的是什麼值的話，可以在 state 給定值，如果不確定或是想要另外做處理，就可以在 action 的 payload 再另外給。

### 在 store 傳入 reducer

```js
const store = createStore(counterReducer)
```

### 建立一個 action 來呼叫 reducer

getState() 是 store 中內建的 method，會回傳當下的 state

```js
const counterSubscriber = () => {
  const latestState = store.getState()
}
```

### 加入一個變異監聽器

使用 store 中內建的 subscribe()，並帶入參數爲剛剛建立的 action

```js
store.subscribe(counterSubscriber)
```

### 新增 dispatch

觸發 state 變異的唯一方法，帶入 action 參數，並且必須要有 type 屬性，就帶入想要呼叫的 action type 即可，這邊綁定按鈕並監聽事件。

前面的 reducer 中有寫到 action.payload 可以自己帶入，所以這邊 payload 我們帶入想要的值。

```js
const plusNumBTN = document.querySelector('.plus-num')

plusNumBTN.addEventListener(
  'click',
  () => store.dispatch({ type: 'plus', payload: 5 }),
  false
)
```

下方是一個完整的範例，有興趣可以玩看看 ⬇️

<iframe height="300" style="width: 100%;" scrolling="no" title="Redux-demo" src="https://codepen.io/bucky0112/embed/YzVvjLG?default-tab=js%2Cresult&theme-id=dark" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
See the Pen <a href="https://codepen.io/bucky0112/pen/YzVvjLG">
Redux-demo</a> by Bucky Chu (<a href="https://codepen.io/bucky0112">@bucky0112</a>)
on <a href="https://codepen.io">CodePen</a>.
</iframe>
