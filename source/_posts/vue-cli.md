---
title: 如何使用 Vue CLI 建置專案
tags:
  - vue
  - vue cli
  - node.js
  - w3HexSchool
date: 2020-05-18 22:01:47
categories: vue
keywords:
  - vue cli
  - node.js
decription: Vue CLI 介紹與環境建置，以及建置專案。
---
Vue CLI 介紹與環境建置，以及建置專案。
<!--more-->

## Vue CLI 是什麼？
---

之前所用到的 Vue.js 都是使用 CDN 載入的模式來開發，而 Vue CLI 有什麼不同呢？
它有以下幾點特色：

1. 基於 Webpack 所建置的開發工具。
2. 便於使用各種第三方套件 (Bootstrap, Vue Router...)。
3. 可運行 Sass、Bebal 等編譯工具。
4. 便於開發 SPA 的網頁工具。
5. 簡單設定，就能搭建開發常用環境。

缺點：

* 不便開發非 SPA 的網頁（改用 CDN 模式開發）。

了解以上 Vue CLI 的優缺點後，就可以來試著安裝了，但是首先首要條件要先安裝 Node.js。

## 安裝 nvm (Node Version Manager)

由於 JavaScript 只能在瀏覽器中運行，所以為了要讓 Vue CLI 能夠在電腦本地端運行，就需要 Node.js，所以先來安裝 Node.js。
而安裝 Node.js 最推薦的是採用 nvm 的方法，這樣之後可以方便切換不同版本。

* 在終端機執行指令安裝：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

安裝完後執行 `nvm --version` ，如果成功的話，就會顯示版本。
因為 nvm 可以控管 Node.js 的版本，所以可以執行指令觀看版本：

```
nvm ls
```

## 安裝 Node.js
---

![顯示目前版本跟可以安裝的版本](https://i.imgur.com/slUd39w.png)

如果沒有要安裝特定版本的話，直接安裝最新穩定版本的 Node.js：

```
nvm install stable
```

安裝完後，執行指令，確定是否安裝成功：

```
node -v
```

## 安裝 Vue CLI
---

nvm 及 Node.js 都沒問題後，接著就使用 Node.js 的 npm 繼續安裝 Vue CLI。

```
npm install -g @vue/cli
```

安裝完後，檢查是否安裝成功，執行：

```
vue -V
```

成功的話，會顯示版本，目前最新版本為 4.3.1。

## 使用 Vue CLI
---

當安裝成功後，可以輸入 `vue`，會秀出可以輸入的指令。

![Image](https://i.imgur.com/qHXcEIX.png)

### 建製專案

因為接下來要建置專案，所以先 cd 到想建置的資料夾中，然後執行 `vue create <project name>`

### 專案設定

建置並命名完專案後，就要接著設定，會有兩個選項可以選，分別有：

* default - 安裝基本套件。
* Manually - 按照需求選擇所需套件。（這邊選擇 Manually）

![Image](https://i.imgur.com/IJZucYW.png)

選擇 Manually 後，就可以選擇想要裝的套件，接著就開始安裝了。

![Image](https://i.imgur.com/M2Q71Y0.png)

安裝一段時間後，出現以下的畫面，就代表安裝成功了。

![Image](https://i.imgur.com/9Wu4bru.png)

接著照著它的指示，cd 到該資料夾中，執行 `npm run serve` 後，連到它提供的 localhost 網址，在瀏覽器看到下面的畫面，就代表專案建置成功了。

![Image](https://i.imgur.com/kfUEdi2.png)