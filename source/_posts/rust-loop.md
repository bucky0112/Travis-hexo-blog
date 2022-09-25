---
title: 重複的事就讓迴圈來吧
date: 2022-09-26 00:56:53
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的迴圈...
---

當要處理比較重複的事情時，我們總不可能一直重複寫一樣的程式碼，這時候我們就可以透過一個迴圈來幫我們處理這件事情。而 Rust 中的迴圈主要分為三種，分別是：

- loop
- while
- for

## loop

loop 這個迴圈是一個無限迴圈，也就是說，只要沒有遇到 break 或 return，這個迴圈就會一直執行下去。

一個最簡單的 loop 迴圈如下：

```rust
fn main() {
    loop {
        println!("Hello, world!");
        break; // 如果沒有加上 break，這個迴圈就會一直執行下去
    }
}
```
但 loop 如果只單純這樣寫的話，好像不太實用。所以我們就可以利用昨天學到的 `if...else` 加上判斷的條件，來自己決定什麼時候該停止這個迴圈。

如果我們想要用一個迴圈，來處理從 1 數到 10，那麼就可以這樣寫：

```rust
fn main() {
    let mut count = 1;
    loop {
        println!("count: {}", count);
        count += 1;
        if count > 10 {
            break;
        }
    }
}
```

先宣告一個可變數 count，並且設定初始值為 1，然後透過一個無限迴圈，來處理數字的問題，每次迴圈都會將 count 加 1，並且判斷 count 是否大於 10，如果大於 10，就會跳出迴圈。

## while

while 迴圈本身就是一個條件迴圈，也就是說，但是跟 loop 迴圈給予條件判斷的方式不一樣。while 迴圈只要我們給他的條件成立的話，這個迴圈就會一直執行下去。

也就是說，while 迴圈的條件判斷是 false 的時候，才會跳出迴圈，而 loop 迴圈的條件判斷是 true 的時候，才會跳出迴圈。

```rust

我們一樣用上面從 1 數到 10 的範例，然後用 while 迴圈來寫寫看：

```rust
fn main() {
    let mut count = 1;
    while count <= 10 {
        println!("count: {}", count);
        count += 1;
    }
}
```

while 迴圈的寫法和上面的 loop 寫法很像，只是把 `if` 改成了 `while`，然後把 `break` 移除掉。

## for

for 迴圈是一個迭代迴圈，也就是說，我們可以透過 for 迴圈來處理一個集合中的每一個元素。在處理資料的時候，一般還是使用 for 迴圈來處理還是比較常見的。

如果我們想要處理一個陣列中的每一個元素，例如一個一個印出來，那麼就可以這樣寫：

```rust
fn main() {
    let arr = [1, 2, 3, 4, 5];
    for i in arr.iter() {
        println!("i: {}", i);
    }
}
```

我們首先宣告一個陣列 arr，然後透過 for 迴圈來處理這個陣列中的每一個元素，並且將元素的值賦值給 i，然後印出 i 的值。`iter()` 是一個方法，可以幫助我們將陣列轉換成一個迭代器，然後我們就可以透過 for 迴圈來處理這個迭代器中的每一個元素。

又或者如果我們想要處理一個字串中的每一個字元，那麼就可以這樣寫：

```rust
fn main() {
    let s = "Hello, world!";
    for c in s.chars() { // chars() 也是一個方法，可以幫助我們將字串轉換成一個迭代器
        println!("c: {}", c);
    }
}
```

如果我們想要跟前面的 loop 還有 while 一樣處理一個數字的範圍，例如從 1 到 10，那麼就可以這樣寫：

```rust
fn main() {
    for i in 1..11 {
        println!("i: {}", i);
    }
}
```

這裡的 `..` 是一個範圍運算子，可以幫助我們產生一個範圍，然後我們就可以透過 for 迴圈來處理這個範圍中的每一個元素。

或者可以透過 `..=` 來產生一個包含最後一個元素的範圍，這樣閱讀起來會更加直觀：

```rust
fn main() {
    for i in 1..=10 {
        println!("i: {}", i);
    }
}
```

如果要倒著數的話，也可以透過 `rev()` 來倒著數：

```rust
fn main() {
    for i in (1..=10).rev() {
        println!("i: {}", i);
    }
}
```

以上就是迴圈的基本介紹與一些應用，大家可以試著練習看看，又可以更加偷懶（？）的應用 Rust 了。(▰˘◡˘▰)