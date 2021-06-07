---
title: Tailwind 安裝 React 篇
date: 2021-06-03 23:06:07
tags:
 - tailwind
 - install
categories: Tailwind 試玩
keywords:
 - tailwind
 - install
decription: React 專案如何安裝 Tailwind
---

最近幾年火速竄紅的 CSS 框架 Tailwind 也是蠻多前端工程師開始使用，於是就有了本篇跟風仔的安裝篇XD

<!--Read More-->

## 環境版本介紹

本篇使用的是 Create-React-App 來建立環境，並且將會透過 NPM 來安裝 Tailwind。

* React 版本：17.0.2
* Node.js 版本：14.16 （官方建議版本12.13或以上）
* Tailwind 版本：2.1.4

## 安裝流程

1. 請在終端機輸入： `$ npx create-react-app 專案名稱`，安裝完後再 `$ cd` 進入專案資料夾中。

2. 接著就要開始安裝 Tailwind 了，請輸入：

    `$ npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9`

   *這時候碰到一個問題，我的終端機跳出一個訊息：`no matches found: postcss@^7`*

   *上網查了一下解法，如果跟我一樣遇到這個問題的話，請輸入：*

   `$ npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat "postcss@^7" "autoprefixer@^9"`

3. 安裝 CRACO：

   `$ npm install @craco/craco`

4. 接著請打開編輯器，並將 package.json 中的 scripts 物件中的部份替換成:

   

   ```json
   "scripts": {
       // "start": "react-scripts start",
       // "build": "react-scripts build",
       // "test": "react-scripts test",
       "start": "craco start",
       "build": "craco build",
       "test": "craco test",
       "eject": "react-scripts eject"
     },
   ```

5. 在終端機輸入： `$ touch craco.config.js `，會在專案的根目錄產生一個檔案，並在檔案中手動加上：

   

   ```js
   module.exports = {
     style: {
       postcss: {
         plugins: [
           require('tailwindcss'),
           require('autoprefixer'),
         ],
       },
     },
   }
   ```

6. 在終端機輸入：`$ npx tailwindcss-cli@latest init`，成功的話會自動生成一個 `tailwind.config.js` 在 根目錄中。

7. 然後修改一下該檔案：

   ```js
   module.exports = {
     // purge: [],  將原本的 purge 替換成以下
     purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
     darkMode: false,
     theme: {
       extend: {},
     },
     variants: {
       extend: {},
     },
     plugins: [],
   }
   ```

8. 接著打開 `/src` 目錄中的 `index.css`，並將其內容替換成：

   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

9. 最後一個步驟，請確保 `index.css` 有被引入到 `/src/index.js` 中

   ```js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import './index.css';
   import App from './App';
   import reportWebVitals from;
   ```

10. 執行 `$ npm run start`，，畫面有顯示就代表恭喜你，可以準備進入 Tailwind 的坑了XD

