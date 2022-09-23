---
title: Rust 的字串
date: 2022-09-24 02:58:19
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的字串
---

前面雖然在 Rust 的基本型別有提到字元，但是由於字串跟字元不太一樣，而且覺得蠻有趣的，所以本篇將會專門介紹字串。

## 字串常值

字串常值的使用方式跟字元常值不一樣的地方在於，是使用 `""` 來將文字包起來，看起來就像這樣：

```rust
let msg = "Hello World!";
```

也就是說 `msg` 的值就是一個字串常值（string literial），但只要先記住只要用 `""` 包起來的就是字串常值就好了，因為接下來還有比較複雜的部分要稍微理解一下。

Rust 的字串是一種編碼成 UTF-8 的 Unicode 數值，並且一共有 6 種型別，但最主要以及最常用的有兩種，分別是：

- `&str`
- `String`

## String

首先 `String` 可以經由字串常值加上 `to_string()` 宣告，或者是使用以下方式宣告：

```rust
let x = String::from("This is a pen!");
```

字串以我自己的用途來說，比較多用在組合字串上，那這邊來介紹一下幾種作法，讓大家參考一下。

### 用 + 來組合

這應該是蠻普遍的作法，甚至在其他語言中也有，寫法如下：

```rust
let x = "Rust".to_string();
let y = "is".to_string();
let z = "awesome!".to_string();

let s = x + " " + &y + " " + &z;

println!("{}", s);
// Rust is awesome!
```

### format!

雖然用 + 來組合算是滿簡單的，但如果要組合的字很多的話，寫起來在閱讀上就不是很方便。但還好 Rust 有提供 `format!` 這個巨集來處理，寫起來的閱讀性也比較好，寫法是這樣：

```rust
let x = "Rust".to_string();
let y = "is".to_string();
let z = "awesome!".to_string();

let s = format!("{} {} {}", x, y, z);

println!("{}", s);
// Rust is awesome!
```

兩種方法都可以使用，就看你比較喜歡哪一種。

## 字串切片

`&str`(字串切片)，讀成 string slice 或是 stir。

字串切片跟 `String` 不一樣，是一種固定大小的類型，並且不可變的。而且是一種指向別的 UTF-8 擁有的文字，也就是說 「借用」別人的文字。在這個範例中，y 就是 `&str`，y 參考了 x 所擁有的文字，從 5 bytes 之後開始算，所以 y 的值為 “is a pen!”

如果要指定從第一個開始到第四個的話，可以這樣寫：

```rust
let y = &x[0..4];
```

如果是從索引零開始的話，甚至可以省略掉 0：

```rust
let y = &x[..4];
```

也可以藉由 `len()` 計算總長度來操作：

像一開始的範例，跟下面的結果是一樣的：

```rust
let x = "This is a pen!".to_string();
let len = x.len();
let y = &x[5..len];
```

如果要拿到全部的話，下面兩種寫法是一樣的：

- 第一種寫法

```rust
let len = x.len();
let y = &x[0..len];
```
- 第二種寫法

```rust
let y = &x[..];
```

Rust 的字串用途很廣泛，身為一個工程師在字串上是無可避免一定會碰到的，雖然我自己覺得是有點複雜，但其實多寫幾個範例就比較容易理解了。