---
title: 用 Vue.js 做一個 TodoList
tags:
  - vue
  - todolist
  - w3HexSchool
date: 2020-05-09 13:11:35
categories: vue
keywords:
  - vue
  - todolist
  - v-model
  - v-if
  - v-for
  - methods
  - computed
  - filters
  - watch
decription: 使用 Vue 做出一個簡易的 TodoList
---
很多教學都會試著做出 TodoList 來驗收自己學習的成果，這篇文章也不免俗的將會運用目前學到的 Vue 技術，做出一個簡易的 TodoList。 
<!--more-->

使用 Vue 來做 TodoList 真的蠻方便的，以前我覺得做這個好麻煩，用了 Vue 來做快上不少。
話不多說，先看成果：

<iframe width="100%" height="300" src="//jsfiddle.net/bucky0112/dcg2jzs4/5/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 建立 Vue 環境
---

首先在 HTML 頁面上建置一個 Vue 的環境，讓資料可以透過 Vue 渲染到 HTML。

```
<div id="app">
  <h1>{{ title }}</h1>
</div>
```

資料部份：

```
let vm = new Vue ({
	el: '#app',
  data: {
  	title: 'Simple TodoList',
  }
})
```

## 輸入欄位雙向綁定
---

1. 新增一個 input 欄位，讓輸入的文字可以用 `v-model` 雙向綁定在 `inputNewTodo` 中，在 Vue 的資料中是一個空字串，讓輸入的文字可以塞入：

```
data: {
  title: 'Simple TodoList',
  inputNewTodo: ''
}
```

```
<label>輸入待辦事項：
  <input type="text"
    name="inputNewTodo"
    placeholder="請輸入事項"
    v-model="inputNewTodo"
  >
</label>
```

2. 將輸入的文字可以即時顯示，加上 `v-if` 跟 `v-else` 的判斷條件，可以顯示不同狀態的文字：

```
<div class="showNewTodo">
  <p v-if="inputNewTodo!=''">  // 如果有東西就顯示
    你新增的待辦事項：{{ inputNewTodo }}  // 即時連動輸入的文字
  </p>
      
  <p v-else>  // 如果是空字串就顯示
    尚未新增待辦事項。
  </p>
</div>
```

### 待辦事項

輸入一些待辦事項，首先用一些事項用 checkbox 看看：

```
<div class="todos">
  <h2>待辦事項：</h2>
  <label>
    <input type="checkbox">
    洗車
  </label>
    
  <label>
    <input type="checkbox">
    洗衣服
  </label>
    
  <label>
    <input type="checkbox">
    玩森友會
  </label>
</div>
```

<iframe height="265" style="width: 100%;" scrolling="no" title="QWjZyVJ" src="https://codepen.io/bucky0112/embed/QWjZyVJ?height=265&theme-id=light&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/QWjZyVJ'>QWjZyVJ</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

雖然資料有顯示出來，但是如果資料的部分用在 Vue 的 data 中會比較好管理，所以 HTML 的部份改成：

```
<div class="showTodos">
  <h2>待辦事項：</h2>
  <label v-for="item in todos">  // 前面不一定要叫 item，todos 是資料來源
    <input 
      type="checkbox"
      :value="item" 
    >
    {{item}}
  </label>
</div>
```

讓資料由 Vue 去做管理，新增一個 todos，讓 `v-for` 從裡面擷取資料：

```
todos: ['洗車', '洗衣服', '玩森友會']
```

### 已完成事項

前面做完待辦事項的資料，接著要做出如果打勾 checkbox 後，讓已勾選的資料可以渲染到已完成的空陣列。

首先做一個空陣列：

```
doneList: [],
```

然後要在待辦事項設置一個 `v-model`，讓點選的資料即時進入：

```
<div class="showTodos">
  <h2>待辦事項：</h2>
  <label v-for="item in todos">
    <input 
      type="checkbox"
      :value="item"    // 注意這邊要使用 v-bind 綁定 value
      v-model="doneList"
    >
    {{item}}
  </label>
</div>
```

在 template 的部份：

```
<div class="showDoneList">
  <h2>已完成事項：</h2>
  <p v-if="doneList!=''">已辦完：{{ doneList }}</p>
  <p v-else>尚未完成任何事項</p>
</div>
```

## 讓輸入待辦事項透過點擊新增按鈕加入 todos 資料中
---

在 Vue 的資料中新增一個 `methods`，然後加入點擊新增資料的動作：

```
methods: {
  addNewList: function() {
    this.todos.push(this.inputNewTodo)
  }
}
```

接著在 button 加上 `@click` 綁定 addNewList：

```
<button @click="addNewList">新增</button>
```

這樣輸入新資料，然後點擊按鈕就可以將新資料帶入 todos 的陣列中了。
如果想做出輸入完資料，按 Enter 鍵也有一樣效果的話，就在輸入待辦事項的 input 欄位加入：

```
<label>輸入待辦事項：
  <input type="text" 
    name="inputNewTodo" 
    placeholder="請輸入事項"
    v-model="inputNewTodo"
    @keyup.enter="addNewList"
  >
</label>
```

## 運用 computed 將已完成事項的 array 重新組裝字串
---

雖然點選待辦事項的 checkbox 可以將待辦事項加入到 doneList 的空陣列中，而且已完成事項也可以顯示資料。
但是顯示出來的不是想要的效果，如果只想要顯示陣列中的字串而已的話，該怎麼做呢？
這裡就可以使用 `computed`，在 Vue 的資料中加入：

```
computed: {
  doneListToString: function() {
    return this.doneList.join(', ')
  }
}
```

在 template 部份，把原本已辦完 `已辦完：{{ doneList }}` 改成 computed 的函式名稱：

```
<div class="showDoneList">
  <h2>已完成事項：</h2>
  <p 
    v-if="doneList!=''"
    class="done"
  >已辦完：{{ doneListToString }}</p>
  <p v-else>尚未完成任何事項</p>
</div>
```

就可以將選取的資料以字串加上 `,` 顯示。

## 運用 filters
---

`filters` 可以將文字做成需要的格式處理，例如想要把待辦事項的各個事項前後加上 `|`。
先在 Vue 資料中加入：

```
filters: {
  doneListFormat: function(str) {
    return `| ${str} |`
  }
}
```

然後在 template 部份，在原本 todos 中跑出 item 的後面加上 `filters` 的名稱：

```
<div class="showTodos">
  <h2>待辦事項：</h2>
  <label v-for="item in todos">
  <input 
    type="checkbox"
    :value="item"
    v-model="doneList"
  >
    {{ item | doneListFormat }}
  </label>
</div>
```

## watch
---

`watch` 這個功能可以即時的監聽某個值，如果發生變動就可以做某些事情。
例如當新增待辦事項時，可以跳出 alert，並表示已新增事項：

```
watch: {
  todos: function() {
    alert('已新增事項')
  }
}
```

這樣當新增事項到 todos 中時，由於 todos 的資料變動，所以就會做出我們給的指令。
*注意不要監聽 inputNewTodo，如果每輸入一個字就會一直跳 alert*

這邊還可以做一個運用，如果輸入待辦事項是空字串的話，就會跳 alert，例如：

```
watch: {
  todos: function() {
    if(this.inputNewTodo === '') {
      alert ('請輸入文字')
    }
  }
}
```

這樣當輸入的是空字串的話，就會跳 alert 提示請輸入文字。

## 加入一些其他功能
---

### 判斷輸入文字才能新增待辦事項

接著修改一些小 bug，由於新增待辦事項，如果沒輸入文字，直接按新增或是按 Enter 都可以新增到 todos 中，所以要在 addNewList 加入判斷，需要加入文字才能夠新增。

還有加入新增事項後，將輸入欄位清空。

```
addNewList: function() {
  if(this.inputNewTodo === '') {
    alert('請輸入文字')  // 如果輸入空字串就跳提示
  } else {
    this.todos.push(this.inputNewTodo);
    this.inputNewTodo = ''  // 清空輸入欄位
  }
}
```

### 刪除所有事項

最後如果想刪除所有事項的話，我再加入一個刪除的按鈕，並在 `methods` 新增並綁定它，就完成了。

```
deleteAllTodos: function() {
  this.todos = [];
  this.doneList = []
}
```
## 後記
---

其他一些東西還有再做一些修改，有微調或是去掉，例如：`watch`。
新增事項成功就跳 alert 有點惱人，後來改成監聽如果完成，好像也不太適合，於是就關掉 `watch` 了。

其他如果有想到其他東西可以再想辦法加進去，例如 localStorage。
