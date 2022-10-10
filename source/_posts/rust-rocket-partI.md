---
title: 來玩 Rust 的框架吧！ - Rocket - Part I
date: 2022-10-11 01:10:50
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - rocket
decription: Rust 的框架 Rocket
---

今天開始我們將會開始使用 Rust 的 Web 框架 -- [Rocket](https://rocket.rs/)，來建立一個簡單的 Web Server。

## Rocket 是什麼？

簡單介紹一下 Rocket，Rocket 是一個基於 Rust 語言的一個 Web 框架，截止目前為止在 GitHub 上一共有 18.6k 的星星數。

Rocket 的設計靈感來自於 Ruby on Rails、Flask等知名的 Web 框架，並且在設計上有著類似的特性，並以 3 個核心概念來設計：

- 安全、正確，還有重視開發者體驗。
- 所有的請求都會由 Rocket 來處理型別。
- 不強制使用 Rocket 的函式庫，可以讓開發者自由選擇。

然後個人覺得 Rocket 的文件寫的非常的完整，算是蠻適合新手入門的一個框架。

## 從 Hello World 開始

那麼我們就從一個簡單的 Hello World 開始，首先我們先建立一個 binary-based 的新專案：

```bash
$ cargo new --bin rocket-server
```
### 安裝 Rocket

在 Rust 安裝套件非常容易，只要在 `Cargo.toml` 裡面 `[dependencies]` 的下方加入我們要安裝的套件就可以了，在這裡像這樣：

```toml
[dependencies]
rocket = "0.5.0-rc.2"
```

然後在 `main.rs` 修改一下 code：

```rust
#[macro_use] extern crate rocket;

#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![index])
}
```

接著在終端機輸入 `cargo run`，一開始會先下載 Rocket 的套件，然後會接著執行程式，終端機的畫面會是這樣：

![CleanShot 2022-10-08 at 15.44.16@2x](https://i.imgur.com/tfrH2pz.png)

都沒問題的話，可以按照提示連去網址，或是去 `http://localhost:8000`，就可以看到我們的瀏覽器顯示出 Hello World 了。（雖然畫面有點陽春┐(´д`)┌）

以上就是使用 Rocket 建立一個簡單的 Hello World，跟之前用純 Rust 相比是不是感受到他的方便呢？

稍微講解一下剛剛運作的 code，首先我們先引入 Rocket 的套件：

```rust
#[macro_use] extern crate rocket;
```

然後我們定義一個 `index` 的函式，並且回傳一個 `&'static str`，這個 `&'static str` 代表的是一個字串的 reference，而且這個 reference 的生命週期是 `'static`，也就是整個程式的生命週期。

```rust
#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}
```

然後在 `#[launch]` 的函式裡面，我們使用 `rocket::build()` 來建立一個 Rocket 的實例，然後使用 `mount` 來將我們的 `index` 函式 mount 到 `/` 的路徑上，最後回傳這個 Rocket 實例。

```rust
#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![index])
}
```

Rocket 在這裡也提供了另一種啟動 server 的寫法：

```rust
#[rocket::main]
async fn main() -> Result<(), rocket::Error> {
    let _rocket = rocket::build()
        .mount("/", routes![index])
        .launch()
        .await?;

    Ok(())
}
```

兩者都可以運行，但是我覺得 `#[launch]` 的寫法比較簡潔，不過還是有些許的差異，例如 `#[launch]` 會自動幫你建立一個 `main` 函式，而如果使用 `#[rocket::main]` 的話，開發者自己處理的部分可能就比較多，端看個人喜好。

## JSON response

前面我們都只是回傳一個字串，但是如果我們要回傳一個 JSON 的話，該怎麼做呢？

這裡我們可以使用 [Serde](https://github.com/serde-rs/serde) 這個框架來幫忙處理 JSON，首先我們要先在 `Cargo.toml` 裡面加入，並且修改一下，這裡幫大家整理好了：

```toml
[dependencies]
rocket = { version = "0.5.0-rc.2", features = ["json"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
```

接著在 `main.rs` 裡面修改一下：

```rust
#[macro_use]
extern crate rocket;

use rocket::serde::json::{json, Json, Value};

#[get("/")]
fn index() -> Value {
    json!({ "name": "Bucky", "age": 18, "favorite_games": ["splatoon 3", "zelda"] })
}

#[launch]
fn rocket() -> _ {
    let routes = routes![index];
    rocket::build().mount("/", routes)
}
```

接著一樣重新執行，然後瀏覽器重新整理，就可以看到回傳的 JSON 了。

今天就先到這邊，明天再繼續建立一個簡單 CRUD 的 API。