---
title: 來用 Rust 建立一個伺服器吧！Part II
date: 2022-10-08 00:44:10
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: 用 Rust 建立一個伺服器
---

昨天我們成功讀取了 request，今天就繼續接著做，首先來處理發送 response 的部分。

## 發送 response

我們可以利用 `TcpStream` 的 `write` 方法來發送 response：

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 512];

    let c = stream.read(&mut buffer).unwrap();

    println!("請求內容: {}", String::from_utf8_lossy(&buffer[..c]));

    let response = "HTTP/1.1 200 OK\r\n\r\n";
    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

這裡我們先宣告了一個 `response` 變數，裡面放的是一個 HTTP response 的格式，然後我們利用 `write` 方法來發送這個 response，這裡的 `as_bytes` 是把 `response` 轉成 byte array，因為 `write` 方法只接受 byte array。

而且 `write` 方法可能會失敗，所以我們要用 `unwrap` 來處理，如果失敗的話，程式就會 panic。最後呼叫 `flush` 來停止程式，直到所有的資料都被寫入。

然後再試著執行 `cargo run`，並打開瀏覽器，輸入 `127.0.0.1:3000`。這時候網頁不會像之前那樣顯示錯誤，而是會顯示一個空白頁面，這是因為我們還沒有回傳任何內容，所以瀏覽器就會顯示空白頁面。

## 回傳 HTML

接下來我們要回傳一個 HTML 格式的內容，可以讓瀏覽器顯示出來，可以先看以下的範例：

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    let c = stream.read(&mut buffer).unwrap();

    println!("請求內容: {}", String::from_utf8_lossy(&buffer[..c]));

    let response = format!(
        "HTTP/1.1 200 OK\r\nContent-Length: \r\n\r\n{}",
        "<!DOCTYPE html>
<html lang=\"en\">
    <head>
        <meta charset=\"utf-8\">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello Rust</h1>
        <p>成功顯示！</p>
    </body>
</html>
"
    );
    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

首先，這裡需要注意的是緩衝區的大小，我們把它改成 1024，因為我們要回傳的內容比較多，所以緩衝區的大小要比較大，如果沒有改的話，會沒辦法顯示內容。

然後我們再重新執行，並重整瀏覽器，這時候就可以看到我們回傳的 HTML 內容了。

不過，直接把 HTML 的內容寫在 `main` 函式裡面的話，程式碼會變得很雜亂，所以我們要把它移到另一個檔案裡面。

## 把 HTML 內容移到另一個檔案

首先，我們在專案的根目錄建立一個 `index.html` 檔案，把 HTML 的內容放進去：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello Rust</h1>
        <p>成功顯示！</p>
    </body>
</html>
```

然後我們要在 `main` 函式裡面讀取這個檔案，並回傳它的內容：

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    let c = stream.read(&mut buffer).unwrap();

    println!("請求內容: {}", String::from_utf8_lossy(&buffer[..c]));
    let contents = fs::read_to_string("hello.html").unwrap();

    let response = format!(
        "HTTP/1.1 200 OK\r\nContent-Length: {}\r\n\r\n{}",
        contents.len(),
        contents
    );
    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

然後重新再執行一次，這時候一樣可以看到內容，但我們的 `main` 函式就變得比較簡潔了。

## 整理一下資料夾

剛剛是把 `hello.html` 直接放在根目錄，但是這樣好像還是有點亂，所以我們在 `src` 中新增一個 HTML 的資料夾，把 `hello.html` 放進去，然後再修改一下路徑：

![CleanShot 2022-10-05 at 15.56.19](https://i.imgur.com/qGr5ywD.png)

接下來我們需要加入 `path` 的模組，來處理路徑：

```rust
use std::fs; // 這一行就可以刪掉了
use std::{fs, path::Path}; // 因為 fs 跟 path 都在 std 裡面，所以可以用逗號分開

// ...省略
fn handle_connection(mut stream: TcpStream) {
    // ...省略
    let path = Path::new("./src/HTML/hello.html"); // 新增一個 path 變數處理路徑
    let contents = fs::read_to_string(path).unwrap(); // 把路徑傳進去
```

這樣子一樣可以正常運作，而且我們的專案也整理的比較乾淨。

## 驗證請求選擇性回傳內容

雖然可以成功將內容顯示到畫面上，但是這裡有一個問題是，我們嘗試開啟 `http://127.0.0.1:3000/another` 這個網址時，也還是會回傳 `index.html` 的內容，這樣的話就不太好，所以我們要讓它可以驗證請求的內容，並選擇性的回傳內容。

首先，可以先寫一個判斷式，來判斷請求的內容：

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    stream.read(&mut buffer).unwrap();
    let get = b"GET / HTTP/1.1\r\n"; // 新增一個變數，用來判斷請求的內容

    if buffer.starts_with(get) { // starts_with 是用來判斷 buffer 的內容
        let path = Path::new("./src/HTML/hello.html");
        let contents = fs::read_to_string(path).unwrap();
        let response = format!(
            "HTTP/1.1 200 OK\r\nContent-Length: {}\r\n\r\n{}",
            contents.len(),
            contents
        );
        stream.write(response.as_bytes()).unwrap();
        stream.flush().unwrap();
    } else {
        println!("{}", "失敗");
    }
}
```

重新執行一次，並開啟瀏覽器。網址 `http://127.0.0.1:3000` 一樣可以正常顯示內容，但是如果連不同網址，例如 `http://127.0.0.1/another`，就會在終端機上印出 `失敗`，並且畫面不會顯示任何內容。

## 加上錯誤頁面

剛剛已經可以判斷請求內容，但總不可能要讓使用者都看終端機來判斷是否錯誤，所以現在要來加上錯誤的頁面方便使用者觀看。

首先，我們要新增一個 `404.html` 的檔案，並且在裡面寫上一些內容：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Hello!</title>
  </head>
  <body>
    <h1>404!</h1>
    <p>你走錯頁面囉！</p>
  </body>
</html>
```

接下來就在 `handle_connection` 的 `if...else` 裡面加上錯誤頁面的內容：

```rust
// ...省略
    } else {
        let path = Path::new("./src/HTML/404.html");
        let status_line = "HTTP/1.1 404 NOT FOUND";
        let contents = fs::read_to_string(path).unwrap();

        let response = format!(
            "{}\r\nContent-Length: {}\r\n\r\n{}",
            status_line,
            contents.len(),
            contents
        );

        stream.write(response.as_bytes()).unwrap();
        stream.flush().unwrap();
    }
}
```

接著一樣重新執行，然後開啟瀏覽器，輸入 `http://127.0.0.1:3000/404`，就可以看到我們新增的錯誤頁面了。

## 重構程式碼

由於剛剛做的 `if...else` 裡面有很多重複的程式碼，所以我們要來重構一下，讓程式碼更簡潔。

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    stream.read(&mut buffer).unwrap();
    let get = b"GET / HTTP/1.1\r\n";

    // 這裡用兩個變數來儲存狀態碼跟路徑
    let (status_line, path) = if buffer.starts_with(get) {
        ("HTTP/1.1 200 OK", Path::new("./src/HTML/hello.html"))
    } else {
        ("HTTP/1.1 404 NOT FOUND", Path::new("./src/HTML/404.html"))
    };

    let contents = fs::read_to_string(path).unwrap();
    let response = format!(
        "{}\r\nContent-Length: {}\r\n\r\n{}",
        status_line,
        contents.len(),
        contents
    );
    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

大概就是這樣，就可以讓程式碼更加簡潔，而且也能正常運作。
但是目前我們所建立的是單一執行緒的伺服器，這也代表目前一次只能處理一個請求，這在現代的伺服器處理上會很沒有效率，那麼我們在明天將會開始建立多執行緒的伺服器。