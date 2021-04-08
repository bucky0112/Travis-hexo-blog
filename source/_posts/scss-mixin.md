---
title: 學會 Mixin，讓你 SCSS 開發更快速
tags:
  - scss
  - mixin
date: 2020-09-24 22:49:10
categories: SCSS 學習筆記
keywords:
  - scss
  - mixin
decription: 如何使用 Mixin 來重複使用 CSS。
---
以往自己在寫 CSS 時，多少會遇到想要寫某一個效果卻忘記怎麼開始，然後就需要上網找怎麼解決問題。當下雖然問題解決了，不過可能過沒多久，當遇到同樣問題時，又需要再去找解法，這時候 **Mixin** 就派上用場了。
<!--more-->

## 如何開始用 Mixin

### 基本用法

Mixin 的用法我覺得有點像變數，只是差在變數只能給一個值，而 Mixin 則是把一個物件定義成一個變數，然後要用的時候再呼叫變數名稱就可以使用裡面的方法，就像下面的 CodePen：
<iframe height="265" style="width: 100%;" scrolling="no" title="mixin demo" src="https://codepen.io/bucky0112/embed/NWNowvB?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/NWNowvB'>mixin demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

```scss
// 定義一個 @mixin，並給它一個名稱
@mixin hide-text {
	text-indent: 110%;
	white-space: nowrap;
	overflow: hidden;
}

// 要使用時，呼叫 @include 加上 mixin 名稱
.h1 {
	@include hide-text
}
```

### 帶入參數的用法
這是比較進階的用法，用法跟 JavaScript 的函式蠻像的，可以帶入複數個參數，非常方便，寫 CSS 變的更像在寫一般程式語言了。

```scss
// 下面有重複使用的部份
.head {
  font-size: 25px;
  line-height: 2em;
  color: #ff0999;
  margin: 15px;
}

.main {
  font-size: 25px;
  line-height: 2em;
  color: #ff0999;
  margin: 15px;
}

.footer {
  font-size: 25px;
  line-height: 2em;
  color: #ff0999;
  margin: 15px;
}
```

```scss
// 可以帶入參數來更靈活使用
@mixin p-style($p-size, $p-color) {
  font-size: $p-size;
  line-height: 2em;
  color: $p-color;
  margin: 15px;
}

.head {
  @include p-style(20px, #677962);
}

.main {
  @include p-style(30px, #933962);
}

.footer {
  @include p-style(10px, #946809);
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="mixin demo2" src="https://codepen.io/bucky0112/embed/gOrqooE?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/gOrqooE'>mixin demo2</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>