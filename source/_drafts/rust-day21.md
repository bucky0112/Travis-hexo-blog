---
title: rust-day21
tags:
---

從今天開始，我們要開始實作一個簡單的 HTTP 伺服器，並且複習一下並且運用我們之前學到 Rust 的知識，然後視情況加入一些需要了解的部分，到時候再加入解說，那麼就開始吧！

## 建立一個新專案

首先，我們要建立一個新的專案，這邊我們使用 `cargo` 來建立一個新的專案，使用 `cargo new` 指令，並且指定專案名稱為 `server`：

```bash
$ cargo new server
```

## 監聽 TCP 連線

接著，我們要開始實作一個 TCP 伺服器，並且監聽 TCP 連線。
Rust 的標準函式庫中有一個 `std::net` 模組，裡面有一個 `TcpListener` 型態，可以用來監聽 TCP 連線，所以我們可以先在 `src/main.rs` 中 使用 `TcpListener`：

```rust
use std::net::TcpListener;
 
const PORT: i32 = 3000;
 
fn main() {
    let listener = TcpListener::bind(format!("127.0.0.1:{}", PORT)).unwrap();
 
    for stream in listener.incoming() {
        let stream = stream.unwrap();
 
        println!("連線建立!");
    }
}
```

上面的程式碼的意思是，我們從 `std::net::TcpListener` 中 import `TcpListener`，然後我們建立一個 `PORT` 常數，並且指定為 `3000`，接著我們在 `main` 函式中，我們建立一個 `listener` 變數，並且使用 `TcpListener::bind` 方法。這個方法會傳回一個 `Result` 型態，所以我們使用 `unwrap` 方法來解包，如果有錯誤發生，程式就會 panic，然後我們使用 `for` 迴圈來監聽 TCP 連線，並且將 `stream` 變數綁定到 `listener.incoming()` 方法，這個方法會傳回一個 `TcpStream` 型態。

我們先試著執行一下，在終端機中執行 `cargo run`，然後打開瀏覽器，輸入 `http://127.0.0.1:3000`。

雖然瀏覽器會顯示以下畫面：

![CleanShot 2022-10-04 at 01.08.36@2x](https://i.imgur.com/2NOn8qD.png)

不過，我們可以在終端機中看到 `連線建立!` 的訊息，這代表我們的 TCP 伺服器已經成功的監聽到了 TCP 連線。

但是可能有人會好奇，為什麼我們才開啟一次 `http://127.0.0.1:3000`，卻可以看到好幾個訊息出來呢？這是因為瀏覽器除了請求頁面內容之外，也會試著請求其他資源，像是瀏覽器分頁上的 favicon.ico 網站圖示等等的資源，所以我們會看到好幾個訊息出來。

然後我們也可以試著重新整理網頁，雖然一樣是完全沒有畫面。但是在終端機會多出幾行訊息，這也就是說我們剛剛寫的 `listener.incoming()` 方法會持續監聽 TCP 連線，直到我們關閉程式。

## 讀取請求

接下來要來讀取請求，然後避免 `main` 的程式碼太長，我們先把讀取請求的程式碼放到 `handle_connection` 函式中：

```rust
use std::net::TcpStream;

// ...省略
fn handle_connection(stream: TcpStream) {
    println!("建立連線!");
}
```

這裡可以使用 `TcpStream` 型態，因為我們在 `main` 函式中，已經將 `stream` 變數綁定到 `listener.incoming()` 方法，這個方法會傳回一個 `TcpStream` 型態。

然後我們在 `main` 函式中，將 `handle_connection` 函式綁定到 `for` 迴圈中：

```rust
// ...省略
fn main() {
    // ...省略
    for stream in listener.incoming() {
        let stream = stream.unwrap();
 
        handle_connection(stream);
    }
}
```

接著我們在 `handle_connection` 函式中，使用 `stream.read` 方法來讀取請求。
這個 `read` 方法會需要從參數中傳入一個 `&mut [u8]` 型態的變數，所以我們先建立一個 `buffer` 變數，並且指定為一個 `u8` 型態的陣列，用來當作緩衝區使用。
而緩衝區的大小，沒有特別規定。不過，這裡設定為 512 bytes，也就是 512 個 `u8` 型態的元素，也已經非常足夠了。

這裡也將 `std::io::prelude::*` 引入，來實作 I/O 相關讀寫的方法。

```rust
use std::io::prelude::*;

fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 512];
 
    let c = stream.read(&mut buffer).unwrap();
}
```

接著我們可以使用 `println!` 來印出 `buffer` 變數的內容，這樣我們就可以看到請求的內容了：

```rust
// ...省略
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 512];
 
    let c = stream.read(&mut buffer).unwrap();
 
    println!("請求內容: {}", String::from_utf8_lossy(&buffer[..c]));
}
```

這裡使用 `String::from_utf8_lossy` 方法，來將 `buffer` 變數轉換成 `String` 型態，並且使用 `&buffer[..c]` 來指定要轉換的範圍。

然後我們重新執行程式，並且把瀏覽器打開。雖然這時候瀏覽器一樣沒有畫面，但是我們可以在終端機看到請求的內容了：

![CleanShot 2022-10-04 at 02.33.56@2x](https://i.imgur.com/FddM6uI.png)
