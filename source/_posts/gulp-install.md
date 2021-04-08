---
title: 安裝 Gulp
tags:
  - gulp
  - install
date: 2020-10-03 16:56:38
categories: Gulp 前端自動化工具開發
keywords:
  - gulp
  - install
decription: 介紹 Gulp 及如何安裝
---
Gulp 是一種前端自動化工具，可以幫助開發者將 SASS 跟 ES6 編譯成一般瀏覽器看得懂的版本，也可以把圖片跟 .css 還有 .js 壓縮成更小的檔案，是一個很方便的開發工具，這篇文章將介紹如何安裝 Gulp 並引入專案中。
<!--more-->

## 安裝 NVM

安裝 Gulp 之前，首先你的電腦需要有 Node.js 的環境，建議使用 NVM 來安裝 Node.js，方便日後可以切換不同版本的 Node.js。

1. 使用 `cURL` 或 `Wget` 指令安裝：：

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash
```

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash
```

2. 確認是否安裝成功，輸入指令確認，成功的話會顯示 `nvm`：

```bash
command -v nvm
```

## 用 NVM 安裝 Node.js

1. 輸入指令查看 LTS 版本（長期穩定開發版本）：

```bash
nvm ls-remote --lts
```

![0EC377CE-88E1-44E7-92E3-9FB180441B8C](https://i.imgur.com/vrH4DDn.png)

2. 輸入指令安裝指定版本：

```bash
nvm install v12.18.4
```

3. 確定有無成功安裝 Node.js：`node -v`，如果有會顯示版本。
4. 確認 npm 版本：`npm -v`，如果有會顯示版本。

## 安裝 Gulp

安裝了一堆東西，接著就是安裝 Gulp 了：

1. 安裝 Gulp 在 Global 環境：

```bash
npm install --global gulp-cli
```

2. 確定有無成功安裝 Gulp：`gulp -v`，如果有會顯示版本。

恭喜你，可以開始 Gulp 專案了🎉

## 初始化專案

1. 利用指令 `cd` 到專案資料夾，輸入 `npm init`，建立一個 package.json，接著會確認一些資料，沒什麼需要修改的話就一路 Enter 到底就好。
2. 輸入 `npm install --save gulp`，建立資訊在你的 package.json 中。
3. 輸入 `gulp -v` 確認安裝完成。