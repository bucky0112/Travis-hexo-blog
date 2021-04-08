---
title: ES6 好用語法 - 解構賦值
tags:
  - ES6
  - 解構賦值
  - destructuring assignment
date: 2020-09-21 22:12:52
categories: ES6 好用語法
keywords:
  - ES6
  - 解構賦值
  - destructuring assignment
decription: ES6 中好用語法，解構賦值的使用。
---
懶得定義一樣的值嗎？來試試複製大法吧～
<!--more-->
## 解構賦值 (Destructuring assignment)
在 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 中的介紹為 - **解構賦值**語法是一種 JavaScript 運算式，可以把陣列或物件中的資料解開擷取成為獨立變數。

## 陣列解構
一般如果想要把陣列中的資料給複製並建立一個新的變數的話，可能會這樣做：
```js
const winter_is_comming = ['December', 'January', 'February'];

const winter_1st_month = winter_is_comming[0];
const winter_2nd_month = winter_is_comming[1];
const winter_3rd_month = winter_is_comming[2];
```

如果透過解構的話，作法有點像是把右邊的值給複製進去給左邊的變數一樣，可以這樣做：
```js
const winter_is_comming = ['December', 'January', 'February'];

const [winter_1st_month, winter_2nd_month, winter_3rd_month] = winter_is_comming;
```

接著來練習一下：
```js
// 使用解構組合使 arr2 變成為 [1, 2, 3, 4, 5, 6] 的結果
const arr = [1, 2, 3];
const arr2 = [...arr, 4, 5, 6]; // arr2 = [1, 2, 3, 4, 5, 6]
// ---------------------------------------------------------
// 使用解構組合將以下兩個陣列為 [1, 2, 3, 4, 5, 6] 的結果
const nums1 = [1, 2, 3];
const nums2 = [4, 5, 6];
const nums3 = [...nums1, ...nums2]; // nums3 = [1, 2, 3, 4, 5, 6]
// ---------------------------------------------------------
// 使用解構組合將 a、b、c 三個變數的值分別設為 'angular', 'react', 'vue'
const jsFramwork = ['angular', 'react', 'vue'];
const [a, b, c] = jsFramwork; // a = 'angular'; b = 'react'; c = 'vue';
// ---------------------------------------------------------
// 請使用解構，將以下陣列分別取出為獨立變數 Hua, Xion, Ming, Mei
const people = ['小花', '大雄', '大明', '小美'];
const [Hua, Xion, Ming, Mei] = people;
console.log(Hua, Xion, Ming, Mei);
```


## 物件解構
物件的解構賦值用法也差不多，看下面例子：
```js
// 請使用解構，取出 name 及 age 的變數
const person = {
  name: '小明',
  age: 16
}
const { name, age } = person;
console.log(name, age);
```

使用的時機主要是在可能透過 api 抓取到資料的時候，如果想要拿到某些值來使用，就可以透過這個方法。

### 進階使用方法

* 取出物件中的物件
如果想拿到物件中的物件的話，也是辦得到的，例如：
```js
const person = {
	name: '小明',
  age: 16,
	skills: {
    front: 'JavaScript',
    back: 'Ruby',
	},
}

const { skills } = person;
const { front, back } = skills;
console.log(front, back); // "JavaScript", "Ruby"
```

* 物件屬性名稱縮寫
先看下方的例子：
```js
const gameName = '動物森友會';
const gameType = '模擬遊戲';
const gamePrice = 1600;

const animalCrossing = {
  gameName: gameName,
  gameType: gameType,
  gamePrice: gamePrice,
}
```

如果物件內的屬性跟變數名稱一樣的話，那就可以這樣寫：
```js
const animalCrossing = {
  gameName,
  gameType,
  gamePrice,
}

console.log(animalCrossing);
// 結果會是一樣的
```