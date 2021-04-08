---
title: SCSS 變數運用
tags:
  - sass
  - scss
  - variable
date: 2020-09-20 20:23:38
categories: SCSS 學習筆記
keywords: 
  - sass
  - scss
  - variable
  - darken
  - lighten
decription: SCSS 的變數運用、數值運算、字串使用、darken、lighten 的用法。
---
說來慚愧，立志成為一名前端工程師的我，只會純 CSS 跟框架來幫網頁做樣式，對於 SASS 這個好用的語言工具卻是知道的不多，所以想趁有時間來學一下 SASS，就開了這個學習筆記系列。
<!--more-->

## 何謂 SASS？
根據[維基百科中](https://zh.wikipedia.org/wiki/Sass)介紹，SASS 全名為 **S**yntactically **A**wesome **S**tyle**s**heets，是一個將指令碼解析成 CSS 的手稿語言，也就是 SassScript。SASS 有兩種語法，一種是 SASS，另一種是 SCSS，那麼兩種有什麼差別呢？

這是 SCSS 的寫法：
<iframe height="265" style="width: 100%;" scrolling="no" title="scss sample" src="https://codepen.io/bucky0112/embed/OJNBqGK?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/OJNBqGK'>scss sample</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

這是 SASS 的寫法：
<iframe height="280" style="width: 100%;" scrolling="no" title="sass sample" src="https://codepen.io/bucky0112/embed/XWdxGwe?height=280&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/XWdxGwe'>sass sample</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

跟普通的 CSS 相比，SCSS 是相對好入門的寫法，在相同架構下，例如 li 就可以寫在 ul 中，而且寫法差不多；而 SASS 看起來架構也一樣，差別在於完全沒有大括號跟分號，感覺很適合懶人的寫法。
不過我似乎很少看到有人使用 SASS，也許是我孤陋寡聞，看的東西不夠多qq。所以為了避免未來協作實務上的困難，所以我之後的實用還是會以 SCSS 為主。

## 變數運用
前面稍微簡單介紹一下 SCSS 語法之間的差異，這裡就要來講一下 SCSS 好用的地方 - **變數**。

在寫 CSS 時，有時候會遇到文字或是區塊共用同樣的顏色，例如下方例子：
<iframe height="265" style="width: 100%;" scrolling="no" title="same color" src="https://codepen.io/bucky0112/embed/JjXmVjZ?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/JjXmVjZ'>same color</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

如果遇到需要改變顏色的需求時，可能就得要一個一個更換。
不過在 SCSS 就可以運用到變數的概念，設定變數也很簡單，一個錢字符號加上變數的名稱，後面設定想要的效果，例如：

```scss
// 設定字型大小
$size-xl: 40px;
// 設定字型顏色
$font-color: red;

.header {
  font-size: $size-xl;
  color: $font-color;
}

.content {
  font-size: $size-xl;
  color: $font-color;
}

.footer {
  font-size: $size-xl;
  color: $font-color;
}
```

以上就可以一口氣更換大量的效果，如果想要再改的話，只要變更變數的值就可以了，非常靈活。

## SCSS 變數做加減乘除好方便
傳統 CSS 要做數值運算的話，得要透過 `calc()` 才能做到，不過在 SCSS 中的變數，很輕易的就可以做到跟一般的程式語言一樣的數值運算，例如：

```scss
// 設定一個基準
$font-m: 20px;
// 將基準 * 2
$font-l: $font-m * 2;

h1 {
  font-size: $font-l;
}

p {
  font-size: $font-m;
}
```

## 字串運用
變數在 SCSS 中也可以做一般程式語言字串的使用，例如：

```scss
@import url('https://fonts.googleapis.com/css2?family=Kufam&display=swap');
$font-family-base: 'Kufam', cursive;

h1 {
  font-family: $font-family-base;
}

p {
  font-family: $font-family-base;
}
```

## 透過 darken、lighten 來調顏色
darken、lighten 是 SCSS 內建的功能，可以用來微調顏色，是一個非常有趣的功能，使用方法是 `darken(orange, 10%);`，這樣就可以將 orange 的顏色調深 10%，反之 lighten 就是調淺，例如：
```scss
$bg-red: red;
$bg-green: green;

h1 {
  background-color: lighten($bg-red, 30%);
}

p {
  background-color: darken($bg-green, 10%);
}

span {
  background-color: darken(orange, 10%);
}
```
