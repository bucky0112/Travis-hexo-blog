---
title: rust-enums
tags:
---

## 枚舉（Enums）

枚舉是一種定義一組可能值的方法，這些值被稱為成員（variants）。枚舉的每個成員都是一個獨立的類型，並且可以是不同類型的值，包括整數、浮點數、字符串、元組、數組、結構體等。

```rust
enum PhoneSystem {
    Android,
    Ios,
    Windows,
    Linux,
}
```

像上面的例子，首先使用 `enum` 關鍵字定義了一個名為 `PhoneSystem` 的枚舉，命名規則是大寫開頭的駝峰式命名法。

前面提到枚舉的每一個成員都可以是不同類型的值，這讓枚舉在使用上非常靈活，不過還是有一些限制，比如說，枚舉的成員不能重複，也不能是空的。

```rust
enum PhoneSystem {
    Android,
    Ios,
    Windows,
    Linux,
    Android, // error: duplicate variant `Android`
}
```

```rust
enum PhoneSystem {
    // error: empty enum is not allowed
}
```

## 訪問枚舉成員

在定義了一個枚舉之後，就可以使用 `::` 來訪問枚舉的成員。
我們這裡改用新的例子：

```rust
enum MyBirthdayGift {
    TenYearsOld(String),
    TwentyYearsOld(String),
    ThirtyYearsOld(String),
}

fn main() {
    use MyBirthdayGift::*;
    let gift = ThirtyYearsOld("一棟房子".to_string());
}
```

在上面的例子中，我們定義了一個名為 `MyBirthdayGift` 的枚舉，它的成員有三個，接著我們使用 `use` 來將 `MyBirthdayGift` 的成員引入當前作用域。

然後我們定義了一個變量 `gift`，並且使用 `ThirtyYearsOld` 來初始化它。

## Option

在 Rust 中，`Option` 是一個標準庫中的枚舉，它的成員有 `Some` 和 `None`。

```rust
enum Option<T> {
    Some(T),
    None,
}
```

`Option` 用來表示一個值是否存在，如果存在，則使用 `Some` 包裹它，如果不存在，則使用 `None`。

```rust
let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i32> = None;
```

## match

枚舉可以使用 `match` 來進行匹配，這裡我們使用新的例子：

```rust
let my_wallet = Some(100);

match my_wallet {
    Some(money) => {
        println!("我有 {} 元", money);
    },
    None => {
        println!("我沒有錢QQ");
    },
}
// 我有 100 元
```

在上面的例子中，我們使用 `match` 來匹配 `my_wallet`，如果 `my_wallet` 是 `Some`，則匹配 `Some`，並且將值綁定到 `money` 上，然後執行 `println!`。

如果 `my_wallet` 是 `None`，則匹配 `None`，然後執行 `println!`。

## if let

`if let` 是一種簡化 `match` 的方法，它只匹配一個模式，如果匹配成功，則執行相應的代碼，否則不執行任何代碼。

```rust
let my_wallet = Some(100);

if let Some(money) = my_wallet {
    println!("我有 {} 元", money);
}
// 我有 100 元
```

在上面的例子中，我們使用 `if let` 來匹配 `my_wallet`，如果 `my_wallet` 是 `Some`，則匹配 `Some`，並且將值綁定到 `money` 上，然後執行 `println!`。

如果 `my_wallet` 是 `None`，則不執行任何代碼。

## Option None

在 Rust 中，`None` 是一個特殊的值，它是一個空的 `Option`，也就是說，`None` 是一個 `Option`，但是它的值是 `None`。

```rust
let x: Option<i32> = None;
```

## Option Some

在 Rust 中，`Some` 是一個特殊的值，它是一個包含了一個值的 `Option`，也就是說，`Some` 是一個 `Option`，但是它的值是 `Some`。

```rust
let mut x: Option<i32> = None;
x = Some(6);
x.is_some(); // true
x.is_none(); // false
```

上面的例子中，我們定義了一個變量 `x`，它的類型是 `Option<i32>`，然後我們將它的值設置為 `None`，然後我們將它的值設置為 `Some(6)`，然後我們使用 `is_some` 和 `is_none` 來判斷 `x` 的值。

## result

在 Rust 中，`Result` 是一個標準庫中的枚舉，它的成員有 `Ok` 和 `Err`。

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Result` 用來表示一個值是否存在，如果存在，則使用 `Ok` 包裹它，如果不存在，則使用 `Err`。

```rust
let some_number = Ok(5);
let some_string = Ok("a string");
let absent_number: Result<i32, &str> = Err("no number");
```

## unwrap

`unwrap` 是一個 `Result` 的方法，它可以將 `Result` 轉換為 `T`，如果 `Result` 是 `Ok`，則返回 `Ok` 中的值，如果 `Result` 是 `Err`，則返回 `Err` 中的值。

```rust
let my_wallet = Ok(100);

let money = my_wallet.unwrap();
println!("我有 {} 元", money);
// 我有 100 元
```

在上面的例子中，我們使用 `unwrap` 將 `my_wallet` 轉換為 `i32`，然後將值綁定到 `money` 上，然後執行 `println!`。

## expect

`expect` 是一個 `Result` 的方法，它可以將 `Result` 轉換為 `T`，如果 `Result` 是 `Ok`，則返回 `Ok` 中的值，如果 `Result` 是 `Err`，則返回 `Err` 中的值。

```rust
let my_wallet = Ok(100);

let money = my_wallet.expect("錢包裡沒有錢");
println!("我有 {} 元", money);
// 我有 100 元
```

在上面的例子中，我們使用 `expect` 將 `my_wallet` 轉換為 `i32`，然後將值綁定到 `money` 上，然後執行 `println!`。

`expect` 跟 `unwrap` 也可以搭配 `match` 來使用。

```rust
let my_wallet = Ok(100);

match my_wallet {
    Ok(money) => println!("我有 {} 元", money),
    Err(e) => println!("錢包裡沒有錢"),
}
// 我有 100 元
```

以上就是 Rust 中的枚舉，可以活用的地方非常多，大家可以多多利用。