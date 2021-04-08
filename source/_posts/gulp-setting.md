---
title: Gulp 設置準備
tags:
  - gulp
  - setting
date: 2020-10-03 19:42:56
categories: Gulp 前端自動化工具開發
keywords:
  - gulp
  - setting
decription: Gulp 設置準備
---
安裝 Gulp 完之後，接著可以開始基本的設置。
<!--more-->

## 開始設定並測試：

1. 在根目錄下建立一個 gulpfile.js
2. 建立一個 source 的資料夾，並在裡面新增一個 index.html
3. 在 gulpfile.js 裡面輸入：

```js
const gulp = require('gulp');

gulp.task('copyHTML', function() {
  return gulp.src('./source/**/*.html')
    .pipe(gulp.dest('./public/'))
})
// 做的動作意思是引入 gulp，並用 task 來執行函式，將 source 裡面所有 .html 複製到 public 資料夾中
```

4. 在終端機輸入指令：`gulp copyHTML`，就會在 public 資料夾中 copy 一個 index.html 出來。

## 後記

Gulp 的基本設置與概念大概就是這樣，後續會繼續學習如何使用更多套件來執行任務。