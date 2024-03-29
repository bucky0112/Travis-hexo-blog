---
title: rust-day29
date: 2022-10-14 00:29:31
tags:
categories:
keywords:
decription:
---


目前為止我們都是使用模擬的資料，但是我們的 API 會需要真實的資料，所以今天我們要來做一個簡單的資料庫。

## SQLite

SQLite 是一個輕量級的資料庫，我們可以使用它來儲存我們的資料，我們這個專案會來嘗試看看。

首先我們要先安裝 SQLite 的套件，我們可以在 [SQLite 官網](https://www.sqlite.org/) 找到相關的資訊，為了篇幅的緣故，我們這邊就不贅述了。

## Diesel

我們今天要使用的是 [Diesel](https://diesel.rs/)，一個 ORM 的 crate，ORM 是 Object Relational Mapping 的縮寫，簡單來說就是可以讓我們使用物件導向的方式來操作資料庫。

我們要使用 Diesel 的 CLI 來幫我們產生相關的 code，所以我們要先安裝它。

```bash
$ cargo install diesel_cli --no-default-features --features sqlite
```

如果過程中沒什麼問題的話，可以試著執行 `diesel` 看看，如果有看到相關的訊息，那就代表安裝成功了。

然後我們要設定一下，並指定資料庫為 SQLite。

```bash
$ diesel setup --database-url=database.sqlite
```

這時候會看到我們的專案目錄多了一個 `migrations` 的資料夾，還有一個 `Diesel.toml` 的設定檔。

![CleanShot 2022-10-09 at 23.57.32@2x](https://i.imgur.com/5ptnu6o.png)

接著我們要來建立一個 migration，migration 就是我們要對資料庫做的變更，我們可以使用 `diesel migration generate` 來產生一個 migration。

```bash
$ diesel migration generate create_demo
```

這時候會看到我們的 `migration` 多了一個資料夾，裡面會有一個 `up.sql` 和 `down.sql`，這兩個檔案就是我們要對資料庫做的變更。

![CleanShot 2022-10-10 at 00.04.49@2x](https://i.imgur.com/O3GhUwU.png)

繼續編輯 `up.sql`，我們要來建立一個 `demo` 的 table。

```sql
CREATE TABLE demo (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name VARCHAR NOT NULL,
  email VARCHAR NOT NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
)
```

還有一個 `down.sql`：

```sql
DROP TABLE demo
```

然後我們要讓 Diesel 來執行這個 migration：

```bash
$ diesel migration run --database-url=database.sqlite
```

這時候 `src` 裡面會多一個 `schema.rs`，裡面會有我們剛剛建立的 table 的相關設定。

![CleanShot 2022-10-10 at 00.17.54@2x](https://i.imgur.com/mkd6yJ8.png)

如果要確保我們的 migration 是正確的，可以使用 `diesel migration redo` 來執行。

```bash
diesel migration redo --database-url=database.sqlite
```

## 設定 Diesel 跟 Rocket 的連結

雖然我們已經有安裝 Diesel 了，但是我們還是需要設定一下，讓 Diesel 跟 Rocket 連結起來。

`Cargo.toml` 的設定是這樣：

```toml
[dependencies]
rocket = { version = "0.5.0-rc.2", features = ["json"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0.85"
base64 = "0.13"
diesel = { version = "1.4", features = ["sqlite", "r2d2"] }
diesel_migrations = "1.4"

[dependencies.rocket_sync_db_pools]
version = "0.1.0-rc.2"
default-features = false
features = ["diesel_sqlite_pool"]
```

裡面有些版本大家可以再自行調整。

然後再新增一個 `Rocket.toml`，裡面的設定是這樣：

```toml
[global.databases]
sqlite_path = { url = "database.sqlite" }
```

接著我們要在 `main.rs` 裡面新增一些設定：

```rust
#[macro_use]extern crate rocket;
#[macro_use]extern crate rocket_sync_db_pools;
#[macro_use]extern crate diesel;
#[macro_use]extern crate diesel_migrations;

#[database("sqlite_path")]
struct DbConn(diesel::SqliteConnection);

fn rocket() -> _ {
    let routes = routes![get_demo, view_demo, create_demo, update_demo, delete_demo];
    rocket::build()
        .mount("/", routes)
        .register("/", catchers![not_found])
        .attach(DbConn::fairing())
}
```

以上就是我們所要設定的 Diesel 跟 Rocket 的連結。

接著來測試一下目前設定是否都正常，我們先在 `get_demo` 加上新的參數：

```rust
#[get("/demo")]
fn get_demo(_auth: BasicAuth, _conn: DbConn) -> Value {
// 省略...
}
```

然後執行 `cargo run`，如果沒有錯誤的話，就代表我們的設定都沒有問題。

## Diesel Model query

接下來我們要來建立 Model，在 `src` 中新增一個 `models.rs`，裡面的程式碼如下：

```rust
#[derive(Queryable)]
pub struct Demo {
    pub id: i32,
    pub name: String,
    pub email: String,
    pub created_at: String,
}
```

然後 `src` 中的 `schema.rs` 要改成這樣：

```rust
diesel::table! {
    demo (id) {
        id -> Integer,
        name -> Text,
        email -> Text,
        created_at -> Timestamp,
    }
}
```

接著在 `main.rs` 帶入 Models 和 Schema：

```rust
// 省略...
mod auth;
mod models;
mod schema;

use diesel::prelude::*;
use auth::BasicAuth;
use models::*;
use schema::*;
use rocket::response::status;
use rocket::serde::json::{json, Json, Value};

#[database("sqlite_path")]
struct DbConn(diesel::SqliteConnection);

#[get("/demo")]
async fn get_demo(_auth: BasicAuth, conn: DbConn) -> Value {
    conn.run(|c| {
        let all = demo::table.limit(100).load::<Demo>(c).expect("Error loading demo");
        json!(all)
    })
    .await
}
```

這時候會發現 `json!(all)` 會有錯誤，但不確定是什麼問題，我們可以先執行 `cargo run`，然後再來看看錯誤訊息：

![CleanShot 2022-10-14 at 01.22.54@2x](https://i.imgur.com/kTdcWCJ.png)

仔細看一下錯誤訊息，可以發現在 `models::Demo` 中，缺少了 `Serialize`，所以我們要在 `models.rs` 中加上 `Serialize`：

```rust
#[derive(serde::Serialize, Queryable)]
```

這時候再執行一次 `cargo run`，就可以正常執行了。
然後用 Postman 測試一下：

![CleanShot 2022-10-14 at 01.35.05@2x](https://i.imgur.com/ISII8Dg.png)

可以發現目前拿到的是空陣列，代表我們設定 OK 了。