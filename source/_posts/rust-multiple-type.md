---
title: Rust 的複合型別
date: 2022-09-23 01:00:03
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: Rust 的複合型別
---

在 Rust 中，有些型別是由其他型別組合而成的，這些型別稱為複合型別，這些複合型別可以讓我們更好的處理資料，讓程式更簡潔。


## 元組（tuple）

元組有固定長度的特性，代表著一但宣告完，之後就無法再增加或刪減。
建立元組的方法是用一個 `()` 將值放入，每個值之間用 `,` 分隔，不同值不限定相同型別：

```rust
let nums = (1, 4.4, 333);
```

雖然不一定要加上型別，但以下為加上型別的範例：

```rust
let nums: (u8, f64, i32) = (1, 4.4, 333);
```

兩種方法可以取出 tuple 的值：

### 1.

```rust
fn main() {
    let nums: (u8, f64, i32) = (1, 4.4, 333);
    let one = nums.0;
    let two = nums.1;
    let three = nums.2;
    println!("{}, {}, {}", one, two, three);
}
```

### 2.

```rust
fn main() {
    let tup: (char, bool, i32) = ('Y', false, 333);
    let (one, two, three) = tup;
    println!("{}, {}, {}", one, two, three);
}
```

## 陣列（array）

陣列和元組不同的是，每個型別都必須是一樣的，而宣告的方式跟大部分的陣列宣告差不多，都是使用 `[]` 將值放入，並用 `,` 區分開來。就像這樣：

```rust
let new_arr = [1, 2, 3];
```

如果要加上型別的話，也是用 `[]`，但要注意的是，只需要加入型別跟值的數量，並且用 `;` 區隔，就像這樣：

```rust
let new_arr = [u8； 3] = [1, 2, 3];
```

而 Rust 的陣列跟其他程式語言比較不同的地方在於，Rust 的陣列有數量的限制，最多為 32，如果超過的部分，資料就會被丟棄。
所以 Rust 的陣列適合用在已確定值的多寡，像是一年有十二個月、一週有七天這種確定陣列長度的情況下。

## 向量（vector）

向量 `Vec<T>` 和陣列類似，但不一樣的是，向量是一種可以動態可擴增的陣列，也就是可以任意改變其大小，在裡面放入新的或是刪除元素。
要建立向量也很簡單，可以使用 `vec!` 巨集來宣告，就像這樣：

```rust
let x = vec![1, 3, 5, 7, 9];
```

或是呼叫 `Vec::new` 來建立，然後將元素放入：

```rust
let mut x = Vec::new();
x.push(1);
x.push(3);
x.push(5);
x.push(7);
x.push(9);
```

以上印出的結果都會是一樣的，可以用 `println!()` 印出結果，但這裡要特別注意的是，原本這個函式的第一個參數只需要帶入 `"{}"`，但因為複合型別跟其他的型別不太一樣，所以如果要印出全部的值的話，可以在 `{}` 內加入 `:?`，這樣就可以印出全部的結果，就像下方範例：

```rust
println!("{:?}", x);
```

如果要取得特定位置的值，只要使用 `[]` 來存取，並在裡面放入索引值就可以了，就像這樣：

```rust
let x = vec![1, 3, 5, 7, 9];
println!("{}", x[1]);
```

索引值也可以使用變數，例如：

```rust
let x = vec![1, 3, 5, 7, 9];
let i = 3;
println!("{}", x[i]);
```

但是索引值的型別只能使用 `usize`，如果使用其他型別會出錯，這邊使用上要特別注意。