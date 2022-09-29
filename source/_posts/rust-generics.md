---
title: 泛型
date: 2022-09-30 02:12:14
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - generics
decription: Rust 的泛型
---

## 什麼是泛型？

在寫程式中，可能會遇到不同需求，例如想要得到多種類型的結果，像是結果型別為 i32，又或是浮點數 f64 等等...。這對工程師來說根本是種折磨，因為工程師就是懶，要寫相同的過程，但結果只是不同型別的話，就像下面這樣：

```rust
fn result_i8(x: i8, y: i8) -> i8 {
    x + y
}
fn result_i32(x: i32, y: i32) -> i32 {
    x + y
}
fn result_f32(x: f64, y: f64) -> f64 {
    x + y
}
```

還好 Rust 提供了泛型（generics）。

泛型可以提高程式碼的重複使用，也就是如果程式碼中有很多重複的部分的話，泛型就可以處理重複程式碼的問題。

## 泛型函式定義

在使用泛型定義函式時，只需要在函式名稱的後面加上 `<T>`，T 代表泛型參數，雖然也可以把 T 換成任一字母，但慣例會用 T 來表示 Type，就像這樣：

```rust
fn result<T: std::ops::Add<Output = T>>(x: T, y: T) -> T {
    x + y
}
```

_std::ops::Add 是 Rust 提供的一種特徵_

或是寫這樣：

```rust
use std::ops::Add;
fn main() {
    fn result<T: Add<Output = T>>(x: T, y: T) -> T {
        x + y
    }
}
```

## 泛型結構

泛型也可以運用在結構上，例如：

```rust
struct Score<T> {
    x: T,
	  y: T,
}

fn main() {
    let int = Score { x: 5, y: 10 };
	  let float = Score { x: 1.0, y: 4.0 };
}
```

上面的範例中，可以看到建立了一個結構 `Score`，並且一樣起手式在後面加上了 `<T>`，這樣就代表了使用了泛型的參數，這樣就是一個泛型結構，並且結構中有 x 跟 y 兩個欄位，型別都賦予泛型。
這樣一來，使用 `Score` 這個結構，並賦予值的時候就只能使用同種型別，也就是如果 x 是 i32 型別的話，那麼 y 也必須同樣是 i32 的型別。

如果兩種是不同型別的話，那麼這時候就會報錯。

```rust
fn main() {
	  // expected integer, found floating-point number
    let int = Score { x: 5, y: 10.2 };
	  // expected floating-point number, found integer
	  let float = Score { x: 1.0, y: 4 };
}
```

但如果真的想要不同型別的話，也是沒問題的。既然一個泛型參數不夠的話，那就再加一個就好了。就像這樣：

```rust
struct Score<T, N> {
    x: T,
	  y: N,
}
```

剛剛的需求跟遇到的問題就可以迎刃而解，也就是結構是可以使用多個泛型參數。

## 泛型列舉

列舉也可以使用泛型，就像這樣：

```rust
enum Option<T> {
    Some(T),
    None,
}
```

就是我們在昨天的文章中提到的 `Option` 列舉，這個列舉就是使用了泛型，也就是 `Option` 列舉中的 `Some` 跟 `None` 都是泛型。

## 泛型方法

方法也可以使用泛型，就像這樣：

```rust
struct Score<T> {
    x: T,
    y: T,
}

impl<T> Score<T> {
    fn x(&self) -> &T {
        &self.x
    }
    fn y(&self) -> &T {
        &self.y
    }
}
```

上面的範例中，我們建立了一個結構 `Score`，並且在結構中定義了兩個方法 `x` 跟 `y`，這兩個方法都是泛型的，也就是在方法名稱的後面加上了 `<T>`，這樣就代表了使用了泛型的參數，這樣就是一個泛型方法。

要執行的話，就像這樣：

```rust
fn main() {
    let int = Score { x: 5, y: 10 };
    println!("x: {}", int.x()); // x: 5
    println!("y: {}", int.y()); // y: 10
}
```

以上就是有關 Rust 泛型的基本介紹，希望大家可以對 Rust 泛型有更深的了解。