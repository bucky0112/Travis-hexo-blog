---
title: 用 Vuex 來管理 State 資料狀態
tags:
  - vuex
  - state
date: 2020-10-10 21:53:34
categories: 管理 Vue 資料的一百種方法之 Vuex
keywords:
  - mapState
  - vuex
  - state
decription: 用 Vuex 來管理 State 資料狀態
---
在之前的專案中，如果碰到要資料傳遞的部份，例如是父子層關係的話，只要用 `props` 跟 `emit` 就可以解決了，而遇到同階層的關係，就用 `$eventbus` 來處理，不過這個方法只能處理比較簡單的資料，而且在 Vue 3 也不支援了。不怕，總是有技術可以解決問題，所以就從 state 開始來學學 Vuex 吧～
<!--more-->

![image-20201026231043927](https://i.imgur.com/JY7TkcK.png)

## 什麼是 Vuex？

Vuex 是 Vue 官方開發的一種狀態管理模式，它的設計是把所有元件的狀態集中式管理，放在一個 **store** 裡面。
以下是 Vuex 的處理方法：
```
Vuex
state ----------> 資料狀態，類似 component 中的 data
actions ---------> 跟 methods 一樣，進行非同步的行為及取得資料
getter ---------> 跟 computed 一樣，資料呈現的方式
mutations -------> 改變資料內容的方法
```


## 怎麼使用 state？

由於 State 就跟 Data 差不多概念，只是 State 是一個統一存放在 Vuex 的 Store 中的 Data，要使用的話也蠻簡單的，打開 store/index.js：

![2BE446E7-3B75-4D76-82C6-079E0B65E4DD](https://i.imgur.com/RqaI0zQ.png)

直接在  state 中建立一筆資料：

```js
state: {
  movieTitle: 'Joker',
},
```

然後在需要存取的頁面中，使用 `computed` 來拿到 State：
```js
computed: {
  movieTitle() {
    return this.$store.state.movieTitle;
  },
},
```

要怎麼確定有沒有拿到 state 呢？可以使用 Vue Devtools，如果成功的話就可以看到以下畫面：

![F9A26099-07F2-4A62-9A6A-26D55756DC0A](https://i.imgur.com/eBAbVAd.png)


### 使用 mapState 來輔助存取 state

如果一個 component 中需要大量地從 store 中存取 state 的話，這時候可以使用 mapState 來省去很多 return 的步驟：

1. 從 Vuex 中 import `mapState`
2. 使用 `computed` 調用  `mapState` 拿到 Vuex 的 State

```js
import { mapState } from 'vuex';

export default {
  computed: {
    ...mapState(['movieTitle']),
  },
};
```

接著就可以在這個頁面中使用拿到的 State，例如：

```js
<p>{{ movieTitle }}</p>
```

如果在 store 中有其他 State 要調用的話，例如：

```js
state: {
  movieTitle: 'Joker',
  movieAge: 'PG13',
},
```

那麼在 computed 中就可以這樣用：

```js
<script>
import { mapState } from 'vuex';

export default {
  computed: {
    ...mapState(['movieTitle', 'movieAge']),
  },
};
</script>
```

像這樣就可以拿到從 Store 的 State

![B34E7F44-3764-4B1A-8982-7CE0D0122152](https://i.imgur.com/TMdDYi6.png)

可以從 CodeSandbox看一下實際是怎麼運作的：
<iframe src="https://codesandbox.io/embed/nice-aryabhata-uukxp?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="nice-aryabhata-uukxp"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>