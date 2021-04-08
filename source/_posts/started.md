---
title: 開始我的 Vue 3
tags:
  - vue 3
date: 2021-02-10 10:55:50
categories: Vue 3 學習系列
keywords:
  - vue 3
  - fragment
decription: 從 0 開始學習 Vue 3
---
Vue 3 正式版問世也一段時間了，也是時候來認識一下新朋友了，前端就是要不停地追技術啊0.0
<!--more-->

## 前言

自從 Vue 3 的消息不斷傳出，相信主力開發工具為 Vue 的前端工程師也多少都有耳聞，小弟我也不例外，常常會跟朋友討論相關訊息。而當正式版（代號：one piece）發布之後，全世界前端大概都開始了尋找大秘寶之旅。

![img](https://i.imgur.com/r1znob8.png)

雖然公司目前專案大概也還不到使用 Vue 3 的時機，但是多了解新技術總是不會錯。剛好 Vue.js Taiwan 的主辦人 - Kuro 大大，在過年前出了一本 Vue 3 的書 - [重新認識 Vue.js：008天絕對看不完的 Vue.js 3 指南](https://www.tenlong.com.tw/products/9789864345687)，想趁著過年期間來學一下，本系列就是紀錄我學習 Vue 3 的歷程，那麼就開始吧！

## 安裝

* 如果想要小試一下的話，可以使用 CDN 的方式來掛載，只要加上：

  ```html
  <script src="https://unpkg.com/vue@next"></script>
  ```

* 有經驗的開發者想透過 Vue-CLI 的話：

  ```bash
  $ npm install -g @vue/cli
  $ vue create 專案名稱
  ```

## 來寫個 Hello World 吧

以往在 Vue 2.x 版本中的寫法：

```html
<div id="app">
  {{ text }}
</div>
```

```js
const vm = new Vue({
  el: '#app',
  data() {
    return {
      text: "Hello World",
    };
  },
})

// 或是使用 mount 掛載
const vm = new Vue({
  data() {
    return {
      text: "Hello",
    };
  },
})

vm.$mount("#app");
```

而在 Vue 3 則是加入了 Composition API 的寫法：

```js
const { createApp, ref } = Vue;

const vm = createApp({
  setup() {
    const text = ref("Hello World");
    return {
      text
    };
  }
});

vm.mount("#app");
```

<font size="1">謎之音：不得不說，看起來還真有點像 React。</font>

## Vue 3 試玩後有感的新增特性

根據 [Vue 官方文件](https://v3.vuejs.org/guide/migration/introduction.html#notable-new-features)中介紹的新增特性有不少，其中最有感的是上方有提到的 Composition API，還有 Fragment ，讓原本必須要單一根元素下編譯模板的情況下，取消了這個限制，將會變得更加直覺。

<iframe height="265" style="width: 100%;" scrolling="no" title="Vue 2  Fragment" src="https://codepen.io/bucky0112/embed/mdOrRNv?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/mdOrRNv'>Vue 2  Fragment</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

像上面的 CodePen 例子，原本在 Vue 2 中，template 中如果有一個以上的元素，那麼必須使用一個 `<div>` 包起來才能正常顯示。

而在 Vue 3 中，就少了這個限制：

<iframe height="265" style="width: 100%;" scrolling="no" title="Vue 3  Fragment" src="https://codepen.io/bucky0112/embed/zYoKNWN?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/zYoKNWN'>Vue 3  Fragment</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

其他還有很多新功能等待發掘，就一起來加入 Vue 3 的行列吧！