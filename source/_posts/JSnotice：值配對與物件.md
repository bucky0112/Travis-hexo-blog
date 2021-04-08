---
title: 'JS notice： 名稱/值配對與物件'
tags:
  - javascript
  - 物件
categories: JavaScript 的怪奇物語
keywords:
  - javascript
  - JavaScript 全攻略：克服 JS 的奇怪部分
  - 物件
decription: 物件在 JavaScript 的意義
---
這篇會稍微了解物件在 JavaScript 中的判別。
<!--more-->

## 名稱/值配對
---

首先，**名稱/值**的配對，代表一個名稱會對應到唯一的值，例如：

```
year = 2020
```

嗯，就這樣而已，不用想的太複雜。

## 物件
---

物件在 JavaScript 中也是名稱與值的配對組合，例如：

```
{
  dcHero: "Batman"
}
```

而這個名稱或值甚至可以是多種名稱/值的配對，例如：

```
var dcHero = {
  name: 'Batman',
  skill: ['有錢', '有高級裝備', '潛行'],
  location: {
    place: '高譚市',
    base: '韋恩豪宅'
  }
}
```

就算是這樣多層的關係，也可以看得出名稱/值的關係，不用想的太複雜。