---
title: 挑戰 Codewar - 1
tags:
  - codewar
  - w3HexSchool
date: 2020-05-07 23:03:09
categories: 挑戰 Codewar 系列
keywords:
- Opposite number
- Even or Odd
- Number-Star ladder
decription: 本篇將會說到如何答題的過程與解答。
---
這是在胡立的程式導師實驗計畫第四期中的 [Codewar 練習題](https://github.com/bucky0112/mentor-program-4th/blob/master/codewar.md)，裡面的題目都是出自 [Codewars](https://www.codewars.com/dashboard)，之後會試著去由淺入深解開裡面的題目，並在這個系列寫出我解題的過程與心得。
<!--more-->
## Opposite number
---

第一題先從難度零顆星的開始。目的是要將數值正反顛倒，正數變負數，負數變正數。
一進去就給你一個 function，讓你去思考怎麼解。

```
function opposite(number) {
  //your code here
}
```

一開始有點想太多，想說如果是負數的話，就給它一個負號，讓它變正數；如果是正數，就給負號。

所以一開始的作法我是用 `if...else`：

```
function opposite(number) {
  if(number < 0) {
    return -number
  } else {
    return -number
  }
}
```

後來想想不對，應該是不用這麼複雜，不管什麼數，給負數就對了，所以就變成：

```
function opposite(number) {
  return -number
}

opposite(10); // -10
opposite(-50) // 50
```

## Even or Odd
---

這一題是要做出判斷給的數值是奇數或是偶數。

想了一下，應該是要用判斷 `if...else` 去做，只要判斷出偶數（或是奇數），另一個就會得到結果，所以會是這樣：

```
if number % 2 === 0
return even
else return odd
```

用 JavaScript 來操作：

```
function even_or_odd(number) {
  if (number % 2 === 0) {
    return 'Even'
  } else {
    return 'Odd'
  }
}

even_or_odd(2); // 'Even'
even_or_odd(7)  // 'Odd'
```

## Number-Star ladder
---

這題從零顆星開始變成一顆星頭目了，感覺有點難，範例是如果給數值 4，會依序回傳：

```
1
1*2
1**3
1***4
```
一開始題目就幫你設定好為：

```
function pattern(n){
 var output="";
  //being coder
 return output;
}
```

這個問題卡了我一天都解不出來，只知道是組字串的作法，最後只好去看提示，後來才知道 JavaScript 有換行的語法，然後照我自己的方法做。
*補充一下，在 JavaScript 中如果想要做出換行的效果，可以使用 '\n' 這個語法*

所以作法大概是：

```
如果是 1，就直接印 1
其他數字從 2 到 n 跑迴圈
米字(starkey) 從 1 到 n 跑迴圈
組裝字串 1 + 換行 + 1 + starkey + n
```

而在 JavaScript 中的作法為：

```
function pattern(n){
  if (n <= 1) {               // 如果 n 小於 1，直接回傳 n
    return n;
  }
  var output="1";
  for (var i = 2; i <= n; i++) {
    var starKey = "";
    for (var s = 1; s < i; s++) {
      starKey += "*";
    }
    output += "\n" + 1 + starKey + i;
  }
  return output;
}

console.log(pattern(5));
// "1
//1*2
//1**3
//1***4
//1****5"
```

然後還有看到一種作法也滿好的，在 starKey 部份是改用 `Array().join()` 的方式， 
於是就學了起來，就可以改成：

```
function pattern (n) {
  if (n <= 1) {
    return n;
  }
  var output = "1";
  for (var i = 2; i <= n; i++) {
    output += "\n" + 1 + Array(i).join("*") + i;
  }
  return output;
}

console.log(pattern(5));
// "1
//1*2
//1**3
//1***4
//1****5"
```

這次解題過程還算蠻有趣的，雖然第 3 題有點難度，想到後面還有更難的題目就覺得可怕...

![](https://i.imgur.com/CQMhOEc.gif)

但是解完題目後，所得到的成就感也很讓人著迷，會想要再接再厲繼續解下去，希望能透過完成這個系列精進寫程式的想法。