---
title: 使用 Electron 搭配 React 建置桌面程式
date: 2022-02-24 16:42:51
tags:
  - electron
  - react
  - build
categories: Electron
keywords:
  - electron
  - react
  - build
decription: 使用 Electron 搭配 React 建立桌面程式
---

這次公司打算把專案製作的網頁打包成桌面程式，之前就已經知道有 Electron 這個工具可以用，剛好趁這個機會來紀錄一下過程。

<!--more-->

## 什麼是 Electron

[Electron](https://www.electronjs.org/) 原本是 GitHub 開發的一個開源框架，使用 Node.js (後段) 與 Chromium (前端)，可以讓開發者透過 JavaScript、HTML 和 CSS 就可以來製作跨平台的桌面 GUI 應用程式，現在開發者常用的 Visual Studio Code 就是使用 Electron 建置的。
## 簡介

本次計畫是要將我之前練習的[作品](https://github.com/bucky0112/weather-app-react)，打包成桌面程式。

## 專案初始

如果是新專案的話，可以使用 Create React App 來建立專案，並且要加上 TypeScript。

```bash
$ yarn create react-app my-app --template typescript
```

如果是像我這次要做的，在原有專案加上 TypeScript，就執行以下：

```bash
$ yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

## 安裝插件

```bash
$ yarn add cross-env electron-is-dev

$ yarn add --dev concurrently electron electron-builder wait-on
```
## 新增檔案

安裝完以上兩個步驟之後，接著在 public 資料夾中新增一個檔案，並命名為 electron.js，並貼上以下內容：

```js
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;
const path = require("path");
const isDev = require("electron-is-dev");
let mainWindow;
function createWindow() {
mainWindow = new BrowserWindow({ width: 900, height: 680 });
mainWindow.loadURL(
isDev
? "http://localhost:3000"
: `file://${path.join(__dirname, "../build/index.html")}`
);
mainWindow.on("closed", () => (mainWindow = null));
}
app.on("ready", createWindow);
app.on("window-all-closed", () => {
if (process.platform !== "darwin") {
app.quit();
}
});
app.on("activate", () => {
if (mainWindow === null) {
createWindow();
}
});
```

## package.json 新增內容

```json
"name": "程式檔案名稱",
"description": "描述你的程式",
"author": "作者名",
"build": {
"appId": "<com.your_app>"
},
"main": "public/electron.js",
"homepage": "./",
"scripts": {
	"react-start": "react-scripts start",
	"react-build": "react-scripts build",
	"react-test": "react-scripts test --env=jsdom",
	"react-eject": "react-scripts eject",
	"electron-build": "electron-builder",
	"release": "yarn react-build && electron-builder --publish=always",
	"build": "yarn react-build && yarn electron-build",
	"start": "concurrently \"cross-env BROWSER=none yarn react-start\" \"wait-on http://localhost:3000 && electron .\""
},
```

到這裡基本上就設定的差不多了，接著先來測試看看

```bash
$ yarn start
```

沒問題的話，會出現一個視窗，並且顯示內容

![Image](https://i.imgur.com/Z7cX99N.png)

![Image](https://i.imgur.com/nSuIgXm.png)

## 將內容打包成程式檔

如果以上都沒問題的話，就可以來打包成可以在電腦執行的一般程式了。

```bash
yarn build
```

成功的話，就會在資料夾 dist 中新增一個資料夾

![Image](https://i.imgur.com/WaYaoOa.png)

這裡會按照你的電腦是什麼系統來編譯成什麼檔案，舉例來說，在 Mac 編譯的最後結果就會是 dmg 檔，如果是 Windows 的話，就會是 exe 檔。

![Image](https://i.imgur.com/t7RYplX.png)

執行該檔案就會安裝檔案

![Image](https://i.imgur.com/RUYll6x.png)

如果是 Mac 的話，會在應用程式看得到剛剛安裝的檔案；如果是 Windows 的話，則會在桌面顯示檔案捷徑。

## 遇到的雷點

首先這次專案一開始就說要安裝 TypeScript，一開始曾試過沒安裝 TypeScript，後面就有遇到一些錯誤，所以在專案的一開始要記得安裝 TypeScript。