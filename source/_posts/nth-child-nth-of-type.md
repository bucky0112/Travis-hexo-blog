---
title: 偽類選取器之 :nth-child() 和 :nth-of-type()
tags:
  - css
  - class
  - pseudo-class
  - nth-child
  - nth-of-type
date: 2020-10-22 23:22:40
categories: 眼花撩亂的 CSS 選取器 
keywords:
  - css
  - class
  - pseudo-class
  - nth-child
  - nth-of-type
decription:
---
最近都在忙著面試，有些面試官人很好，如果有不懂的地方，他們會試著教你，所以我也藉由面試學到了蠻多東西（真的謝謝面試官大大）。 像是學到了 `:nth-child()` 跟 `:nth-of-type()` 這兩個的用法，回家也試著玩了一下，蠻有趣的，之後可以運用在 side-project 看看，這篇文章先來介紹這兩者的使用方法。
<!--more-->

## 蝦米是 pseudo-class？

pseudo-class，中文是**偽類選取器**，是 class 的一種，都是由一個冒號作為開頭，例如常見的 `:hover`。

## :nth-child() 怎麼用？

不囉嗦，直接看用法

<iframe height="265" style="width: 100%;" scrolling="no" title="choose num 4" src="https://codepen.io/bucky0112/embed/zYBomwO?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/zYBomwO'>choose num 4</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

可以看到如果想要指定某個物件改變樣式的話，就可以使用 `:nth-child(n)`，數值從 1 開始算起，所以如果要選第 4 個，那就是輸入 4；如果要選擇 1、2、5 的話，就可以這樣寫：

```css
ul :nth-child(1),
ul :nth-child(2),
ul :nth-child(5) {
  color: red;
}
```



### 選取奇數或偶數的用法

如果你想把奇數欄位的 list 都改成紅色，而把偶數欄位的 list 都改成綠色的話，只要在括號中輸入關鍵字，**奇數是 odd**，而**偶數是 even**，就如同下方範例一樣：

<iframe height="265" style="width: 100%;" scrolling="no" title="choose odd and even" src="https://codepen.io/bucky0112/embed/GRqNYQO?height=265&theme-id=dark&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/GRqNYQO'>choose odd and even</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



### 間隔跳位選取的用法

如果你想嘗試類似迴圈的用法，例如每間隔 3 個就改變樣式，那麼就可以直接在括號中加入 **3n**，就可以做到每間隔 3 個就把 list 改成紅色了，例如下方例子：

<iframe height="265" style="width: 100%;" scrolling="no" title="choose custom number" src="https://codepen.io/bucky0112/embed/NWrbENV?height=265&theme-id=dark&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/NWrbENV'>choose custom number</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

它在括號內的公式是 `An + B`，A 跟 B 是可以自訂的數值，+ 是可以變更的運算符號，而 n 則是由 0 開始的正整數，所以如果是 3n 的話，所得到的結果那就會是選取的物件，例如：

```
3 x 0 = 0
3 x 1 = 3 // 第 3 個物件
3 x 2 = 6 // 第 6 個物件
3 x 3 = 9 // 第 9 個物件
```

所以也會有其他的變化式，例如想選擇 4 個裡面的第 3 個的話，那就是 4n+3，可以看下方例子：

<iframe height="265" style="width: 100%;" scrolling="no" title="choose custom number" src="https://codepen.io/bucky0112/embed/bGeBQaX?height=265&theme-id=dark&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/bGeBQaX'>choose custom number</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



## :nth-of-type() 怎麼用？

跟 `:nth-child()` 差不多的用法，差別在於是**會先將網頁中的物件先依標籤來分類，然後再做同類的順序選取**，如果要指定某個物件改變樣式的話，就可以使用 `:nth-of-type(n)`，數值從 1 開始算起，所以如果要選第 4 個，那就是輸入 4，可以看下方範例：

<iframe height="265" style="width: 100%;" scrolling="no" title="nth-of-type pick some" src="https://codepen.io/bucky0112/embed/PozWpNY?height=265&theme-id=dark&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/PozWpNY'>nth-of-type pick some</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

所以當選取了 2、4、5 的物件時，代表選取了第 2、4、5 個 `<h2>` 和 `<p>`。

### 選取奇數或偶數的用法

基本上也是跟 `:nth-child()` 差不多的用法，只要在括號中輸入關鍵字，**奇數是 odd**，而**偶數是 even**，就如同下方範例一樣：

<iframe height="265" style="width: 100%;" scrolling="no" title="nth-of-type pick odd/even" src="https://codepen.io/bucky0112/embed/LYZxWrv?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/LYZxWrv'>nth-of-type pick odd/even</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### 間隔跳位選取的用法

一樣也有類似迴圈的用法，例如每間隔 3 個就改變樣式，那麼就可以直接在括號中加入 **3n**，就可以做到每間隔 3 個就把元素改成紅色了，例如下方例子：

<iframe height="265" style="width: 100%;" scrolling="no" title="nth-of-type pick custom number" src="https://codepen.io/bucky0112/embed/jOryBdG?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/jOryBdG'>nth-of-type pick custom number</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>