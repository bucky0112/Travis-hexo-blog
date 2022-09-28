---
title: 結構
date: 2022-09-29 00:49:05
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - struct
decription: Rust 結構
---

結構（struct）是 Rust 提供的一種可以讓開發者建立資料型別相對複雜的一種方式，類似 JavaScript 的物件和 Python 的類別。

結構和元組類似，都是可以由不同不同型態組成，不過與元組不同地方在於必須要為每個組件命名，所以可以不必按照順序，就可以讀取或修改結構的個別資料。

Rust 提供了三種結構，分別是：

* 具名欄位（named-field）：最常用的用法，每個組件都有一個名稱。
* 類元組（tuple-like）：因為類似元組，所以也必須要按照順序使用。 
* 類單元（unit-like）：比較不常見，其結構完全沒有組件。

## 具名欄位

直接看範例如何建立一個具名欄位的結構：

```rust
struct Hunter {
    name: String,
	  nen_type: String,
 	  age: u8,
	  hunter_license: bool,
}
```

首先起手式先用關鍵字 `struct` 為整個結構命名，命名的方式為*駝峰命名（Camel Case）*，然後在大括號中加入欄位，欄位的名稱則是使用*蛇式命名（snake_case）*，接著就幫每個欄位定義型別。

定義完之後如果要使用剛剛建立的結構，可以建立一個變數並且指定為結構，就像剛剛建立的寫法差不多，但差別在我們可以給予每個欄位相對應型別的值，就像下面的寫法這樣：

```rust
let killua = Hunter {
    name: "奇犽".toString(),
	  nen_type: "變化系".toString(),
	  age: 14,
	  hunter_license: true,
};
```

### 取得結構中的值

如果要取得結構中某個值的話，假設要印出 *name* 和 * hunter_license* 的話，就像這樣：

```rust
println!("{}是{}獵人", killua.name, killua.nen_type);
// 奇犽是變化系獵人
```

如果要改變值的話，例如我們要幫奇犽加一歲。要注意的是 Rust 沒辦法讓開發者只標記特定欄位為可變，一定要整個變數。所以我們就先在變數前加上 `mut`，然後一樣直接先取出 *age* 這個值，接著就可以改變數值，就像這樣：

```rust
let mut killua = Hunter {
    name: "奇犽".to_string(),
	  nen_type: "變化系".to_string(),
	  age: 14,
	  hunter_license: true,
};

killua.age = 15;

println!("{}是{}獵人，今年 {} 歲", killua.name, killua.nen_type, killua.age);
// 奇犽是變化系獵人，今年 15 歲
```

## 類元組

Rust 也有像是元組的結構，就像這樣建立：

```rust
struct Score(i32, i32);
```

建立類元組的變數和值也很像建立元組，不過差別在要加入結構名稱：

```rust
let my_score = Score(59, 70);
```

要使用的話，也跟使用元組的方式一樣：

```rust
struct Score(i32, i32);
let my_score = Score(10, 20);
println!("{} {}", my_score.0, my_score.1);
// 59 70
```

## 類單元

第三種比較特別，因為在宣告結構型態的時候完全沒有欄位，就像這樣就建立完成了：

```rust
struct OneUnit
```

由於這種型態比較不常使用，這邊就先不介紹了。

## 結構的方法

在 Rust 中，結構也可以定義方法，就像這樣：

```rust
struct Hunter {
    name: String,
    nen_type: String,
    age: u8,
    hunter_license: bool,
}

impl Hunter {
    fn get_name(&self) -> &String {
        &self.name
    }
}

fn main() {
    let killua = Hunter {
        name: "奇犽".to_string(),
        nen_type: "變化系".to_string(),
        age: 14,
        hunter_license: true,
    };

    println!("{}", killua.get_name());
    // 奇犽
}
```

在上面的程式碼中，我們先定義了一個結構，然後在結構後面加上 `impl`，接著就可以定義方法了，方法的名稱就是 `get_name`，然後在方法的括號中加上 `&self`，這個 `&self` 就是指向結構的指標，接著就是回傳值，這邊回傳的是 `&String`，也就是指向字串的指標。

接著就可以在主程式中使用這個方法了。

Rust 的結構就介紹到這邊了，明天來介紹 Rust 的泛型。