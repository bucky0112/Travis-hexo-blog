---
title: Rust 的閉包
date: 2022-10-04 00:30:51
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - closure
decription: Rust 的閉包
---

## 什麼是閉包（closure）？

Rust 的閉包是一種匿名函式，可以從其函式的作用域中捕獲數值，而且閉包是為了快速執行而設計的，也因為如此它們的性能比一般函式還要好。

閉包有以下幾個特性：

- 是一種匿名函式
- 可以保存為變數，或是作為參數傳遞給其他函式
- 可以在某處建立閉包，然後在不同的地方執行它
- 可以從定義的作用域中捕獲變數

## 如何建立閉包？

閉包的建立方式：

1. 使用 `|` 來分隔參數，`|` 後面是函式的主體
2. 使用 `{}` 來包裹函式的主體

例如：

```rust
let plus_one = |x: i32| -> i32 { x + 1 };
plus_one(1); // 2
```

建立閉包的時候，有些特殊的地方可以注意：

- 可以不用參數，直接使用 `||` 來建立一個空閉包
- 可以不用 `return` 來回傳值，直接使用 `->` 來指定回傳值的型別
- 可以不用指定型別，Rust 會自動推導型別

## 如何使用閉包？

上面的範例中，我們建立了一個閉包，並且將它存到變數 `plus_one` 中，然後呼叫它。

閉包可以像函式一樣使用，例如：

```rust
fn main() {
    let plus_one = |x: i32| -> i32 { x + 1 };
    let a = plus_one(1);
    println!("a is {}", a); // a is 2
}
```

## 如何傳遞閉包？

閉包可以像函式一樣傳遞給其他函式，例如：

```rust
fn main() {
    let plus_one = |x: i32| -> i32 { x + 1 };
    let a = do_twice(plus_one, 1);
    println!("a is {}", a); // a is 3
}

fn do_twice<F>(f: F, arg: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(arg) + f(arg)
}
```

上面的範例中，我們定義了一個函式 `do_twice`，它接受一個函式 `f` 和一個參數 `arg`，然後呼叫 `f` 兩次，並且將結果相加。

在呼叫 `do_twice` 的時候，我們傳遞了一個閉包 `plus_one`，並且傳遞了一個參數 `1`。

## 如何捕獲變數？

閉包可以從定義的作用域中捕獲變數，例如：

```rust
fn main() {
    let num = 5;
    let plus_num = |x: i32| -> i32 { x + num };
    let a = plus_num(1);
    println!("a is {}", a); // a is 6
}
```

上面的範例中，我們定義了一個變數 `num`，並且將它傳遞給一個閉包 `plus_num`，然後呼叫 `plus_num`。

在呼叫 `plus_num` 的時候，閉包會從定義的作用域中捕獲變數 `num`，並且將它加到 `x` 上。

## 如何使用 `move` 關鍵字來捕獲變數？

使用 `move` 關鍵字可以用來捕獲變數，並且將它們移動到閉包中，例如：

```rust
fn main() {
    let num = 5;
    let plus_num = move |x: i32| -> i32 { x + num };
    let a = plus_num(1);
    println!("a is {}", a); // a is 6
}
```

上面的範例中，我們定義了一個變數 `num`，並且將它傳遞給一個閉包 `plus_num`，然後呼叫 `plus_num`。

在呼叫 `plus_num` 的時候，與上面其他例子不同的是，我們使用 `move` 關鍵字來捕獲變數，並且將它加到 `x` 上。
