---
title: rust-day21-build-a-http-server-part1
tags:
---

從今天開始，我們要開始實作一個簡單的 HTTP 伺服器，並且複習一下並且運用我們之前學到 Rust 的知識，然後視情況加入一些需要了解的部分，到時候再加入解說，那麼就開始吧！

## 建立一個新專案

首先，我們要建立一個新的專案，這邊我們使用 `cargo` 來建立一個新的專案，使用 `cargo new` 指令，並且指定專案名稱為 `server`：

```bash
$ cargo new server
```

## 建立一個 Server 結構

建立完專案之後並用編輯器打開，直接進到 `/src/main.rs`，並且刪除原本的程式碼，並且改成：

```rust
fn main() {
    let server = Server::new("127.0.0.1:8080".to_string());
    server.run();
}

struct Server {
    addr: String,
}

impl Server {
    fn new(addr: String) -> Self {
        Self { addr }
    }

    fn run(self) {
        println!("Listening on {}", self.addr);
        // Listening on 127.0.0.1:8080
    }
}
```

這邊我們先建立一個 `Server` 結構，並且定義一個 `addr` 欄位，然後利用結構新增兩個方法，回傳 `Server` 的實例。第一個 `new` 方法，第二個 `run` 方法中。然後先測試一下印出 `addr` 的值，沒意外的話就會印出值。

## 建立 HTTP Request 和 method

接下來我們要建立一個 `Request` 結構，並且定義一個 `method` 欄位，並且定義一個 `Method` 枚舉，並且定義一些常用的 HTTP method，並加上型別：

```rust
enum Method {
    GET(String),
    POST,
    PUT,
    DELETE,
    HEAD,
    CONNECT,
    OPTIONS,
    TRACE,
    PATCH,
}
```

然後我們要在 `Request` 結構中定義一個 `method` 欄位，並且指定型別為剛剛建立的枚舉 `Method`，還有定義 `path` 和 `query_string`：

```rust
struct Request {
    path: String,
    query_string: Option<String>,
    method: Method,
}
```

上面的 `query_string` 是一個 `Option`，因為有些 HTTP request 是沒有 query string 的，所以我們要用 `Option` 來表示。

## 利用模組來組織程式碼

由於我們的 `main.rs` 中的程式碼開始變多，為了避免越來越多之後難以維護，所以我們要將程式碼分成不同的模組，並且在 `main.rs` 中引入這些模組，這樣就可以讓程式碼更好管理。

首先，我們要建立一個 `lib.rs`，並且在 `main.rs` 中引入這個模組：

```rust
mod server;
```

## 建立一個 parse_request 方法