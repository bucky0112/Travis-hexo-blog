---
title: Vuex 中的 Computed 用法 - Getters
tags:
  - getters
  - Vuex
date: 2021-03-18 21:30:50
categories: 管理 Vue 資料的一百種方法之 Vuex
keywords:
  - getters
  - Vuex
decription: Vuex 中的 Computed 用法 - Getters
---
在公司專案中，有多筆資料是從 `actions` 的回傳結果再透過 `mutations` 來更改資料狀態。這時候想要透過 computed 來處理這些資料，但是還要再把資料傳入 components 中再做處理，感覺總是沒那麼即時，如果在 Vuex 裡面也可以做 computed 就好了...咦？還真的有嗎！？太方便了吧！
<!--more-->

## 需求

我的資料有複數筆陣列，結構如下示範：

```js
const list_one = ['A001', 'A002', 'A003', 'A004'],
const list_two = ['B001', 'B002', 'B003'],
const list_three = ['C001', 'C002', 'C003', 'C004', 'C005']
```

我的需要是將複數筆陣列的資料給合併成一個陣列，然後再去做資料處理。

## Getters 的用法

如果要把 `state` 的資料給傳入 component 中再做處理也是一種作法，但是先在 Vuex 中做完，然後再拿到處理好的資料看要再做其他處理，這樣應該會是一個更好的作法。

以下是我在 CodeSandbox 做的範例

<iframe src="https://codesandbox.io/embed/festive-chaplygin-fr72u?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="festive-chaplygin-fr72u"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

作法跟一般 component  中的 `computed` 差不多，差別在需要帶一個參數，也就是資料來源 `state`：

```vue
state: {
  list_one: ["A001", "A002", "A003", "A004"],
  list_two: ["B001", "B002", "B003"],
  list_three: ["C001", "C002", "C003", "C004", "C005"]
},
getters: {
  combineArray(state) {
    const { list_one, list_two, list_three } = state;
    return list_one.concat(list_two, list_three); // 這裏使用 concat() 來做合併陣列
  }
}
```

接著在 component 中來接收資料狀態：

```vue
computed: {
  combineArray() {
    return this.$store.getters.combineArray;
  }
}
```

如果有多筆來自 Vuex 的資料的話，最方便可以使用 `mapGetters`，效果也是一樣的：

```js
<script>
import { mapGetters } from "vuex";
export default {
  computed: {
    ...mapGetters(["combineArray"]),
  },
};
</script>
```

