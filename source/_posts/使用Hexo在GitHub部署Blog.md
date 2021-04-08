---
title: 使用 Hexo 在 GitHub 部署 Blog
date: 2020-03-28 14:55:52
tags:
- hexo
- next
categories: Hexo
keywords:   
- hexo
- next
- github
- blog
decription: #文章敘述
---

本來一直不是很想用 Hexo，因為覺得很多人使用，所以有使用另一種 Hugo 來架設 Blog，但是用完一直覺得不合口味，原本打算就將就著用。

剛好看到 Hexo 最多人使用的主題 - Next，感覺還不錯，真香，就來裝裝看吧！
<!--more-->

## 前置作業
---

**安裝需求**

需要安裝：

* Node.js
* Git

> 以下方式為 Mac 使用者方法，其他作業系統請詳閱[官網](https://hexo.io/docs/index.html)

1. 安裝 Xcode

首先到 App Store 安裝 Xcode，安裝完成後，
開啟它並前往 Preferences -> Download -> Command Line Tools -> Install 安裝命令列工具。

2. 安裝 node.js

使用 NVM，或是直接用 HomeBrew 安裝：

```
$ brew install node
```

3. 安裝 Hexo：

```
$ npm install -g hexo-cli
```

4. 完成後可以輸入指令，看看有無安裝成功，成功的話會顯示版本：

```
$ hexo version
```

我的版本是：

hexo: 4.2.0
hexo-cli: 3.1.0

## 建立專案
---

在指定資料夾建立檔案：

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

完成後會建立以下資料：

![](https://i.imgur.com/1eucaEQ.png)

## 更改主題與其他設定
---

主題選用的是 [Next](https://github.com/theme-next/hexo-theme-next)，更改步驟如下：

1. 把 next 這個主題 clone 下來：

```
   $ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

2. 找到 /_config.yml，打開修改：

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next    // 改為 next
```

3. 網站設定：

```
# Site
title: 標題
subtitle: 副標題
description: 網站描述
keywords: 網站關鍵字
author: 作者名
language: zh-TW
timezone: 時區
```

其他一些細部設定就可以自己慢慢摸索。

如果要先看看網站的樣式，可以啟動本地端的 server：

```
$ hexo server
```

## 寫作
---

接下來，大概設定完就可以開始建立文章了。

指令是：

```
$ hexo new [layout] <title>
```

Layout 有 3 種，分別是：post、page(頁面)、draft(草稿)，

假設要發一篇檔名為 Hello-World 草稿的話：`hexo new draft Hello-World`

如果要將 draft 發布為 post 的話，就鍵入：

```
$ hexo publish [layout] <title>
```

## 把網站部署到 GitHub
---

既然都做的差不多了，就可以開始把網站部署到 GitHub Pages。

1. 新增名為 `<username>.github.io` 的 repo，例如：`bucky0112.github.io`

2. 安裝 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git) 這個插件：

```
$ npm install hexo-deployer-git --save
```

3. 修改 /_config.yml：

```
url: https://username.github.io
root: /repo.github.io/

deploy:
  type: git
  repo: <repository url> #新增repo的網址
  branch: master
```

4. 上傳網站，執行：

```
$ hexo deploy
```

5. 接著需要等待一些時間，讓子彈飛一會，就可以去你的網站看看成果了。

```
https://username.github.io/repo.github.io
```

## Hexo 常用指令
---

以下是之後再使用 Hexo 發佈文章時常用的指令：

```
$ hexo generate      #產生靜態檔案 / hexo g
$ hexo deploy        #部署網站 
$ hexo server        #啟動本地端伺服器 / hexo s
$ hexo new <post>    #新增文章
$ hexo clean         #清除快取檔案和已產生的靜態檔案
```