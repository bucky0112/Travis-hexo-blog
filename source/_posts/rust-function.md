---
title: 優雅的寫出函式 
date: 2022-09-21 01:22:18
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的函式
---

函式在 Rust 中非常重要，從我們的第一天開始就已經展示過函式了。而且在 Rust 寫函式的感覺非常簡單俐落，宣告一個函式就可以不用打很多字，超爽的～

那麼就一起來看怎麼寫函式吧！

## 函式

在 Rust 中，至少都會有一個主函式，也就是 `main`，而要宣告一個函式也非常簡單，只需要用關鍵字 `fn` 並接上函式的名稱，並且在後面加上 `()`，這裡可以加上參數或是不加都可以，最後再加上 `{}` 就宣告了一個函式，就像這樣：

```rust
fn main() {
}
```

在一般程式中當然不會只有一個函式，所以接著來宣告另一個函式。
前面有提到 Rust 的命名慣例是使用 snack case，所以要建立一個 `do_stuff` 的函式就像這樣：

```rust
fn do_stuff() {
	println!("Hello, world!");
}
```

那麼要怎麼執行 `do_stuff()` 呢？只需要在 `main()` 中呼叫即可：

```rust
fn main() {
	do_stuff();
}

fn do_stuff() {
	println!("Hello, world!");
}
```

執行結果就可以印出 Hello, world!

## 參數

函式的好用在於可以帶入不同的參數，去執行想要的結果。
那麼要怎麼使用參數呢？
首先帶入的參數必須要定義型別，接著在呼叫函式中帶入符合型別的值即可。
假設我們想要做一個計算總價的程式，我們需要的參數有數量跟價格，就可以這樣寫：

```rust
fn do_stuff(qty: i32, price: i32) {
    let total = qty * price;
    println!("{}", total);
}
```

接著呼叫函式的時候，帶入相對應的值就可以得到我們想要的結果。

```rust
fn main() {
    do_stuff(20, 100);
}
```

最後印出來的結果就會是 2000。

## 函式回傳值

在函式中，也可以使用回傳值來操作，例如上面的例子中，我們可以選擇不要在 `do_stuff()` 印出結果，而是在 `main()` 中來處理。

首先一樣帶入參數，並且定義型別，接著比較特別的是加上一個箭頭，並宣告一個型別，然後 `return` 結果。

如果我們想改一下函式，想算出價格半價的總金額，那可以這樣做：

```rust
fn do_stuff(qty: i32, price: i32) -> i32 {
	let half_price = price / 2;
    return qty * half_price;
}
```

或者想要懶一點的話，甚至可以把 `return` 省略，然後把 `;` 拿掉：

```rust
fn do_stuff(qty: i32, price: i32) -> i32 {
    let half_price = price / 2;
    qty * half_price
}
```

接著呼叫函式，並帶入值，就可以得到結果。

```rust
fn main() {
    let result = do_stuff(20, 100);
    println!("{}", result); // 印出1000
}
```

看完今天的文章，是不是覺得 Rust 的函式很簡單呢？

明天預計會來講解一下型別的部分，相比之下函式真的不是很困難XD
但沒關係，我們慢慢從基礎打好，一步一步的體會 Rust 的樂趣吧～