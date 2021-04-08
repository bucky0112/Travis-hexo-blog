---
title: 基礎 Vue.js(下)
tags:
  - vue
  - javascript
date: 2020-03-30 16:54:07
categories: vue
keywords: 
- vue
- javascript
- 前端框架
decription: Basic use about Vue.
---

最後一篇 Vue 的基礎，會提到修飾符、還有 v-bind 切換 class，表單的運用，還有 component 的概念。
<!--more-->

## 修飾符
---

修飾符在網頁中很常使用到，例如點擊一個 a 連結，不想讓他產生作用的話，就會使用到 `preventDefault()` 這個語法，例如 jQuery 的用法：

```
<a href="http://www.google.com">Click me</a>
    
<script>
  $("a").on("click", function(e) {
    e.preventDefault();
  });
</script>
```

這樣子點擊的話，連結就不會有作用。

而在 Vue 中也有這個用法，阻止默認行為：

`<button @click.prevent="doThis"></button>`

```
<div id="app">
  <input type="text" v-model="text">
  <!-- 當點擊a連結，一樣可以執行function，但不會連到Google -->
  <a href="http://www.google.com" @click.prevent="reverseText">Reverse Text</a>
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
    	  this.newText = this.text.split("").reverse().join("");
    	}
    }
  });
</script>
```

## 透過 v-bind:class 來綁定 HTML
---

可以給予 `v-bind:class` 一個 class 對象，去做出動態切換 class

`<div v-bind:class="{ class名稱: 切換的動作 }"></div>`

看以下範例，有一個藍色的方塊，只要在 class 名稱加上 yellow，就會變成黃色：

```
<div id="app">
  <!-- 只要在class動態加上yellow就會變黃色 -->
  <div class="box"></div>
  <button class="changeColor btn btn-primary">Change Color</button>
</div>
    
<style>
  .box {
    width: 250px;
    height: 250px;
    background-color: blue;
  }
    
  .box.yellow {
    background-color: yellow;
  }
</style>
    
<script>
  var app = new Vue({
    el: "#app",
    data: {
    	change: false  // 這邊有一個切換動作change的data
    }
  });
</script>
```

```    
<div id="app">
  <!-- 加上 v-bind:class="{class名稱: 切換的動作}" -->
  <div class="box" :class="{'yellow': change}"></div>
  <!-- 在按鈕綁定click，讓它去做切換的效果 -->
  <button class="changeColor btn btn-primary" @click="change = !change">Change Color</button>
</div>
```

{%iframe https://codepen.io/bucky0112/embed/dyoJaow?height=265&theme-id=dark&default-tab=html,result %}

## 計算屬性 computed 的使用
---

下面的範例是當在 `text` 欄位輸入文字時，會在 `.showText` 顯示反轉文字

不過

```
<div id="app">
  <h2>直接輸入文字</h2>
  <input type="text" class="form-control mt-3" v-model="text">
  <h2>在下方顯示反轉文字</h2>
  <h3 class="showText">
    {{text.split("").reverse().join("")}}
  </h3>
</div>
    
<script>
  var app = new Vue({
    el: "#app",
    data: {
    	text: ""
    }
</script>
```

但是如果要重複使用 `{% raw %}{{text.split("").reverse().join("")}}{% endraw %}` 這一段的話，會有點難維護，所以可以運用 computed 來處理

```
var app = new Vue({
  el: "#app",
  data: {
    text: ""
  },
  computed: {
    reverseText: function() {  // 在 computed 中要使用 function
    	return this.text.split("").reverse().join(""); // 並且會回傳值，所以就可以應用在回傳反轉後的結果
    }
  }
});
```

接著只要加上回傳值的 function，就可以顯示結果了。

```
<div id="app">
  <h2>直接輸入文字</h2>
  <input type="text" class="form-control mt-3" v-model="text">
  <h2>在下方顯示反轉文字</h2>
  <h3 class="showText">
    {{reverseText}}
  </h3>
</div>
```

那麼 computed 跟 methods 在使用上有什麼差異呢？

- computed 一般用來回傳用於畫面呈現的資料 **在監控資料更動後，重新運算後將結果呈現於畫面上。由於資料變動就會觸發，所以如果運行的資料太多，在效能處理上就會變慢。**
- methods 是運用在互動的函式，可以用來修改資料，內容因為需要觸發才會運作，所以如果資料量大的話會建議使用 methods。

## Vue 表單與資料的綁定
---

前面有提到 Vue 雙向綁定用 `v-model` 的用法，這個部份來看看其他綁定的用法。

### checkbox

```
<div id="app">
  <div class="form">
    <label for="dinner">要吃晚餐嗎？</label>
    <input type="checkbox" v-model="checkboxDinner" id="dinner">
      {{checkboxDinner}}
  </div>
</div>


<script>
  var app = new Vue ({
  	el: "#app",
  	data: {
  	  checkboxDinner: false, // checkbox 選項只有true或false
  	}
  })
</script>
```

`checkbox` 的用法這邊用在是或不是的選項 ，這邊預設是 false，當點擊時就會變成 true。

還有加入 Array 的用法：

```
var app = new Vue ({
  el: "#app",
  data: {
    checkboxArray: [],
  }
})
```

這裏有綁定 3 個選項，當點擊 `checkbox` ，data 的 checkboxArray 空陣列就會填入該選項的 value，最後會顯示在 `<span>` 中的 `v-for` 迴圈。

```
<div id="app">
  <div class="form">
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="check2" value="雞" v-model="checkboxArray">
      <label class="form-check-label" for="check2">雞</label>
    </div>
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="check3" value="豬" v-model="checkboxArray">
      <label class="form-check-label" for="check3">豬</label>
    </div>
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="check4" value="牛" v-model="checkboxArray">
      <label class="form-check-label" for="check4">牛</label>
    </div>
    <p>晚餐火鍋裡有<span v-for="item in checkboxArray">{{item}}</span>。</p>
  </div>
</div>
```

`checkbox` 還可以另一種的運用，就是只顯示單選，例如當選擇一個選項就會將 value 綁入 singleRadio 的空字串中

```
var app = new Vue ({
  el: "#app",
  data: {
    singleRadio: "",
  }
})
    
<div id="app">
  <div class="form-check">
    <input type="radio" class="form-check-input" id="radio2" value="雞" v-model="singleRadio">
    <label class="form-check-label" for="radio2">雞</label>
  </div>
  <div class="form-check">
    <input type="radio" class="form-check-input" id="radio3" value="豬" v-model="singleRadio">
    <label class="form-check-label" for="radio3">豬</label>
  </div>
  <div class="form-check">
    <input type="radio" class="form-check-input" id="radio4" value="牛" v-model="singleRadio">
    <label class="form-check-label" for="radio4">牛</label>
  </div>
  <p>晚餐火鍋裡有 {{singleRadio}}。</p>
</div>
```

### 下拉式的選單

當選擇某一個選項時，會將 value 綁入 data 的 selected 空字串中，然後顯示在 `{{selected}}`

```
<div id="app">
  <h2>喜歡澳洲哪一個城市？</h2>
  <select name="" id="" class="form-control" v-model="selected">
    <option value="" disabled>-- 請選擇地點 --</option>
    <option value="Melbourne">墨爾本</option>
    <option value="Sydney">雪梨</option>
    <option value="Adelaide">阿德雷德</option>
  </select>
  {{selected}}
</div>
    
<script>
  var app = new Vue ({
    el: "#app",
    data: {
    	selected: "",
    }
  })
</script>
```

## component 元件基礎概念
---

以下有一個範例，當點擊按鈕時，透過 `v-on:click` 綁定 counter 的按鈕會 + 1：

```
<!-- 當點擊按鈕時，透過 v-on:click綁定counter的按鈕會+1 -->
<div id="app">
  <div>
    目前點擊<button @click="counter += 1">{{counter}}</button>下
  </div>
</div>
    
<script>
  let app = new Vue({
    el: "#app",
    data: {
    	counter: 0
    }
  })
</script>
```

如果新增另一個按鈕的話，會怎麼樣呢？

```
<div id="app">
  <div>
    目前點擊<button @click="counter += 1">{{counter}}</button>下
  </div>
  <div>
    目前點擊<button @click="counter += 1">{{counter}}</button>下
  </div>
</div>
```

因為兩個是共用同一個變數 counter，所以當按其中一個按鈕，兩個會一起 + 1。

那麼如果想要讓個別按鈕資料獨立的話，這邊有另一個方法可以使用：

**component 元件**，透過這樣的方式，可以讓每個 component 中的 data 都會是互相獨立，看以下範例：

```
// Vue.component(tagName, options)
Vue.component('counter-component', {
  template: `<div>
    C目前點擊<button @click="counter += 1">{{counter}}</button>下
  </div>`,
  data: function() {
    return {
      counter: 0 
    }
  },
});
    
let app = new Vue({
  el: "#app",
  data: {
    counter: 0
  }
})
```

tagName 可以取任意的名字，但是要注意必須是小寫，如果是多組字，就要使用 kebab Case 來命名。

option 裡面有 template 跟 data，template 是要顯示在頁面上的樣板，注意要用 `` 包起來，data 在這邊則是 function 並 return 值。

這樣點擊的數量就會是單獨計算，就算再新增一個 component 也一樣會分開計算。

```
<div id="app">
  <div>
    A目前點擊<button @click="counter += 1">{{counter}}</button>下
  </div>
  <!-- 兩者所點擊的數量是分開計算 -->
  <counter-component></counter-component>
  <counter-component></counter-component>
</div>
```