---
title: JS 基礎回顧 - 物件與陣列
tags:
  - 物件
  - 陣列
  - javascript
  - w3HexSchool
date: 2020-05-01 14:16:34
categories: JS 基礎回顧
keywords: 
  - 陣列
  - array
  - 物件
  - object
decription: 關於物件與陣列的介紹
---
在 JavaScript 中，物件與陣列這兩個還滿常搭配使用的，所以這篇就混在一起講吧。~~絕對不是為了要省事。~~
<!--more-->
## 陣列 Array
---

![](https://i.imgur.com/IIHLVfs.jpg)

陣列的概念有點像放藥的盒子，一個蘿蔔一個坑。
可以是零到多數元素的集合，可以放入資料，例如數字、文字，或是陣列、物件、函式，沒有規定只能放什麼進去。

### 如何建立陣列

可以是空陣列：

```
var a = []
```

如果要建立資料：

```
var a = [1, 2, 3, 'aaa', 'bbb']
```

或是：

```
var a = [];

a[0] = 123;
a[1] = 456;
a[2] = 'abc'
```

要注意的是，陣列是有順序的集合，第一筆資料是從 0 開始。

### 取得陣列的長度

`length`，陣列的長度，等於陣列元素的個數，例如：

```
var list = [1, 2, 3, 'aa', 'bb', 123];

console.log(list.length);   // 會得到6
```

如果想取用特定某一個元素，假設取第一個：

```
var list = [1, 2, 3, 'aa', 'bb', 123];

console.log(list[0]);  // 會印出1
console.log(list[1]);  // 會印出2
console.log(list[2]);  // 會印出3
console.log(list[3]);  // 會印出"aa"
console.log(list[4]);  // 會印出"bb"
console.log(list[5]);  // 會印出123
```

### 陣列的操作

如果想加入新的元素，可以使用 `push` 或是 `unshift`

```
var heroes = ['蝙蝠俠', '超人', '閃電俠', '水行俠'];

heroes.push('蜘蛛人');      // 從後面新增
heroes.unshift('美國隊長'); // 從前面新增


console.log(heroes[0]);   // '蜘蛛人'
console.log(heroes[5]);   // '美國隊長'
```

想移除元素的話，可以使用 `shift` 或是 `pop`

```
var heroes = ['蝙蝠俠', '超人', '閃電俠', '水行俠'];

heroes.pop();         // 從後面開始移除
console.log(heroes);  // ["蝙蝠俠", "超人", "閃電俠"]


var badguys = ['小丑', '雷克斯', '企鵝', '班恩'];

badguys.shift();      // 從前面開始移除
console.log(badguys)  // ['雷克斯', '企鵝', '班恩']
```

如果想檢查某個物件有沒有在陣列裡，可以使用 `indexOf` 這個語法，例如：

```
var heroes = ['蝙蝠俠', '超人', '閃電俠', '水行俠'];

// 想查找閃電俠
console.log(heroes.indexOf('閃電俠'))  // 2
// 想查找蝙蝠俠
console.log(heroes.indexOf('蝙蝠俠'))  // 0
// 想查找小丑？
console.log(heroes.indexOf('小丑'))   // -1，因為陣列中沒資料，所以比 0 小，是 -1
```

## 物件
---

在 [JS 基礎回顧 - 變數與資料型別](https://bucky0112.github.io/bucky0112.github.io/2020/04/10/JS%E5%9F%BA%E7%A4%8E%E5%9B%9E%E9%A1%A7-%E8%AE%8A%E6%95%B8%E8%88%87%E8%B3%87%E6%96%99%E5%9E%8B%E6%85%8B/) 有提到變數值的型別分成基本型別跟物件型別。而基本上在 JavaScript 的世界中，除了基本型別以外，絕大部分都可以歸類為物件型別。

那麼物件是什麼呢？它是一種元素的集合，是 key 跟 value 的組合。

### 如何建立物件

那麼該如何建立一個物件？
最基本的方法，宣告一個變數，加 = 再加 {}，就是一個基本的物件了。

```
var batman = {

}
```

如果試著用 `console.log(batman)` 的話，會顯示是 object。

下面是一個標準的物件：

```
var batman = {
  name: '蝙蝠俠',
  skills: ['有錢', '潛行', '格鬥技'],
  age: 35 
}
```

### 物件新增屬性

如果要在原本的物件增加屬性的話，可以試著用以下的方法：

1. `變數名稱.新增屬性 = 值`，例如：`batman.tools = '蝙蝠車'` 
2. 使用前面陣列教過的 `push`，例如：`batman.skills.push('解謎能力')`，注意使用 `push` 新增是會從陣列後面擠進去。

```
var batman = {
  name: '蝙蝠俠',
  skills: ['有錢', '潛行', '格鬥技'],
  age: 35 
}

batman.tools = '蝙蝠車';
batman.skills.push('解謎能力');

console.log(batman.tools);        // '蝙蝠車'
console.log(batman.skills)        // ["有錢", "潛行", "格鬥技", "解謎能力"]
```

### 物件存取屬性

如果要知道剛剛新增的屬性有沒有成功加入，可以試試 `.` 或是 `[]` 來存取：

```
console.log(batman.tools);        // '蝙蝠車'
console.log(batman.skills);       // ["有錢", "潛行", "格鬥技", "解謎能力"]

console.log(batman['tools']);     // '蝙蝠車'
console.log(batman['skills'])     // ["有錢", "潛行", "格鬥技", "解謎能力"]
```

### 物件刪除

如果想刪除某個屬性的話，就可以使用 `delete`，例如想刪除掉 age 的話，下面兩種方式都可以：

```
delete batman.age;
delete batman['age'];

console.log(batman)
```

所以出來的結果，就會只剩下 name 跟 skills

### 如何取得物件的屬性？

如果想取得屬性的話，可以使用 `Object.keys(obj)`，例如：

```
console.log(Object.keys(batman)) // 得到 ["name", "skills", "age"]
```

### 物件可以怎麼運用？

前面陣列提到，幾乎什麼資料都可以放。
物件也差不多，能包文字跟數字，還可以包陣列跟物件，像大腸包小腸一樣，所以可以做出以下：

```
var tanjirou = {
  name: '竈門炭治郎',
  job: '鬼殺隊士',
  skills: ['超強嗅覺', '疼妹妹', '水之呼吸'],     // 目前只看到動畫第 8 集
  finalMove: function () {
    return this.skills[2]                     // this 表示這個物件名稱 tanjirou
  }
}

console.log(tanjirou.finalMove() + ' 拾之型 生生流轉')  // "水之呼吸 拾之型 生生流轉"
```

### 多組物件放陣列

如果物件有多組資料差不多的話，像是 key 一樣，但是 value 不太一樣的，還可以放在陣列中，來看看實例的操作：

假設想要比較 iPhone 11 系列的所有手機的話，下面有 3 組物件，分別是 iPhone 11、Pro 跟 ProMax：

```
var iPhone11 = {
  name: 'iPhone 11',
  system: 'iOS 13',
  memory: '4 GB',
  storage: [64, 128, 256],
  display: 6.1,
  battery: '3110 mAh',
}
```

```
var iPhone11Pro = {
  name: 'iPhone 11 Pro',
  system: 'iOS 13',
  memory: '4 GB',
  storage: [64, 256, 512],
  display: 5.8,
  battery: '3190 mAh'
}
```

```
var iPhone11ProMax = {
  name: 'iPhone 11 Pro Max',
  system: 'iOS 13',
  memory: '4 GB',
  storage: [64, 256, 512],
  display: 6.5,
  battery: '3969 mAh'
}
```

為了方便管理，就可以把這 3 筆放入陣列中：

```
var iPhone11Series = [
  {
    name: 'iPhone 11',
    system: 'iOS 13',
    memory: '4 GB',
    storage: [64, 128, 256],
    display: 6.1,
    battery: '3110 mAh',
  },
  
  {
    name: 'iPhone 11 Pro',
    system: 'iOS 13',
    memory: '4 GB',
    storage: [64, 256, 512],
    display: 5.8,
    battery: '3190 mAh'
  },
  
  {
    name: 'iPhone 11 Pro Max',
    system: 'iOS 13',
    memory: '4 GB',
    storage: [64, 256, 512],
    display: 6.5,
    battery: '3969 mAh',
  }
]
```

如果想要顯示 3 者的螢幕尺寸：

```
var iPhone11Display = iPhone11Series[0].display;
var iPhone11ProDisplay = iPhone11Series[1].display;
var iPhone11ProMaxDisplay = iPhone11Series[2].display;

console.log(
  'iPhone 11 系列 的顯示器個別是' + 
  iPhone11Display + '吋、' + 
  iPhone11ProDisplay + '吋、' + 
  iPhone11ProMaxDisplay + '吋'
  )
```

最後得到的結果是 "iPhone 11 系列 的顯示器個別是6.1吋、5.8吋、6.5吋"