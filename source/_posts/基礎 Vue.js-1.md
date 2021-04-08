---
title: 基礎 Vue.js(上)
tags:
  - vue
  - javascript
date: 2020-03-30 11:34:35
categories: vue
keywords: 
- vue
- javascript
- 前端框架
decription: Basic use about Vue.
---

身為目前 3 大前端應用框架之一的 Vue.js，截至目前為止在 GitHub 已經累積 160k 的星星數，以下是個人的學習筆記。
<!--more-->

## 開發環境
---

在 [Vue.js官網](https://cn.vuejs.org/v2/guide/installation.html) 中建議在瀏覽器上安裝 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)，這樣可以方便在瀏覽器中觀看訊息。

在 `<script>` 中直接載入 Vue 的檔案，在官網有提供各式檔案來源可供存取，要注意的是如果在開發環境下，建議使用開發版本，這樣會提供完整的警告訊息，方便開發者去查閱問題來源。

## 應用程式建立
---

首先在 HTML 建立一個 `div` ，這邊可以使用 id 或是 class，一般建議是使用 id，

接著在 `<script>` 中輸入建立 Vue 的起手式

```
<div>
  <div id="app"></div>
</div>
    
<script>
  var app = new Vue({
    el: '#app'
  })
</script>
```

然後打開瀏覽器的開發人員工具，如果有安裝  [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) 就可以看到已經建立了一個 Root

![](https://i.imgur.com/01jwjmg.png)

如果要建立資料的話，然後顯示在 HTML 上：

```
<div>
  <div id="app">
  <!-- 建立完data，想要顯示message的話，在 {{ }} 中放入名稱就可以顯示 -->
    {{message}} 
  </div>
</div>
    
<script>
  var app = new Vue({
    el: '#app',
    // 建立data，裡面可以存放資料，例如 message
    data: {
      message: 'Hello World'
    }
  })
</script>
```

注意事項： 一個頁面可以同時建立 2 個 app，但是 3 個就沒辦法

```
<div>
  <div id="app">
    {{message}} 
  </div>
    
  <div id="app2">
    {{message}} 
  </div>
</div>
    
<script>
  var app = new Vue({
    el: '#app',
    data: {
    	message: 'Hello'
    }
  })
    
  var app2 = new Vue({
    el: 'app2',
    data: {
    	message: 'World'
    }
  })
</script>
```

建立兩個是可以的，一樣會出現 2 個 Root

![](https://i.imgur.com/BYN501c.png)

但是如果建立 3 個 app，就會出現找不到 element

![](https://i.imgur.com/eyWxDkg.png)

## 雙向綁定的資料

在 Vue 中，有雙向綁定的特色，

前面提到如果要將資料顯示在網頁上，可以使用 `{% raw %}{{  }}{% endraw %}` 這個語法，在 Vue 的語法中還有其他可以使用的：

- v-model
- v-text
- v-html

### v-model 的使用

主要是使用在：

- `<input>`
- `<select>`
- `<textarea>`
- components

例如：

```
<div id="app">
  {{message}}
  <input type="text" v-model="message">
</div>
    
<script>
  var app = new Vue({
    el: '#app',
    data: {
    	message: 'Hello'
    }
  })
</script>
```

在畫面上 `<text>` 跟 `{{message}}` 就會顯示一樣的內容

![](https://i.imgur.com/bwOOIxu.png)

而當直接在畫面修改 `<text>` 輸入欄中的內容時，`{{message}}`也會一起改變。

![](https://i.imgur.com/NvbXPV9.png)

### v-text 跟 v-html 使用的方法

兩者差不多，都可以直接顯示內容，差別在 `v-html` 可以加入 html 標籤

```
<div id="app">
  {{message}}
  <input type="text" v-model="message">
  <div v-text="message"></div>
  <div v-html="message"></div>
</div>
    
<script>
  var app = new Vue({
    el: '#app',
    
    data: {
    	message: '<h1>Hello</h1>'
    }
  })
</script>
```

在畫面上的呈現是這樣：

![](https://i.imgur.com/Rhr0U3R.png)