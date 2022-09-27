---
title: 參考和借用
date: 2022-09-28 02:09:03
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - reference
 - borrow
decription: Rust 的參考和借用
---

上一篇提到的所有權，在變數的所有權更換時，原本的變數所有權也跟著解除。這讓 Rust 在使用變數的時候跟其他程式語言比較起來，就稍微顯得麻煩。

## 參考

不過 Rust 也提供了相對應的機制可以解決這個問題，也就是參考（reference）。直接看下方的範例：

```rust
let x = "abc".toString();

do_some(&x);
println!("{}", x);

fn do_some(s: &String) {
	println!("{}", s);
}
```

在這個例子中，我們建立了一個變數 x，型別是 `String`，然後也建立了一個函式，參數的型別也是 `String`。
看起來跟上一篇的範例差不多，不過這裡有一些小小的差異。不同之處在於參數以及型別加上了 `&` 符號，這個符號就是參考，而參考的使用就是讓開發者不必變更所有權就可以使用其變數的值。這時候運行就不會報錯，並且可以成功印出值。

雖然 `&` 可以讓開發者建立一個指向變數的參考，但是因為並沒有更換所有權，所以最終指向的變數，即使不再使用也不會被丟棄。

在 Rust 的世界中，建立參考這樣的動作也叫做借用 (borrowing)。可以想像一下，假設有人手上有某樣東西，他把東西借給你。而當你使用完之後，你就把借來的東西還給他，有借有還，再借不難。

## 借用

既然可以透過借用來解決使用權的問題，那麼如果我們想修改借用的值可不可以呢？

在知道答案之前，我們先來練習修改字串的方法，這裡要介紹的是 `push_str()` 這個方法，我們直接看範例：

```rust
let mut x = "Hello".to_string();
x.push_str(", world!");
println!("{}", x);
```

首先變數 `x` 是字串，並且設為可變，接著再透過 `push_str()` 並帶入字串，就會從 `x` 的字串後面接上新字串。

那麼依樣畫葫蘆來試試借用的值能不能這樣做：

```rust
fn main() {
    let mut x = "Hello".to_string();
    x.push_str(", world!");
    do_some(&x);
    println!("{}", x);
}

fn do_some(s: &String) {
    println!("{}", s);
}
```

執行結果也是沒問題的，但前提是要記得在變數設為可變，否則就會報錯。
而且最好在借用的參數也加上可變，就像這樣：

```rust
fn main() {
    let mut x = "Hello".to_string();
    x.push_str(", world!");
    do_some(&mut x);
    println!("{}", x);
}
fn do_some(s: &mut String) {
    println!("{}", s);
}
```

但借用也是有一些限制的，例如：

* 在同作用域中建立多個可變的借用是非法的
* 在同作用域可變的借用和不可變借用不能共存

在同作用域中，超過一個以上的多變借用是不行的，所以下面的例子會報錯：

```rust
let mut x = "Hello".to_string();
let y = &mut x;
let z = &mut x;
println!("{}, {}",y, z);
```

既然不能在同個作用域中，那麼只要分開就可以了，就像這樣把某一個放入大括號中：

```rust
let mut x = "Hello".to_string();
let y = &mut x;
println!("{}", y);
{
	let z = &mut x;
    println!("{}", z);
}
```

而可變借用跟不可變借用的錯誤例子如下：

```rust
let mut x = "Hello".to_string();
let y = &x;
let z = &mut x;
println!("{}, {}", y, z);
```

而解決的方式也不難，也是利用作用域就可以解決：

```rust
let mut x = "Hello".to_string();
let y = &x;
println!("{}", y);
let z = &mut x;
println!("{}", z);
```

像這樣分段使用變數，當使用完之後其變數因為沒有再使用，所以這樣是可以的。