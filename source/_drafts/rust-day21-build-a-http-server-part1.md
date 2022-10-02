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

首先，在原本的 `main.rs` 中，把有關 `Server` 的部分重新整理成一個模組，並在 `main` 中的 `server` 引用 `mod server`:

```rust
use server::Server;

fn main() {
    let server = Server::new("127.0.0.1:8080".to_string());
    server.run();
}

mod server {
    struct Server {
        addr: String,
    }

    impl Server {
        fn new(addr: String) -> Self {
            Self { addr }
        }

        fn run(self) {
            println!("Listening on {}", self.addr);
        }
    }
}
```

但存檔後會發現編譯器會報錯說找不到 `Server`，這是因為我們還沒有引入這個模組，這部分蠻簡單的，只要加上 `pub` 關鍵字就可以了：

```rust
mod server {
    pub struct Server {
        addr: String,
    }

    impl Server {
        pub fn new(addr: String) -> Self {
            Self { addr }
        }

        pub fn run(self) {
            println!("Listening on {}", self.addr);
        }
    }
}
```

然後我們的 `Method` 和 `Request` 也可以整理成個別的模組，或是統一成一個模組，這裡我們的作法是這樣：

```rust
// http 模組
mod http {
    // request 模組
    pub mod request { // 這邊要加上 pub，因為我們要在 main.rs 中引入這個模組
        use super::method::Method; // 這邊要加上 super::，因為我們要引入上一層的 method 模組
        pub struct Request { // pub 表示這個結構是 public 的
            path: String,
            query_string: Option<String>,
            method: Method,
        }
    }
    // method 模組
    pub mod method { // 這邊要加上 pub，因為我們要在 main.rs 中引入這個模組
        pub enum Method { // 這邊也要加上 pub
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
    }
}
```

然後在 `main.rs` 引入 `http` 模組：

```rust
use server::Server;
use http::request::Request; // 引入 http 模組中的 request 模組
```

不過目前這些模組都還是在 `main.rs` 中，所以我們還是要另外新增檔案來放這些模組，這裡我們把 `server` 和 `http` 兩個模組分別做以下處理：

```rust
// src/server.rs
pub struct Server {
    addr: String,
}

impl Server {
    pub fn new(addr: String) -> Self {
        Self { addr }
    }

    pub fn run(self) {
        println!("Listening on {}", self.addr);
    }
}
```

因為 `http` 中有兩個模組，所以我們要再新增一個 `src/http` 目錄，並在裡面新增 `request.rs` 和 `method.rs` 兩個檔案，分別放 `Request` 和 `Method` 的程式碼：

```rust
// src/http/request.rs
use super::method::Method;

pub struct Request {
    path: String,
    query_string: Option<String>,
    method: Method,
}
```

```rust
// src/http/method.rs
pub enum Method {
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

然後由於有分成兩個檔案，所以我們還需要新增 `src/http/mod.rs`，然後引入這兩個檔案：

```rust
pub use method::Method;
pub use request::Request;

mod method;
pub mod request;
```

最後在 `main.rs` 中引入 `server` 和 `http`：

```rust
use server::Server;
use http::Method;
use http::Request;

mod http;
mod server;

fn main() {
    let server = Server::new("127.0.0.1:8080".to_string());
    server.run();
}
```

這樣就完成了模組的切分，是不是看起來乾淨很多。
明天會接著實作監聽 TCP 連線的部分。