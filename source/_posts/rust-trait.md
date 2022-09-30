---
title: 特徵
date: 2022-10-01 02:15:17
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - trait
decription: Rust 的特徵
---

特徵（Trait）是 Rust 的一個重要的特性，它可以讓我們在不同的型別上定義共用的行為，並且可以在不同的型別上使用相同的函式。特徵有點像是其他語言的介面（interface），目前我們可以先把它想成是一種定義共享行為的方式。

由於我在學特徵這部分的時候，覺得這個概念滿抽象的，所以我會盡可能的以不同的例子來展示，希望能讓大家更容易理解。（標題越來越懶的想XD）

## 定義特徵

要怎麼定義一個特徵呢？

其實很簡單，只要在 trait 這個關鍵字後面加上特徵的名稱，然後在大括號裡面定義特徵的行為就可以了。

```rust
trait Summary {
    fn summarize(&self) -> String;
}
```

這是一個基本的特徵，在上面的程式碼中，我們定義了一個名為 `Summary` 的特徵，裡面有一個名為 `summarize` 的函式，它會回傳一個 `String`。

再講的仔細一點，定義一個特徵就是：

*把方法的型態簽章放在一起，來定義實現某種目的所必須的一組行為。*

分別有以下幾個重點：

* 關鍵字：trait
* 只有方法的型態簽章，沒有具體實現
* 方法的型態簽章：就是方法的名稱、參數、回傳值等等。
* 特徵可以有多個方法：每個方法簽名占一行，以 `;` 結尾
* 實現該特徵的類型必須提供具體的方法實現


## 實作特徵

我們直接來看一個實際建立特徵的例子：

```rust
struct Cat {
    enemy: bool,
    life: u32,
}

trait Noisy {
    fn get_noise(&self) -> &str;
}

impl Noisy for Cat {
    fn get_noise(&self) -> &str {
        "Meow?"
    }
}

fn print_noise(item: &dyn Noisy) { // 這裡的 dyn 的意思是：動態分配
    println!("{}", item.get_noise());
}

fn main() {
    let cat = Cat {
        enemy: false,
        life: 100,
    };
    print_noise(&cat); // prints "Meow?"
}
```

在上面的範例中，首先定義了一個名為 `Noisy` 的特徵，裡面有一個名為 `get_noise` 的函式，它會回傳一個 `&str`。然後我們定義了一個名為 `Cat` 的結構，它實現了 `Noisy` 這個特徵，也就是說，`Cat` 這個結構會有一個 `get_noise` 的函式，它會回傳一個 `&str`。
最後，我們定義了一個名為 `print_noise` 的函式，它接受一個 `Noisy` 特徵的參數，然後呼叫 `get_noise` 這個函式，並且印出回傳值。

而且特徵還可以有多個方法，而且不限單一型別。例如想要呼叫多個數字，那就可以在特徵中另外加一個方法：

```rust
trait Noisy {
    fn get_noise(&self) -> &str;
    fn call_number(&self) -> u32; // 新增一個呼叫數字的方法
}
```

然後在 `Noisy` 特徵中定義一個 `call_number` 的方法，接著在 `Cat` 結構中實現：

```rust
impl Noisy for Cat {
    fn get_noise(&self) -> &str {
        "Meow?"
    }
    fn call_number(&self) -> u32 {
        20
    }
}

fn print_number(item: &dyn Noisy) {
    println!("{}", item.call_number());
}
```

最後定義一個 `print_number` 的函式，它接受一個 `Noisy` 特徵的參數，然後呼叫 `call_number` 這個函式，並且印出回傳值。
然後看是否要在 `main` 中呼叫 `print_number` 這個函式，來印出 `call_number` 的回傳值。

## 特徵的泛型

昨天我們有介紹了泛型，那麼就來看一下特徵的泛型是怎麼寫的。

```rust
trait Noisy {
    fn get_noise(&self) -> &str;
}

impl Noisy for u8 {
    fn get_noise(&self) -> &str {
        "BYTE!"
    }
}

fn main() {
    println!("{}", 5u8.get_noise()); // BYTE!
}
```

在上面的程式碼中，我們定義了一個名為 `Noisy` 的特徵，裡面有一個名為 `get_noise` 的函式，它會回傳一個 `&str`。然後我們在 `u8` 上實作了 `Noisy` 這個特徵，並且提供了 `get_noise` 的具體實現。最後我們在 `main` 函式中呼叫了 `5u8.get_noise()`，這個 `5u8` 就是 `u8` 的一個實例。

## 特徵作為參數

```rust
fn print_noise(item: &Noisy) {
    println!("{}",item.get_noise());
}

fn main() {
    let cat = Cat { enemy: false, life: 100 };
    print_noise(&cat); // prints "Meow?"
}
```

在上面的例子，首先定義了一個名為 `print_noise` 的函式，接受一個參數 `item`，並且限制 `item` 的型別必須實作 `Noisy` 這個特徵。然後在 `main` 函式中呼叫了 `print_noise`，並且傳入了一個 `Cat` 的實例。因為 `Cat` 實作了 `Noisy` 這個特徵，所以我們可以傳入 `Cat` 的實例。

## 特徵作為回傳值

```rust
fn get_noise() -> impl Noisy {
    Cat { enemy: false, life: 100 }
}

fn main() {
    let cat = get_noise();
    println!("{}",cat.get_noise()); // prints "Meow?"
}
```

在上面的程式碼中，定義一個名為 `get_noise` 的函式，它會回傳一個實作了 `Noisy` 這個特徵的型別。然後我們在 `main` 函式中呼叫了 `get_noise`，並且將回傳值儲存在 `cat` 變數中。因為 `Cat` 實作了 `Noisy` 這個特徵，所以我們可以將 `Cat` 的實例傳給 `get_noise`。

## 特徵的繼承

```rust
trait Noisy {
    fn get_noise(&self) -> &str;
}

trait NoisyAnimal: Noisy {
    fn get_name(&self) -> &str;
}

struct Cat {
    enemy: bool,
    life: u32,
}

impl Noisy for Cat {
    fn get_noise(&self) -> &str {
        "Meow?"
    }
}

impl NoisyAnimal for Cat {
    fn get_name(&self) -> &str {
        "Black Cat"
    }
}

fn main() {
    let cat = Cat { enemy: false, life: 100 };
    println!("{}", cat.get_noise()); // prints "Meow?"
    println!("{}", cat.get_name()); // prints "Black Cat"
}
```

最後這個範例，定義了一個名為 `Noisy` 的特徵，還有一個名為 `get_noise` 的方法。然後定義一個名為 `NoisyAnimal` 的特徵，它繼承了 `Noisy` 這個特徵，並且還有一個名為 `get_name` 的方法。接著我們有一個 `RedFox` 的結構，它實作了 `NoisyAnimal` 這個特徵。所以我們可以在 `RedFox` 的實例上呼叫 `get_noise` 和 `get_name`。

看完今天的特徵，大家是不是又更懂 Rust 了呢？（希望我沒有說明的很爛(灬ºωº灬)）
那麼大家明天見囉！