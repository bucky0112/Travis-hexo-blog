---
title: React v.s Vue 之 Component 比較心得
tags:
  - react
  - vue
date: 2020-09-30 15:59:50
categories: 不那麼重要的
keywords:
  - react
  - vue
decription: 比較 React 跟 Vue 的心得
---
目前學 React 學了一點皮毛，而 Vue 也使用了一陣子，想寫一篇來比較一下兩者對於我這個小碼農有什麼不一樣。
<!--more-->

## Vue 的 Component 

我用 Vue 寫了一個簡單的 counter，預設數字是 0，當點擊 + 時，數字增加 1；而點擊 — 時，數字減 1，數字等於 0 時，就不能減，並且用 3 個 component 放在一起。

直接看完成品：
<iframe height="265" style="width: 100%;" scrolling="no" title="Vue Counter Demo" src="https://codepen.io/bucky0112/embed/MWydQgY?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/MWydQgY'>Vue Counter Demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## React 的 Component

React 的部份也是一樣的功能，一樣也是放 3 個 component。

直接玩玩看：
<iframe height="265" style="width: 100%;" scrolling="no" title="React count demo" src="https://codepen.io/bucky0112/embed/KKzLZJL?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/bucky0112/pen/KKzLZJL'>React count demo</a> by Bucky Chu
  (<a href='https://codepen.io/bucky0112'>@bucky0112</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 兩者比較

先說這只是我自己的心得，可能有某些個人喜好影響。

* Vue：
我覺得 Vue 的寫法還滿直觀的，而且 HTML、CSS 跟 JavaScript 三者分離的寫法滿乾淨，可能是我先學 Vue，所以一些語法的部份已經滿習慣的，寫出來也蠻快的，我個人是蠻喜歡的。
真要說缺點的話，可能在 CodePen 有些限制，所以 CSS 的部份要另外寫，沒辦法直接像在 Vue CLI 裡寫在 .vue 中。如果要寫在 template 裡面也是可以，寫成 inline-style 也是一個方法，不過當樣式一多的話，就會看起來很亂，我自己是不太喜歡。

* React：
什麼都可以寫進 JavaScript 裡面還蠻爽的，這點跟 Vue 差非常多，連 CSS 樣式都可以寫進 React 裡，如果要 code review 的話就蠻快的，不用分開來看（也有可能我寫的功能不多）。語法部份目前因為還不熟，所以可能就不像 Vue 那樣直觀，不過這部分還需要時間驗證。
缺點的部份，因為我還沒學得很深，只會使用 `useState` 而已，所以目前覺得好像沒有什麼不方便的地方。

## 總結

好像寫了一篇廢文，不過還是想紀錄一下目前的心得。總之 Vue 還是我目前的主力開發工具，然後 React 純粹是個人興趣在學習，未來我希望還是能把技能繼續點在這兩項上面，然後可能看工作需求再做決定，這兩個框架我都蠻喜歡的，就繼續加強觀念，多加練習，希望能練成 React & Vue 雙刀流吧～