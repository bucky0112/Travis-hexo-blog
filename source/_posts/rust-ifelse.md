---
title: 控制流程 if…else
date: 2022-09-25 01:28:41
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的 如果可以...
---

不知不覺中我們也一起學到了第 10 天，已經達成三分之一了。(ง๑ •̀_•́)ง

今天來介紹的是 Rust 的條件判斷，相信有學過其他程式語言的人應該都知道，條件判斷是用來判斷程式執行的流程，而在 Rust 中，條件判斷的語法跟其他語言也差不多，但還是有一些地方需要了解一下。

## if...else

在 Rust 中寫 `if…else` 非常簡單，不需要在條件加上 `()`，因為在 `if` 和 `{` 中的範圍都是代表條件，這也就表示在這範圍內寫的條件必須是 `true` 或 `false`，而且在大多數情況下並不需要加入型別。

以下是一個簡單的範例：

```rust
fn main() {
    let result = print_result(20, 10);
    println!("{}", result);
    // ?
}

fn print_result(x: i32, y: i32) -> char {
    let msg;
    if x > y {
        msg = '?';
    } else {
        msg = '?';
    };
    return msg;
}
```

在上面的範例中，我們定義了一個 `print_result()` 函式，並且在裡面定義了一個 `msg` 變數，接著在 `if…else` 中判斷 `x` 和 `y` 的大小，如果 `x` 大於 `y`，則將 `msg` 設定為 `?`，否則就設定為 `?`。

然後建立一個變數 `result` 來接收 `print_result()` 函式的回傳值，最後將 `result` 的值印出來。

`print_result()` 的參數 `x` 和 `y` 分別帶入 `20` 和 `10`，所以 `x` 大於 `y`，所以 `msg` 的值會是 `?`，最後印出來的結果就是 `?`。

## else if 多個條件判斷

有時候我們可能會需要多個條件判斷，這時候就可以使用 `else if` 來判斷，以下是一個範例：

```rust
fn main() {
    let result = print_result(20, 20);
    println!("{}", result);
    // =
}

fn print_result(x: i32, y: i32) -> char {
    let msg;
    if x > y {
        msg = '?';
    } else if x < y {
        msg = '?';
    } else {
        msg = '=';
    };
    return msg;
}
```

在這個範例中，我們多加了一個條件判斷，如果 `x` 小於 `y`，則將 `msg` 設定為 `?`，否則就設定為 `=`。
由於 `x` 和 `y` 的值都是 `20`，所以 `x` 等於 `y`，所以 `msg` 的值會是 `=`，最後印出來的結果就是 `=`。

## 特殊寫法

前面都有提到說在 Rust 中，每次一段結尾都要加上 `;`，但在 `if…else` 中，如果只有一行程式碼，則可以省略 `;`，以下是一個範例：

```rust
fn main() {
    let result = print_result(20, 10);
    println!("{}", result);
    // ?
}

fn print_result(x: i32, y: i32) -> char {
    let msg;
    if x > y {
        msg = '?'
    } else {
        msg = '?'
    };
    return msg;
}
```

甚至我們可以直接寫表達式，來將變數直接指向 `if…else` 的回傳值，就像這樣：

```rust
fn main() {
    let result = print_result(20, 10);
    println!("{}", result);
    // ?
}

fn print_result(x: i32, y: i32) -> char {
    let msg = if x > y { '?' } else { '?' };
    return msg;
}
```

以上就是 Rust 中 `if…else` 的基本用法，是不是很簡單呢？