---
title: 使用 Actions 跟 Mutations 來改變資料狀態
tags:
  - vuex
  - actions
  - mutations
date: 2020-10-12 00:02:47
categories: 管理 Vue 資料的一百種方法之 Vuex
keywords:
  - vuex
  - actions
  - mutations
decription: 使用 Actions 跟 Mutations 來改變資料狀態
---
上一篇已經初步了解 State，這篇繼續試著了解怎麼透過 Actions 跟 Mutations 來改變 State。
<!--more-->

## Actions 跟 Mutations 如何改變資料

先看官網的 Vuex 的週期介紹圖，如果 Component 要改變 State 的話，就要透過 Actions 發出 Commit 去呼叫 Mutations，藉由 Mutations 去改變 State。

![vuex](https://i.imgur.com/J10wJPU.png)

### 如何使用

我在 CodeSandbox 建立了一個 Vuex 來模擬變更資料，當按下 Click Me 按鈕，原本顯示在頁面上的 Hello Vuex 會變更為 Good Job，在過 5 秒後則會再變更為 Try Again，就可以繼續按，觀察資料如何變更。
<iframe src="https://codesandbox.io/embed/gracious-lederberg-n4um6?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="gracious-lederberg-n4um6"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

首先在 State 中設立一筆資料

```js
state: {
  testWord: "Hello Vuex"
}
```

接著就可以透過 Actions 跟 Mutations 來改變資料：

* 在 Actions 及 Mutations 加入：

```js
mutations: {
  CHANGE(state, word) {
    state.testWord = word;
  }
},
actions: {
  changeStateWord(context, word) {
    context.commit("CHANGE", word);
  }
},
```

1. 在 actions 建立一個函式，並帶入 `context` 為第一個參數，第二個參數則是 `payload`，可以自定義名稱，而 `context` 則是有下列屬性：

```js
{
  state,      // 等同于 `store.state`，若在模块中则为局部状态
  rootState,  // 等同于 `store.state`，只存在于模块中
  commit,     // 等同于 `store.commit`
  dispatch,   // 等同于 `store.dispatch`
  getters,    // 等同于 `store.getters`
  rootGetters // 等同于 `store.getters`，只存在于模块中
}
```

2. 接著在函式中使用 `commit` 屬性，呼叫 `mutations`。
3. 在 `mutations` 也建立一個函式，建議使用常數來命名方便區分，並帶入 `state` 為第一個參數，第二個參數則是 `payload` 。
4. 因為要藉由 `mutations` 來改變 `state`，所以在函式中加入動作。
5. 最後在 component 中加入 Actions，使用 `this.$store.dispatch('actions', 欲變更資料)`

## 使用嚴格模式

在使用 Actions 跟 Mutations 時，還需要特別注意的是，當使用非同步處理時，要在 Actions 處理，而不要在 Mutations。
這時候可以開啟嚴格模式，幫助追蹤所有的狀態變更，使用方法很簡單，在 Vuex 中加入 strict，並開啟即可：

```js
export default new Vuex.Store({
  strict: true
})
```