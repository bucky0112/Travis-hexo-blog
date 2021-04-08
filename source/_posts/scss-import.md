---
title: SCSS import 切分檔案管理
tags:
  - scss
  - import
date: 2020-09-22 22:40:59
categories: SCSS 學習筆記
keywords:
  - sass
  - scss
  - import
decription: 如何使用 import 來管理 SCSS 檔案。
---
在以往單純使用 CSS 的開發經驗中，總是會使用一支 CSS 檔案來設計所有的樣式，但是這樣的缺點是之後的維護相當困難，要找一個樣式的 code 可能要找很久。之前在寫 Vue 專案的時候，覺得元件式的管理很棒，所有元件分離讓之後的管理變得容易很多，現在才發現原來 SCSS 也可以做類似的事情，感覺好用很多啊～
<!--more-->

## 變數管理
這個方法在我上次的專案中， 在我自定義 BootstrapVue 的顏色時有用到。而在 SCSS 中的作法也是差不多，假設目前有一支 .scss 檔案，已經有設定顏色的變數，想要讓變數分割成一支獨立的檔案來管理的話，做兩步驟就可以成功使用 import 來管理：

1. 新增一支 `_variable.scss`，裡面放變數的設定。
2. 在主要 .scss 檔中 import 檔案：`@import "variable";`

```scss
// all.scss
@import "variable";

.banner-title {
    max-width: 460px;
    background: lighten($danger, 20%);
    color: $white;
  }
  .main-menu {
    background: $danger;
    overflow: hidden;
    a:hover {
      background: $danger;
    }
  }
```

```scss
// _variable.scss
$danger: #ff0000;
$white: #fff;
```

## 分離式管理 SCSS
前面提到將變數分開管理，那麼如果要將元件分得更細的話也可以，例如可以分成每一頁一個 .scss 檔，或是把 navbar 或是 footer 分開放，當然如果有使用 CSS Reset 的話，也是可以使用 import 管理，例如下方的例子：

* 新增一個 `_header.scss`
* 在 `all.scss` 中 `@import “header”`

這樣子把 `all.scss` 當作一個管理其他元件的檔案，在維護管理上就更加的方便了。