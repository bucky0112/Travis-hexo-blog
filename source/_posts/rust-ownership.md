---
title: 記憶體管理機制 - 所有權
date: 2022-09-27 01:25:10
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - ownership
decription: Rust 的所有權機制
---

所有程式語言都有自己的一套管理記憶體的方式，有些語言使用垃圾回收機制（GC），有些則是讓開發者自行分配和釋放記憶體。而 Rust 則是選擇了另一條路，記憶體是由所有權系統管理，並且在編譯期會檢查是否有違反規則，如果有的話，程式就無法編譯。

所有權是 Rust 中非常獨特的概念，因為它讓 Rust 不用像其他語言一樣需要 GC，這樣的作法也讓 Rust 的效能更好。

## 所有權

所有權主要有三個規則：

- 每個值都有變數作為一個擁有者。
- 值只有一個擁有者，任何變數都不能共有擁有權。
- 當擁有者超出作用域時，該值會立刻刪除。

我們直接來看範例：

```rust
let x = "Rust".to_string();
let y = x;
println!("{}", x);
```

首先，我們宣告了一個變數 `x`，並且將 "Rust" 轉成 String 類型，並且將它賦值給 `x`。

接著，我們宣告了一個變數 `y`，並且將 `x` 賦值給 `y`。

這時候，`x` 和 `y` 都是 String 類型，並且都擁有了 "Rust"。

接著，我們將 `x` 印出來，這時候會發生錯誤，因為違反了規則之一，也就是值不能共用。

這裡實際上是 `x` 已經不再擁有 "Rust" 了，而是 `y` 擁有。

## 移動

在上面的例子中，當變數被賦值給另一個變數時，而原本的變數就不再擁有值了，這個動作稱之為移動（move）。

## 複製

但如果我們想要 `x` 跟 `y` 都分別有值的話，可以怎麼做呢？

那麼我們就可以複製一個值，而不是移動，所以我們可以使用 `clone()` 這個方法：

```rust
let x = "Rust".to_string();
let y = x.clone();
println!("{}", x, y);
// Rust Rust
```

既然一般型別的情況下可以使用 `clone()` 來處理，那麼如果是比較複雜的情況的話也可以嗎？例如函式的案例：

```rust
fn main() {
    let x = "Hello".to_string();
    say_something(x);
    println!("{}", x);
}

fn say_something(msg: String) {
    println!("{}", msg);
}
```

這時候，我們依然會發現程式無法編譯，由於我們是將 `x` 移動給了 `say_something()`，而 `x` 已經不再擁有值了，所以這時候就會發生錯誤。

雖然我們一樣可以使用 `clone()` 來複製一個值：

```rust
fn main() {
    let x = "Hello".to_string();
    say_something(x.clone());
    println!("{}", x);
}

fn say_something(msg: String) {
    println!("{}", msg);
}
```

其實 Rust 還有其他的方法可以處理這個問題，那就是參考（reference）跟借用（borrowing）的概念。

那麼我們明天再來繼續談談這兩個概念。