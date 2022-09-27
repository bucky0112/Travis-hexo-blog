---
title: rust-collections
tags:
---

Rust 的標準函式庫有一些非常實用的資料結構，稱之為集合（collections）。這些集合包含了一些常見的資料結構，例如 vector、hash map、linked list 等等。這些集合都是泛型的，所以可以用來存放任何型別的資料。

## 向量（Vector）

vector 是一個可以動態增長的陣列。它的實作是用一個 buffer 來存放資料，當 buffer 不夠用時，會自動增長。vector 的實作是用 unsafe Rust 來實現的，所以它的效能比較好。

### 建立一個 vector

建立一個空的 vector 可以這樣做：

```rust
let mut v: Vec<i32> = Vec::new();
```

或者這樣：

```rust
let mut v = vec![];
```

### 增加元素

可以用 `push` 方法來增加元素：

```rust
v.push(1);
v.push(2);
v.push(3);
```

### 讀取元素

可以用索引的方法來讀取元素：

```rust
let third: &i32 = &v[2];
println!("The third element is {}", third);
// The third element is 3
```

如果索引超出範圍，會 panic：

```rust
let does_not_exist = &v[100];
```

如果要避免 panic，可以用 `get` 方法：

```rust
match v.get(2) {
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
// The third element is 3
```

`match` 是一個很強大的語法，基本上可以拿來取代 `if...else`（上次忘記提 QQ）。在這個例子中，如果 `v.get(2)` 回傳 `Some`，就會執行 `println!`，如果回傳 `None`，就會執行 `println!`。

## 雜湊映射（Hash map）

雜湊映射是一個可以用 key 來取得 value 的資料結構。它的實作是用一個陣列來存放資料，每個元素都是一個 tuple，tuple 的第一個元素是 key，第二個元素是 value。雜湊映射的實作是用 unsafe Rust 來實現的，所以它的效能比較好。

### 建立一個雜湊映射

建立一個空的雜湊映射，指定 key 和 value 的型別：

```rust
let mut scores: HashMap<String, i32> = HashMap::new();
```

或者不寫：

```rust
let mut scores = HashMap::new();
```

### 增加元素

可以用 `insert` 方法來增加元素：

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

println!("{:?}", scores);
// {"Yellow": 50, "Blue": 10}
```

這裡使用了 `use` 來引入函式庫中的 `HashMap`。`insert` 方法的第一個參數是 key，第二個參數是 value。

### 讀取元素

可以用 `get` 方法來讀取元素：

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name);
println!("{:?}", score);
// Some(10)
```

這裡先建立一個 `String`，然後用 `get` 方法來取得元素。最後會回傳一個 `Option`，所以要用 `match` 來取得元素的值：

```rust
match score {
    Some(score) => println!("The score is {}", score),
    None => println!("There is no score"),
}
// The score is 10
```

### 迭代

如果想要一次印出全部的結果的話，可以用 `for` 迴圈來迭代雜湊映射：

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
// Yellow: 50
// Blue: 10
```
