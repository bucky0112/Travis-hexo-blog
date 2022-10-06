---
title: rust-day24
tags:
---

在開始建立多執行緒伺服器之前，我們先來模擬一下單一執行緒伺服器會遇到的問題。

## 模擬請求問題

直接上 code 吧！

```rust
use std::thread; // 引入 thread 模組
use std::time::Duration; // 引入 Duration 模組

// 省略...
fn handle_connection(mut stream: TcpStream) {
    // 省略...
    let delay = b"GET /delay HTTP/1.1\r\n";

    let (status_line, path) = if buffer.starts_with(get) {
        ("HTTP/1.1 200 OK", Path::new("./src/HTML/hello.html"))
    } else if buffer.starts_with(delay) {
        thread::sleep(Duration::from_secs(10)); // 這裡讓執行緒睡 10 秒
        ("HTTP/1.1 200 OK", Path::new("./src/HTML/delay.html")) // 這裡我們要新增一個 HTML 檔案來顯示延遲的內容
    } else {
        ("HTTP/1.1 404 NOT FOUND", Path::new("./src/HTML/404.html"))
    };
    // 省略...
}
```

這邊我們新增了一個 `/delay` 的路徑，當我們輸入 `http://127.0.0.1:3000/delay` 時，會延遲個 10 秒，然後回傳延遲的內容。
而在這一頁重整的話，也一樣會發現我們的網頁會一直在等待，直到 10 秒後才會回傳內容。

然後我們另外開一個分頁，這裡我們輸入 `http://127.0.0.1:3000/`，然後會正常無延遲顯示 `hello.html` 的內容。
這時候我們來做一個有趣的事情，我們在 `/delay` 的分頁重整，然後在 `/` 的分頁重整，我們會發現 `/` 的分頁也會等待 10 秒，然後才會回傳內容。

這就是我們要解決的問題，我們的伺服器只有一個執行緒，所以當有一個請求進來時，其他的請求都會等待，直到這個請求處理完畢。

## 建立多執行緒伺服器

### 使用 thread::spawn

這邊我們可以先使用 `thread::spawn` 來建立多執行緒，讓我們的伺服器可以同時處理多個請求。

```rust
// 省略...
use std::thread; // 引入 thread 模組

fn main() {
    let listener = TcpListener::bind(format!("127.0.0.1:{}", PORT)).unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        thread::spawn(|| handle_connection(stream)); // 這邊我們使用 thread::spawn 來建立多執行緒
    }
}

// 省略...
```

這裡的作法是，因為我們的 HTTP 請求都寫在 `handle_connection` 這個函式裡面，所以我們可以直接將 `handle_connection` 這個函式傳入 `thread::spawn` 來建立多執行緒。這樣每次建立一個新的 TCP 連線時，我們就會建立一個新的執行緒來處理這個請求。

然後我們來測試一下剛剛的情況，現在一樣開兩個分頁，這時候等待 10 秒結束 `/delay` 的分頁一樣會等待 10 秒才 loading 完畢，但是 `/` 的分頁不會等待，因為我們的伺服器已經可以同時處理多個請求了。

不過雖然現在的作法可行，但是由於每次建立一個新的 TCP 連線時，我們就會建立一個新的執行緒來處理這個請求，所以如果有大量的請求進來，例如 DDOS 攻擊我們的伺服器，這樣就會瞬間建立大量的執行緒，會造成效能上的問題。

而解決的方法有很多種，這邊我們選擇使用執行緒池（thread pool）的方式來解決。

但在開始之前，我們先來看一下執行緒池的概念。

### 什麼是執行緒池？

簡單說明一下，執行緒池是執行緒的一種執行模式，執行緒池的概念就是我們先建立一個執行緒池，然後把固定數量的執行緒放進去。當有請求進來時，我們就從執行緒池中取出一個執行緒來處理請求，這時候其他的執行緒也可以處理其他的請求，而當執行緒請求處理完畢後，執行緒會回到執行緒池中等待新的請求。

執行緒池的好處就是我們可以讓多個執行緒同時處理請求，而不會因為單一執行緒的問題而造成等待，並且可以控制執行緒的數量，避免大量的執行緒造成效能上的問題。

那麼了解了執行緒池的概念後，我們就可以來實際操作藉由執行緒池來建立多執行緒的伺服器了。

### 實作

<!-- 首先我們先來建立一個執行緒池，我們可以使用 `threadpool` 這個 crate 來建立執行緒池，這個 crate 的使用方法很簡單，只要在 `Cargo.toml` 中加入 `threadpool = "1.8.1"` 這一行，然後在 `main.rs` 中加入 `extern crate threadpool;` 即可。 -->

首先我們可以先在 `src` 的目錄下新增一個 `lib.rs` 的檔案，接著來寫執行緒池的程式碼：

```rust
// src/lib.rs
pub struct ThreadPool;

impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);

        ThreadPool
    }

    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
    }
}
```

這邊我們先建立一個 `ThreadPool` 的結構，然後在 `ThreadPool` 的結構中實作一個 `new` 方法，這個方法會接收一個 `usize` 的參數，代表我們要建立幾個執行緒，然後在 `new` 方法中我們會先使用 `assert!` 來確認我們傳入的參數是否大於 0，如果不是的話就會 panic 掉，然後我們會回傳一個 `ThreadPool` 的結構。

接著我們想要在 `ThreadPool` 的結構中要有一個類似 `thread::spawn` 的方法，可以從參數傳入要在新執行緒開啟的閉包，他的文件是這樣：

```rust
pub fn spawn<F, T>(f: F) -> JoinHandle<T>
    where
        F: FnOnce() -> T,
        F: Send + 'static,
        T: Send + 'static,
```

所以我們在 `ThreadPool` 的結構中實作一個 `execute` 方法，這個方法會接收一個泛型 `F`，這個泛型 `F` 會是一個閉包，而這個閉包的型別會是 `FnOnce()`，也就是說這個閉包不會接收任何參數，也不會回傳任何值，而且這個閉包只會執行一次。而這個閉包的型別也會是 `Send`，也就是說這個閉包可以在多執行緒中安全的傳遞，而且這個閉包的型別也會是 `'static`，也就是說這個閉包的生命週期會是整個程式的生命週期。

今天先做到這邊，我們明天再繼續接著做。