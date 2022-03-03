---
title: 除了 useState，你可以選擇更好管理狀態的 useReducer
date: 2022-03-03 14:08:25
tags:
  - react
  - hook
  - useReducer
  - useContext
categories:
keywords:
  - react
  - hook
  - useReducer
  - useContext
decription: 可以取代 useState 的 useReducer
---

標題有點引戰，但是如果想要跨元件處理狀態，並且狀態非常複雜的話，那麼 useState 可能就不是那麼好管理，也許可以試試看 useReducer。

<!--more-->

## 使用情境

之前有寫過 useState 的介紹文章，裡面的範例是一個點擊器，可以透過點擊不同按鈕去反應結果，這篇就來使用 useReducer 來做出一樣的成果。

## 初始設置

首先將會分成 3 個元件，分別是加按鈕、減按鈕，還有計算結果，這邊計算結果是用 input 欄位，可以讓使用者自行輸入。

```jsx
// 減
const MinusCount = () => {
  return <button type="button">-</button>;
};

export default MinusCount;

// 加
const PlusCount = () => {
  return <button type="button">+</button>;
};

export default PlusCount;

// 結果
const Result = () => {
  return <input type="number" value={0} />;
};

export default Result;
```

## 建立預設狀態

```jsx
const initialState = {
  countNumber: 0
}
```

## 建立邏輯判斷區來操作狀態

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'PLUS':
      return {
        ...state,
        countNumber: state.countNumber + 1
      }
    case 'MINUS':
      return {
        ...state,
        countNumber: state.countNumber - 1
      }
    case 'UPDATE_NUMBER':
      return {
        ...state,
        countNumber: action.payload
      }
    default:
      return state
  }
}
```

## 使用 useReducer 來帶入狀態以及邏輯判斷

```jsx
const [state, dispatch] = useReducer(reducer, initialState)
```

這樣大致上就設置好 useReducer 了，接着就可以將狀態以及方法分別給各自的元件。

```jsx
<div className='App'>
  <MinusCount method={() => dispatch({ type: 'MINUS' })} />
  <Result num={state.countNumber} />
  <PlusCount method={() => dispatch({ type: 'PLUS' })} />
</div>
```

```jsx
// == Result ==
const Result = () => {
  const { state, dispatch } = useContext(AppContext)

  return (
    <input
      type='number'
      value={state.countNumber}
      onChange={(e) =>
        dispatch({ type: 'UPDATE_NUMBER', payload: Number(e.target.value) })
      }
    />
  )
}

// == Plus ==
const PlusCount = ({ method }) => {
  const { dispatch } = useContext(AppContext)

  return (
    <button type='button' onClick={method}>
      +
    </button>
  )
}

// == Minus ==
const MinusCount = ({ method }) => {
  const { dispatch } = useContext(AppContext)

  return (
    <button type='button' onClick={method}>
      -
    </button>
  )
}
```

## 搭配使用 useContext

剛剛是使用 props 來傳送，我自己是比較常搭配 useContext 來使用，成果如下 ⬇️

<iframe src="https://codesandbox.io/embed/usereducer-example-sc27x0?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="useReducer-example"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## 後記

在官方文件中提到 useReducer 是這樣說的：

> 當你需要複雜的 state 邏輯而且包括多個子數值或下一個 state 依賴之前的 state，useReducer 會比 useState 更適用。而且 useReducer 可以讓你觸發深層更新的 component 作效能的最佳化，因為你可以傳遞 dispatch 而不是 callback。

所以如果是比較單純的狀態的話，那麼使用 useState 就可以了，但是我最近是比較常使用 useReducer，因為寫起來蠻像 Redux 的，而且 useReducer 都把邏輯管理在一起，看起來比較容易閱讀，也方便維護（我就做過把之前的 code 給重寫，看起來就很舒服 XD），所以蠻推薦這個 hook👏。
