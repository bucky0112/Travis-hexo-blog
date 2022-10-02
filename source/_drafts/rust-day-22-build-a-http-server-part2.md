---
title: rust-day-22-build-a-http-server-part2
tags:
---

## 監聽 TCP 連線

接下來我們要開始監聽 TCP 連線，這部分我們要用到 `std::net` 模組，這個模組提供了 TCP 連線的相關功能。

首先我們要先建立一個 `TcpListener`，並且使用 `bind` 方法來綁定一個 IP 位址和 Port，並且使用 `unwrap` 方法來處理錯誤，如果有錯誤的話就會 panic 掉，並且印出錯誤訊息：

```rust
// src/server.rs
use std::net::TcpListener;

pub struct Server {
    addr: String,
}

impl Server {
    pub fn new(addr: String) -> Self {
        Self { addr }
    }

    pub fn run(self) {
        println!("Listening on {}", self.addr);
        let listener = TcpListener::bind(&self.addr).unwrap();
    }
}
```

接著就可以來測試一下，我們可以在終端機輸入 `cargo run` 來執行程式，如果沒有錯誤的話，就會印出 `Listening on 127.0.0.1:8080`，這代表我們的伺服器已經開始監聽了。

## 使用 `loop` 來檢查連線

接下來我們要使用 `loop` 來檢查連線，這邊我們使用 `for` 迴圈來檢查連線，並且使用 `match` 來處理 `Result` 型態的回傳值，如果有錯誤的話就會印出錯誤訊息：

```rust
// src/server.rs
...省略

impl Server {
    pub fn new(addr: String) -> Self {
        Self { addr }
    }

    pub fn run(self) {
        println!("Listening on {}", self.addr);
        let listener = TcpListener::bind(&self.addr).unwrap();

        loop {
            match listener.accept() {
                Ok((stream, addr)) => {
                    println!("Accepted a connection from {}", addr);
                }
                Err(e) => println!("Failed to establish a connection: {}", e),
            }
        }
    }
}
```

接著我們可以再次執行程式，這時候我們會發現程式會一直印出訊息，這代表我們的伺服器正常情況下已經開始不斷連線了。

![CleanShot 2022-10-02 at 05.05.21@2x](https://i.imgur.com/6TyixWP.png)

## 使用 Read trait 來讀取請求

接下來我們要使用 `Read` trait 來讀取請求，這邊我們使用 `std::io::Read`，這個 trait 提供了一個 `read` 方法，這個方法可以讀取請求，並且回傳一個 `Result` 型態的值，如果有錯誤的話就會回傳 `Err`，如果沒有錯誤的話就會回傳 `Ok`，並且回傳一個 `usize` 型態的值，這個值代表著讀取的長度。

```rust
// src/server.rs
use std::io::Read;

...省略

impl Server {
    pub fn new(addr: String) -> Self {
        Self { addr }
    }

    pub fn run(self) {
        println!("Listening on {}", self.addr);
        let listener = TcpListener::bind(&self.addr).unwrap();

        loop {
            match listener.accept() {
                Ok((mut stream, addr)) => {
                    println!("Accepted a connection from {}", addr);

                    let mut buffer = [0; 1024];
                    // 這一段會判斷讀取的長度是否大於 0
                    match stream.read(&mut buffer) { // 這邊會回傳一個 Result 型態的值
                        Ok(_) => println!("Received a request: {}", String::from_utf8_lossy(&buffer)), // 這邊會回傳一個 usize 型態的值
                        Err(e) => println!("Failed to read from connection: {}", e), // 這邊會回傳一個 io::Error 型態的值
                    }
                }
                Err(e) => println!("Failed to establish a connection: {}", e),
            }
        }
    }
}
```

接著我們可以再次執行程式，然後打開瀏覽器，輸入 `http://127.0.0.1:8080`

![CleanShot 2022-10-02 at 19.53.29@2x](https://i.imgur.com/j6hDeDS.png)

雖然畫面會顯示未傳送任何資料，但是我們可以在終端機看到我們的伺服器已經收到了請求：

![CleanShot 2022-10-02 at 19.53.29@2x](https://i.imgur.com/EaUDGNn.png)

## 處理回傳回應

```rust
// src/http/request.rs
use super::method::Method;
use std::convert::TryFrom;

...省略

impl TryFrom<&[u8]> for Request {
    type Error = String;

    fn try_from(buf: &[u8]) -> Result<Self, Self::Error> {
        unimplemented!()
    }
}
```

然後在 `src/server.rs`：

```rust
use crate::http::Request; // 這裡的 crate 是指 src 資料夾，並從 http 資料夾中 import Request
use std::convert::TryFrom; // 這裡的 std 是指標準函式庫，並從 convert 資料夾中 import TryFrom
use std::convert::TryInto; // 這裡的 std 是指標準函式庫，並從 convert 資料夾中 import TryInto
...省略
impl Server {
    pub fn new(addr: String) -> Self {
        Self { addr }
    }

    pub fn run(self) {
        println!("Listening on {}", self.addr);
        let listener = TcpListener::bind(&self.addr).unwrap();

        loop {
            match listener.accept() {
                Ok((mut stream, addr)) => {
                    println!("Accepted a connection from {}", addr);

                    let mut buffer = [0; 1024];
                    match stream.read(&mut buffer) {
                        Ok(_) => {
                            println!("Received a request: {}", String::from_utf8_lossy(&buffer));
                            match Request::try_from(&buffer[..]) {
                                Ok(request) => {}
                                Err(e) => println!("Failed to parse a request: {}", e),
                            };
                        }
                        Err(e) => println!("Failed to read from connection: {}", e),
                    }
                }
                Err(e) => println!("Failed to establish a connection: {}", e),
            }
        }
    }
}
```

上面的程式碼的意思是，我們從 `src/http/request.rs` 中 import `Request`，然後從 `std::convert::TryFrom` 中 import `TryFrom`，接著我們在 `match Request::try_from(&buffer[..])` 這邊，我們會嘗試將 `buffer` 轉換成 `Request`，如果成功的話，就會回傳 `Ok`，並且會回傳一個 `Request` 型態的值，如果失敗的話，就會回傳 `Err`，並且回傳一個 `String` 型態的值。

接下來先處理錯誤的部分，先在 `src/http/request.rs` 中加入：

```rust
// src/http/request.rs
use std::error::Error;
use std::fmt::{Display, Debug, Formatter, Result as FmtResult};

...省略
// 這邊是自訂的錯誤類別
pub enum ParseError {
    InvalidRequest,
    InvalidEncoding,
    InvalidProtocol,
    InvalidMethod,
}


impl ParseError {
    fn message(&self) -> &str {
        match self {
            Self::InvalidRequest => "Invalid Request",
            Self::InvalidEncoding => "Invalid Encoding",
            Self::InvalidProtocol => "Invalid Protocol",
            Self::InvalidMethod => "Invalid Method",
        }
    }
}

impl Display for ParseError {
    fn fmt(&self, f: &mut Formatter) -> FmtResult {
        write!(f, "{}", self.message())
    }
}

impl Debug for ParseError {
    fn fmt(&self, f: &mut Formatter) -> FmtResult {
        write!(f, "{}", self.message())
    }
}

impl Error for ParseError {}
```

記得加入 `type Error = ParseError;`，這樣就會回傳 `ParseError` 這個型態的值。

```rust
impl TryFrom<&[u8]> for Request {
    type Error = ParseError;

    fn try_from(buf: &[u8]) -> Result<Self, Self::Error> {
        unimplemented!()
    }
}
```

今天回傳的部分先到這邊結束，明天再接著繼續。