---
title: rust_trait
tags:
---

特徵（Trait）是 Rust 的一個重要的特性，它可以讓我們在不同的型別上定義共用的行為，並且可以在不同的型別上使用相同的函式。

特徵有點像是其他語言的介面（interface），目前我們可以先把它想成是一種定義共享行為的方式。

## 定義特徵

要怎麼定義一個特徵呢？

其實很簡單，只要在 trait 關鍵字後面加上特徵的名稱，然後在大括號裡面定義特徵的行為就可以了。

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

```rust
struct RedFox {
	enemy: bool,
	life: u32,
}

trait Noisy {
	fn get_noise(&self) -> &str;
}

impl Noisy for RedFox {
	fn get_noise(&self) -> &str {
		"Meow?"
	}
}
```

在上面的程式碼中，我們定義了一個名為 `RedFox` 的結構，裡面有兩個欄位，分別是 `enemy` 和 `life`。

然後我們定義了一個名為 `Noisy` 的特徵，裡面有一個名為 `get_noise` 的函式，它會回傳一個 `&str`。

最後我們在 `RedFox` 上實作了 `Noisy` 這個特徵，並且提供了 `get_noise` 的具體實現。

但是我們為什麼不直接這樣寫就好了呢？

```rust
struct RedFox {
  enemy: bool,
  life: u32,
}

impl RedFox {
  fn get_noise(&self) -> &str {
    "Meow?"
  }
}
```

雖然這樣寫，我們的確可以直接在 `RedFox` 上呼叫 `get_noise` 。但是我們就無法在其他型別上使用 `get_noise` 了。

所以我們還是需要一個方法，讓我們可以在其他型別上使用 `get_noise`，這個方法就是特徵。

## 特徵的泛型

```rust
fn print_noise<T: Noisy>(item: T) {
	println!("{}",item.get_noise());
}

impl Noisy for u8 {
	fn get_noise(&self) -> &str { "BYTE!" }
}

fn main() {
	println!(5_u8); // prints "BYTE!"
}
```

在上面的程式碼中，我們定義了一個名為 `print_noise` 的函式，它接受一個泛型參數 `T`，並且限制 `T` 必須實作 `Noisy` 這個特徵。

然後我們在 `main` 函式中呼叫了 `print_noise`，並且傳入了一個 `u8` 數字。

我們在 `u8` 上實作 `Noisy` 這個特徵，並且提供 `get_noise` 的具體實現。

所以當我們呼叫 `print_noise` 時，會呼叫 `u8` 上的 `get_noise`，並且印出 `BYTE!`。

## 特徵作為參數

```rust
fn print_noise(item: &Noisy) {
  println!("{}",item.get_noise());
}

fn main() {
  let fox = RedFox { enemy: false, life: 100 };
  print_noise(&fox);
}
```

在上面的程式碼中，我們定義了一個名為 `print_noise` 的函式，它接受一個參數 `item`，並且限制 `item` 的型別必須實作 `Noisy` 這個特徵。

然後我們在 `main` 函式中呼叫了 `print_noise`，並且傳入了一個 `RedFox` 的實例。

因為 `RedFox` 實作了 `Noisy` 這個特徵，所以我們可以傳入 `RedFox` 的實例。

## 特徵作為回傳值

```rust
fn get_noise() -> impl Noisy {
  RedFox { enemy: false, life: 100 }
}

fn main() {
  let fox = get_noise();
  println!("{}",fox.get_noise());
}
```

在上面的程式碼中，我們定義了一個名為 `get_noise` 的函式，它會回傳一個實作了 `Noisy` 這個特徵的型別。

然後我們在 `main` 函式中呼叫了 `get_noise`，並且將回傳值儲存在 `fox` 變數中。

因為 `RedFox` 實作了 `Noisy` 這個特徵，所以我們可以將 `RedFox` 的實例傳給 `get_noise`。

## 特徵的繼承

```rust
trait Noisy {
  fn get_noise(&self) -> &str;
}

trait NoisyAnimal: Noisy {
  fn get_name(&self) -> &str;
}

struct RedFox {
  enemy: bool,
  life: u32,
}

impl Noisy for RedFox {
  fn get_noise(&self) -> &str {
    "Meow?"
  }
}

impl NoisyAnimal for RedFox {
  fn get_name(&self) -> &str {
    "Red Fox"
  }
}

fn main() {
  let fox = RedFox { enemy: false, life: 100 };
  println!("{}",fox.get_noise());
  println!("{}",fox.get_name());
}
```

在上面的程式碼中，我們定義了一個名為 `Noisy` 的特徵，它定義了一個名為 `get_noise` 的方法。

然後我們定義了一個名為 `NoisyAnimal` 的特徵，它繼承了 `Noisy` 這個特徵，並且定義了一個名為 `get_name` 的方法。

然後我們定義了一個名為 `RedFox` 的結構，它實作了 `NoisyAnimal` 這個特徵。

所以我們可以在 `RedFox` 的實例上呼叫 `get_noise` 和 `get_name`。
