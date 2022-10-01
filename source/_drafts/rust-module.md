---
title: rust_module
tags:
---

## 什麼是模組（module）？

在使用 Cargo 建立的專案中，會像下方這樣，所有程式都需要經由 `main` 來做處理。但是我們當然不可能把所有邏輯都寫在同一個檔案中，這樣子會非常難維護。

```
project
|__Cargo.toml
|__src
	|__main.rs
```

所以我們可以透過模組化的方式來管理專案，最後再經由 `main` 來處理。
我們來做一個示範，首先在 /src 路徑下新增一個 `lib.rs`，裡面我們建立一個函式 `say_hi`， 裡面我們寫了一段程式來印出 “Hi!”。

```rust
fn say_hi() {
    println!("Hi!");
}
```

接著在 `main` 裡面引入剛剛建立的 `say_hi`。
寫法是先寫這個專案的名字（不是這個專案資料夾名字，而是指 `Cargo.toml` 裡面的 package name），這個專案的名稱是 `hello_world`，接著加上一個標識符 `::`，最後再加上要引入的函式名稱即可，就像下方範例：

```rust
fn main() {
    hello_world::say_hi();
}
```

如果執行的話，會出現完整的錯誤提示。
提到 `say_hi` 函式有建立但沒有執行，並且在 `main` 中呼叫的 `say_hi` 不正確，並提示你說目前這是 private 的函式。

解決的方法是只要在該 private 的函式放上前綴詞 - `pub` ，只要設成 public 就可以解決這個問題：

```rust
pub fn say_hi() {
    println!("Hi!");
}
```

那麼再執行一次，就可以正確出現執行結果。

## 模組的階層

在上面的範例中，我們只有一個模組，但是如果我們想要建立更多的模組，就可以透過階層的方式來建立，像下方這樣：

```
project
|__Cargo.toml
|__src
  |__main.rs
  |__lib.rs
```

在 `lib.rs` 中，我們可以透過 `mod` 關鍵字來建立模組，像下方這樣：

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}
```

這樣子就可以建立一個階層的模組，但是這樣子的話，我們就無法在 `main` 中呼叫 `add_to_waitlist` 了，因為它是 private 的，所以我們需要在 `hosting` 前面加上 `pub`，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `use` 關鍵字

在上面的範例中，我們可以透過 `use` 關鍵字來引入模組，像下方這樣：

```rust
use crate::front_of_house::hosting;

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `as` 關鍵字

在上面的範例中，我們可以透過 `as` 關鍵字來重新命名模組，像下方這樣：

```rust
use crate::front_of_house::hosting;

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `super` 關鍵字

在上面的範例中，我們可以透過 `super` 關鍵字來引入上層的模組，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    // Order a breakfast in the summer with Rye toast
    let mut meal = back_of_house::Breakfast::summer("Rye");
    // Change our mind about what bread we'd like
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // The next line won't compile if we uncomment it; we're not allowed
    // to see or modify the seasonal fruit that comes with the meal
    // meal.seasonal_fruit = String::from("blueberries");
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `pub use` 關鍵字

在上面的範例中，我們可以透過 `pub use` 關鍵字來重新導出模組，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `pub(crate)` 關鍵字

在上面的範例中，我們可以透過 `pub(crate)` 關鍵字來將模組設定為 crate 的 public，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub(crate) fn add_to_waitlist() {}
    }
}

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `pub(in path)` 關鍵字

在上面的範例中，我們可以透過 `pub(in path)` 關鍵字來將模組設定為 path 的 public，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub(in crate::front_of_house) fn add_to_waitlist() {}
    }
}

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `pub(self)` 關鍵字

在上面的範例中，我們可以透過 `pub(self)` 關鍵字來將模組設定為 self 的 public，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub(self) fn add_to_waitlist() {}
    }
}

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。

## 使用 `pub(super)` 關鍵字

在上面的範例中，我們可以透過 `pub(super)` 關鍵字來將模組設定為 super 的 public，像下方這樣：

```rust
mod front_of_house {
    pub mod hosting {
        pub(super) fn add_to_waitlist() {}
    }
}

fn main() {
    hosting::add_to_waitlist();
}
```

這樣子就可以在 `main` 中呼叫 `add_to_waitlist` 了。
