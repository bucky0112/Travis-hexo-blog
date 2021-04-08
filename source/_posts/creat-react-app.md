---
title: 用 Create React App 來建立專案吧～
tags:
  - create react app
  - react
  - cli
date: 2020-10-16 23:49:13
categories: React 學習系列
keywords:
  - create react app
  - react
  - cli
decription: 使用 Terminal 指令來快速建立 React 專案。
---
在逛 GitHub 看別人的 React 專案時，似乎都是使用 Create React App 來開發居多，比較少見使用 CDN 的模式，所以本篇文章將會講解如何入門使用 Create React App 來建立一個專案，並會簡單跟 Vue CLI 做一下比較。
<!--more-->

## 什麼是 Create React App？

Create React App (CRA) 是一款由 React 官方開發的 CLI 工具，可以讓開發者不用再擔心建置環境，可以專心地寫 Code 就好。

## 開始專案

首先開始之前，要先確認一下電腦中是否有 Node.js 跟 NVM，還沒安裝過可以看一下我在 [Vue CLI 的教學](https://bucky0112.github.io/bucky0112.github.io/2020/05/18/vue-cli/)。

都搞定之後，就可以來使用 Create React App 了。

1. 在終端機輸入指令，在 `create-react-app` 後面的就是要建立的專案名稱，接著就會開始建置專案：

   ```bash
   $ npx create-react-app first-app
   ```

2. 完成之後就 cd 到專案資料夾中，並執行指令啟動 React 專案：

   ```bash
   $ cd first-app
   $ npm start
   ```

   沒問題的話，就會自動開啟瀏覽器，看到⬇️這個畫面就代表成功運行了。

![image-20201017153934560](https://i.imgur.com/ly9E5LQ.png)

3. 如果要停止伺服器運作的話，在終端機按下 Ctr + C 就可以結束了。

## 檢視專案架構

如果有使用過 Vue CLI 來開發專案過的話，應該就會對 CRA 的專案結構不陌生，整體感覺蠻像的，看看⬇️圖：

![image-20201017160155674](https://i.imgur.com/3qM8kPN.png)

一樣主要編輯的地方都是在 **src** 這個資料夾內，介紹幾個主要的檔案：

1. **index.js** 就是管理各種套件的檔案，並透過 `ReactDOM.render` 把 React 掛載到 `#root` 上。

   ![image-20201017161127890](https://i.imgur.com/jt08QBy.png)

2. **App.js** 則是最常編輯的檔案，把它想成是一個 Component，主要編輯區在 `function App()` 中，就是會 render 到畫面的 JSX 函式。

   ![image-20201017161502743](https://i.imgur.com/XykxOT0.png)

3. **App.css** 則是我們編輯 CSS 樣式的地方，這部分倒是跟 Vue CLI 不太一樣，我以為會把所有東西都寫在 App.js 中，看來好像不是全部都在寫 JavaScript，不過這樣子區分也不錯，看起來比較好整理。

   ![image-20201017162132158](https://i.imgur.com/s1ZZygj.png)

