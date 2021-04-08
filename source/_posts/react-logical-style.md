---
title:  React 動態加入 CSS 
tags:
  - react
  - 動態加入 css
  - 動態加入 class
date: 2020-10-08 00:49:12
categories: React 學習系列
keywords: 
  - react
  - 動態加入 css
  - 動態加入 class
decription: React 動態加入 CSS
---
前面提到如何使用邏輯運算子來判斷顯示加號減號，這篇則是介紹其他方式，也就是使用 CSS 來動態將 HTML 元素隱藏起來。
<!--more-->

## visibility 與 display 的差異

首先，要先了解 `visibility: hidden` 與 `display: none` 這兩個的運用有什麼差異。

* `visibility: hidden`：當隱藏元素時，原本的元素依然存在佈局中。
* `display: none`：當隱藏元素時，該元素會在原本的佈局移除。

根據以上條件的不同，選擇使用 `visibility: hidden` 會是不錯的作法。

## 動態加入 CSS

在前面的筆記中有提到如何加入 inline-style，這邊就再加入邏輯運算子來判斷，當 `count >= 10` 就執行  `visibility: hidden`：

```js
<div 
  className="plus"
  style={{
  	visibility: count >= 10 && "hidden"
  }}
  onClick={() => setCount(count + 1)}
>
	+
</div>
```

另外當  `count < 1` 就執行  `visibility: hidden`：

```js
<div 
  className="minus"
  style={{
  	visibility: count < 1 && "hidden"
  }}
  onClick={() => setCount(count - 1)}
>
	-
</div>
```

可以試試看 CodePen，看看效果如何：
<iframe height="265" style="width: 100%;" scrolling="no" title="React counter use css" src="https://codepen.io/bucky0112/embed/vYKYWYe?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/vYKYWYe'>React counter use css</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 動態加入 class

除了動態加入 inline-style 以外，還可以動態加入 class 來顯示樣式，作法也差不多。

1. 在 css 部份設定一個 class，並帶入樣式：

```css
.hidden {
  visibility: hidden;
}
```

2. 在加號減號部份加上動態 class 效果：

```js
<div 
  className={`plus ${count >= 10 && 'hidden'}`}
  onClick={() => setCount(count + 1)}
>
	+
</div>

<div
  className={`minus ${count < 1 && 'hidden'}`}
  onClick={() => setCount(count - 1)}
>
  -
</div>
```

呈現的效果是一樣的，可以在 CodePen 玩玩看：
<iframe height="265" style="width: 100%;" scrolling="no" title="React counter use css class" src="https://codepen.io/bucky0112/embed/MWeYYwb?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/MWeYYwb'>React counter use css class</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>