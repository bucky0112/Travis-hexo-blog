---
title: 讓 Vue 每次換頁都能在最上方的方法
tags:
  - vue
  - 開發筆記
  - scroll
  - router
date: 2020-12-27 15:45:31
categories: 開發筆記
keywords:
  - vue
  - 開發筆記
  - scroll
  - router
  - 切換頁面
  - top
decription: 讓 Vue 每次換頁都能在最上方的方法
---
在 Vue 開發的 SPA 上，每次換頁總是會有一個問題，那就是換到其他頁面，畫面總是不會在最頂端，而是在上一頁的位置，這個問題以前覺得還好，但客戶要求修正這個問題，那就來修正吧！
<!--more-->

## 問題點

以我所做的 [side project](https://bucky0112.github.io/house_of_card/#/) 來說，假設當我按下認識桌遊後，接著會跳到認識桌遊的頁面，在一般的網站理所當然的會刷新然後會顯示在最頂端並且讓使用者可以從上往下看。

![image-20201227152948092](https://i.imgur.com/aavRHAo.png)

但是在 Vue 開發的 SPA 中，則是會顯示上一個頁面的位置，那麼接著就來說說該如何解決這個問題。

![image-20201227153157046](https://i.imgur.com/9ukM2or.png)



## 解決方法

找了一下關鍵字，就找到了解法，只要使用 `afterEach` 這個 hook 就解決了。

1. 在 main.js 加入

```js
router.afterEach((to, from, next) => {
	window.scrollTo(0, 0);
});
```

這樣就結束了，好輕鬆啊～～

**參考來源**

https://blog.csdn.net/weixin_40881970/article/details/102912283