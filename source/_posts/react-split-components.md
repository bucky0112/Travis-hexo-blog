---
title: 在 CRA 專案中管理 components
tags:
  - component
  - react
  - creat-react-app
date: 2020-11-23 22:54:22
categories: React 學習系列
keywords:
  - component
  - react
  - creat-react-app
decription: 在 creat-react-app 的專案中，管理各個 components。
---
在一個專案中，通常不會在一個頁面放入太多的 HTML 結構，這樣會變得難以維護。前面也有提到如何拆分成 component，本篇文章則是講解如何在透過 creat-react-app 的專案中，管理各個 components。
<!--more-->

先來看 CodeSandbox 的範例：

<iframe src="https://codesandbox.io/embed/amazing-bhaskara-znp16?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="amazing-bhaskara-znp16"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

可以看到在 App.js 中有 3 個區塊，分別是 `head`、`content`、`footer`。前面的[文章](https://bucky0112.github.io/bucky0112.github.io/2020/09/28/react-component/#more)有提到如何拆分成 component，所以就來依樣畫葫蘆。



## 分成 3 個 component 

```react
const Head = () => (
  <div className="head">
    <h1>Head</h1>
  </div>
);

const Content = () => (
  <div className="content">
    <h2>This is content.</h2>
  </div>
);

const Footer = () => (
  <div className="footer">
    <span>This is footer.</span>
  </div>
);
```

<iframe src="https://codesandbox.io/embed/vibrant-surf-2l1pc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="vibrant-surf-2l1pc"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

可以看到畫面也是一樣，並且正常運作。但是如果內容一多的話，全部都放在同一個頁面中肯定是會非常難以維護，所以在實務上就會拆分成不同的 .js 檔案，這樣就可以方便管理了。

## 檔案拆分

1. 首先在 /src 建立一個 components 的資料夾。
2. 新增 `Head.js`、`Content.js`、`Footer.js` 這三個檔案。
3. 如果有用到 JSX 的檔案，就要匯入 React 套件：

```react
import React from "react";
```

4. 將剛剛在 App.js 中建立的 component 內容一一加入個別對應的 .js 中，例如：

```react
const Content = () => (
  <div className="content">
    <h2>This is content.</h2>
  </div>
);
```

5. 記得匯出：

```react
export default Content;
```

6. 最後在 App.js 中匯入 components：

```react
import Content from "./components/Content.js";
```

這樣就可以區分成非常乾淨而且方便管理的 components 了。

<iframe src="https://codesandbox.io/embed/dazzling-keller-urjpd?autoresize=1&fontsize=14&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="dazzling-keller-urjpd"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

在區分 component 的概念上，跟 Vue CLI 的資料管理差不多，不管是命名方式還有使用方法都蠻相似的，所以使用起來也非常熟悉。