---
title: JS 基礎回顧 - 迴圈
tags:
  - 迴圈
  - javascript
  - w3HexSchool
date: 2020-05-12 16:56:29
categories: JS 基礎回顧
keywords:
- 迴圈
decription: 在 JavaScript 中迴圈的使用。
---
迴圈 (Loop) 在 JavaScript 中，當遇到需要重複做某件事時，是個非常好用的方法。而迴圈又分為幾種不同的方式，本篇將會介紹如何使用比較常見的作法。
<!--more-->

## 迴圈
---

如果一件重要的事，想要說三次的話，可能會這樣表達：

```
console.log('很重要'); // "很重要"
console.log('很重要'); // "很重要"
console.log('很重要')  // "很重要"
```

但是在 JavaScript 中，可以使用迴圈來幫助我們更省時省力地來處理這件事：

## for 迴圈
---

for 迴圈的用法：

```
var i;

for () {}
```

{ }中，要重複一直做的事情，例如：
`{console.log('很重要')}`

( )中，要從哪裏開始，到那裏結束，還有每回合會做什麼事情，例如：
`for (i = 0; i < 3; i++)`
上面那段的意思是：i 從 0 開始計算；到 3 的時候結束；i + 1
寫出來就是這樣：

```
var i
for (i = 0; i < 3; i++) {
  console.log('很重要')
}  // "很重要" "很重要" "很重要"
```

也可以這樣寫：

```
for (var i = 0; i < 3; i++) {
  console.log('很重要')
}  // "很重要" "很重要" "很重要"
```

![Image](https://i.imgur.com/BZlPcjz.png)

畫紅線的是「初始值」，用來初始化 for 迴圈中的計數器。
雖然在這裡可以用 var 來宣告變數，但要小心，這裡的變數並不是專屬 for 迴圈內的變數，變數 i 的有效範圍其實跟 for 迴圈是相同的。

綠線的部分是「執行迴圈的條件」，指的是當滿足這個條件 (結果為 true) 的時候，就會進入大括號 { } 的區塊，然後執行內部程式。

藍線的部分是，在每一次執行完大括號 { } 區塊的程式碼之後，會執行這段程式碼。

## while 迴圈
---

使用 while 迴圈做出上面的動作：

```
var i = 0;

while (i < 3){
  console.log('很重要');
  i++
}
```

![Image](https://i.imgur.com/JFFQMSs.png)

括號 () 內代表的是「執行迴圈的條件」，指的是當滿足這個條件 (結果為 true) 的時候，就會進入大括號 { } 的區塊，然後執行內部程式。

要注意的是在迴圈中，如果第三個條件 `i++` 沒寫的話，會發生什麼事呢？
第一圈 i = 0，第二圈 i 沒有 + 1，所以一樣是 0，以此類推會一直無線迴圈，所以要特別注意。

## for 的運用
---

因為 for 迴圈比較常用，所以以下的範例將會使用 for 迴圈來做示範。

###  for - 加總

下方有一個陣列，裡面紀錄著 iPhone 11 系列的價錢，如果想要使用 for 迴圈把價錢全部加總的話，該怎麼做呢？

```
var alliphone11 = [
  {
    name: 'iPhone 11',
    price: 24900
  },
  
  {
    name: 'iPhone 11 Pro',
    price: 35900
  },
  
  {
    name: 'iPhone 11 ProMax',
    price: 39900
  }
]
```

可以設一個變數，值給數字 0，讓它使用 for 迴圈去跑加總：

```
var totalPrice = 0;
var alliphonelength = alliphone11.length;

for(var i=0; i<alliphonelength; i++) {
  totalPrice += alliphone11[i].price
}

console.log(totalPrice)  //  100700
```

最後 3 次的加總得到 100700。

### for 加上判斷式的運用

for 迴圈也可以加入 if 來判斷，例如想找出價格超過 3 萬的 iPhone 的話，就可以這麼做：

```
var alliphonelength = alliphone11.length;

for(var i=0; i<alliphonelength; i++) {
  if(alliphone11[i].price > 30000) {
    console.log(alliphone11[i].name + '的價格超過 3 萬')
  }
}

// "iPhone 11 Pro的價格超過 3 萬"
// "iPhone 11 ProMax的價格超過 3 萬"
```

### break 與 continue

如果在迴圈中，想要提早離開或是跳過其中幾項的話，這時候就可以使用 break 或是 continue。

來看看最近熱門的動物森友會範例，今天如果身上一堆大頭菜想賣的話，可以去 [在線等! 動森揪團工具](https://ac-room.cc/) ~~我賣菜都來這裏~~，島主都會打上目前菜價多少，要多少張機票一趟。

```
var turnipExchange = [
  {
    name: '莫妮卡',
    sell: 540,
    tripTicket: 2 
  },
  
  {
    name: '小潤',
    sell: 640,
    tripTicket: 4 
  },
  
  {
    name: '傑克',
    sell: 600,
    tripTicket: 3 
  },
  
  {
    name: '阿保',
    sell: 560,
    tripTicket: 2
  }
]
```

那麼我們可以來設條件，如果找到符合菜價 550 以上，機票 3 張就可以賣，就不用繼續找下去了。

```
var teLength = turnipExchange.length;

for (var i=0; i<teLength; i++) {
  if(turnipExchange[i].sell >= 550 && turnipExchange[i].tripTicket <=3) {
    console.log('我要跟' + turnipExchange[i].name + '賣菜')
    break;
  }
}  // "我要跟傑克賣菜"
```

跑完的結果是 - 我要跟傑克賣菜。
如果沒有加上 `break` 會怎麼樣呢？
如果沒有加上 `break` 的條件，for 迴圈會一直跑，再繼續找下一個符合條件的並印出來，會是這樣：

"我要跟傑克賣菜"
"我要跟阿保賣菜"

~~除非你有很多大頭菜啦，不然找一個島主來賣就夠了。~~

---

而 break 跟 continue 兩者的功能差別：

* break 會直接跳離迴圈。
* continue 會跳過一次，然後繼續下一次迴圈。

---

看看 continue 的用法：

假設想要出島找居民，以下是我們找的居民：

```
var villagers = [
  {
    species: 'cat',
    name: '艷后'
  },
  
  {
    species: 'deer',
    name: '彼得'
  },
  
  {
    species: 'koala',
    name: '簡培拉'
  },
  
  {
    species: 'cat',
    name: '莎莎'
  },
  
  {
    species: 'chicken',
    name: '烏骨雞'
  }
]
```

我們鎖定的對象是貓，所以只要考慮貓就好，其他動物就不考慮了，所以可以這麼做：

```
var length = villagers.length;

for (var i=0; i<length; i++) {
  if(villagers[i].species !== 'cat') {
    continue;
  }
    console.log(villagers[i].name);
}
```

最後得到的是艷后跟莎莎。