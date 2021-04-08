---
title: 基礎 Vue.js(中)
tags:
  - vue
  - javascript
date: 2020-03-30 16:52:44
categories: vue
keywords: 
- vue
- javascript
- 前端框架
decription: Basic use about Vue.
---

此篇會講到關於 MVVM 的概念、綁定、for 迴圈及判斷，還有行為 on 的使用。
<!--more-->

## MVVM 的概念
---

首先要先提到傳統 MVC 的概念，

在 Web 應用程式的劃分分為：

- 模型（Model）
- 視圖（View）
- 控制器（Controller）

![](https://i.imgur.com/lmb7P7R.png)
> 圖片來自 TechTerms.com

使用者第一眼看到的就是 View，如果使用者想要取得某些資料，這時 View 會發送請求給 Controller，然後 Controller 會請 Model 找出資料，Model 就會另外在資料庫找出資料，Model 取得資料後，再回傳給 Controller，Controller 再呈現出畫面在 View 上。

### 那麼 MVVM 又是什麼概念？

View 跟 ViewModel 是綁定的，如果使用者想要取得資料，View 會直接請求給 Model，然後 Model 透過 Database 找到資料再回傳給 ViewModel，會直接即時顯示在 View 上。

![](https://i.imgur.com/hYs0zLQ.png)
> 圖片來自維基百科

### 所以 Vue.js 跟 一般 JavaScript 有什麼不同？

一般 JavaScript 在處理畫面上是直接操作 Dom 元素

而 Vue.js 是以資料狀態操作 Dom 元素時，是以資料狀態去變動。

## v-bind 動態屬性指令
---

上面提到 Vue.js 是以資料狀態去變動，這邊要來實際操作一下如何載入圖片：

`<img v-bind:src="imageSrc">`

縮寫

`<img :src="imageSrc">`

以下範例：

```
<div id='app'>
  <!-- 綁定一個屬性src -->
  <img v-bind:src="imgSrc"> 
</div>
    
<script>
  var app = new Vue({
    el: '#app', 
    data: {
    	imgSrc: 'https://images.unsplash.com/photo-1529778873920-4da4926a72c2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1576&q=80'
    }
  })
</scripy>
```

這樣就可以讓圖片顯示出來了

{% iframe https://codepen.io/bucky0112/embed/KKpyXJB?height=361&theme-id=dark&default-tab=js,result %}


不過圖片似乎太大，所以還可以改小一點

這邊使用 Bootstrap 的 `.img-fluid` ，讓圖片設定為響應式。

```
<div id='app'>
  <!-- 多綁定一個屬性class -->
  <img v-bind:src="imgSrc" v-bind:class="className"> 
</div>
    
<script>
  var app = new Vue({
    el: '#app', 
    data: {
    	imgSrc: 'https://images.unsplash.com/photo-1529778873920-4da4926a72c2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1576&q=80',
    	className: 'img-fluid'
    }
  })
</scripy> 
```

{% iframe https://codepen.io/bucky0112/embed/wvaPPXQ?height=265&theme-id=dark&default-tab=js,result %}

## v-if 及 v-for
---

這邊的 data 中有一組陣列屬於 JSON 格式，要運用 `v-for` 讓它顯示在畫面上

```
var app = new Vue({
  el: "#app",
  data: {
    list: [
      {
        name: "小明",
        age: 16
      },
      {
        name: "媽媽",
        age: 38
      },
      {
        name: "漂亮阿姨",
        age: 24
      }
    ]
  }
});
```

這邊有一個語法 `<pre></pre>`，可以讓內容先在頁面上顯示出來，以方便開發者觀看。

```
<div id="app">
  <pre>{{list}}</pre>
</div>
```

如果要讓全部的資料使用 for 迴圈跑出來，在 Vue 的方式是用 `v-for`

```
<div id="app">
  <ul>
    <!-- item可以使用任意名稱 -->
    <li v-for="item in list">
      {{item.name}}的年齡是 {{item.age}}
    </li>
  </ul>
</div>
```

會顯示

- 小明的年齡是 16
- 媽媽的年齡是 38
- 漂亮阿姨的年齡是 24

如果想要顯示每一個 `li` 在陣列中索引是第幾個

```
<div id="app">
  <ul>
      <!-- (陣列的值, 陣列的索引) -->
    <li v-for="(item, index) in list">
      <!-- 索引是從 0 開始，如果要從 1 開始顯示要記得加 1 -->
      {{index + 1}} - {{item.name}}的年齡是 {{item.age}}
    </li>
  </ul>
</div>
```

如果要顯示大於 24 的資料的話，可以使用判斷式 `v-if`

```
<div id="app">
  <ul>
    <li v-for="(item, index) in list" v-if="item.age > 24">
      {{index + 1}} - {{item.name}}的年齡是 {{item.age}}
    </li>
  </ul>
</div>
```

{% iframe https://codepen.io/bucky0112/embed/BaNmJaE?height=265&theme-id=dark&default-tab=js,result %}

## 利用 v-on 來操作行為
---

在 jQuery 中，如果要操作行為的話，會使用 

```
$(selector).on(events, function () {
        
});
```

而在 Vue 中，方法差不多，採用的是 `v-on`，範例如下：

```
<!-- v-on:click 可以使用縮寫成 @click -->
<button v-on:click="doThis"></button>
```

這邊做一個反轉文字的練習

```
<div id="app">
  <!-- 用v-on:keyup.enter，綁定鍵盤按Enter事件到reverseText這個function -->
  <input type="text" v-model="text" v-on:keyup.enter="reverseText">
  <!-- 用v-on:click綁定點擊事件到reverseText這個function -->
  <button v-on:click="reverseText">Reverse Text</button>
  <div class="showText">
    {{newText}}
  </div>
</div>
    
<script>
  var app = new Vue({
    el: "#app",
    data: {
    	text: "",
    	newText: ""
    },
    methods: {
    	reverseText: function() {
        // 反轉文字的語法 split("").reverse().join("")
    		// 當點擊按鈕後啟動function，this.newText顯示內容
    	  this.newText = this.text.split("").reverse().join("");
    	}
    }
  });
</script>
```

> 重點 1：這邊控制資料要加上 "this"，像是 "this.newText"，沒有的話就不會顯示。

> 重點 2 : 預先定義資料狀態很重要，如果 data 中的資料沒有先定義好，會發生錯誤

{% iframe https://codepen.io/bucky0112/embed/abOEdRq?height=265&theme-id=dark&default-tab=js,result %}