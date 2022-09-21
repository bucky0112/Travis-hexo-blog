---
title: Rust 的基本型別
date: 2022-09-22 00:20:59
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的基本型別
---

Rust 的型別是非常重要的，因為它可以幫助我們在編譯時期就找出錯誤，而不是在執行時期才發現錯誤。

所以我們要了解一下 Rust 的型別，並且要知道如何使用它，才能夠感受到 Rust 帶來的好處。

Rust 主要有 4 種基本型別：
- 整數
- 浮點數
- 布林
- 字元

## 整數

整數有兩種變形，分為非帶號（Unsigned）、帶號（Signed）：

|  長度    | Unsigned | Signed |
| ------- | --------- | ------ | 
| 8位元     |  u8       | i8    |
| 16 位元	 | i16	     | u16   |
| 32 位元	 | i32       | u32   |
| 64 位元   | i64       | u64   |
| 128 位元  | i128     | u128  |
| 系統架構  | isize     | usize |

兩個的差異在在負號的差別，帶號就是帶負號，每一個帶號可以儲存的數字範圍從 -(2<sup>n - 1</sup>) 到 2<sup>n-1</sup> -1 以內的數字，`n` 是位元，例如 8 位元的 `i8` 最小到 -128，最大到 127；而非帶號的 `u8` 最小因為沒有負數，所以就從 0 開始，最大到 255。

比較特別的是 `isize` 和 `usize` 是依據使用者程式運行的電腦架構來決定大小，如果在 64 位元架構的電腦上就是 64 位元，以此類推。

那麼該使用那些整數型別呢？如果不確定的話，那就可以使用 Rust 的預設型別 - `i32`。

## 浮點數

浮點數相對整數來說單純很多，只有兩種型別：
- f32
- f64

分別代表 32 位元和 64 位元，預設的型別是 `f64`，擁有更高的精準度，如果沒有特別需求的話就用 `f64` 就可以了。所有的浮點數型別都是帶號。

### 數字運算範例

了解了數字的基本型別之後，就來寫點 code 來練習一下吧：

```rust
fn main() {
    let a: i32 = -5;
    let b: u32 = 10;
    let c: f64 = 3.5;
    let sum = b + 10;
    let negative: i32 = a - 10;
    let product: f64 = c * 3.0;
    let remainder: f64 = c / 3.0;
    println!(
        "sum: {}, negative: {}, product: {}, remainder: {}",
        sum, negative, product, remainder
    );
}
```

運算的結果如下圖：

![https://ithelp.ithome.com.tw/upload/images/20220921/20120293dkMXDqv1Vn.png](https://ithelp.ithome.com.tw/upload/images/20220921/20120293dkMXDqv1Vn.png)

## 布林

布林型別也非常簡單，只有 `true` 和 `false`，要定義布林型別的話加上 `bool` 就可以使用了。

以下是使用範例：

```rust
fn main() {
    let num = 5;
    let x: bool = true;
    let y: bool = false;

    if num < 10 {
        println!("{}", x)
    } else {
        println!("{}", y)
    }
}
```

## 字元

字元跟字串不太一樣，宣告是用單引號，代表一個 Unicode 值，大小為四個位元組，不管是中英日韓、表情符號都是屬於字元型別：

```rust
fn main() {
    let a = 'a';
    let you_tw = '你';
    let cat = '🐈';
    println!("{} {} {}", a, you_tw, cat);
}
```

雖然 Rust 是屬於強型別的語言，但是 Rust 的一些型別是可以自動推導的，例如上面的 `a`，因為它是字元型別，Rust 會自動推導出它是 `char` 型別，所以是可以不需要特地加上 `char`。

也因為如此，雖然型別對於一些寫久了弱型別的工程師來說可能會覺得有點麻煩，但是在寫程式的時候，不用一直去想這個變數是什麼型別，可以更專注在程式的邏輯上。