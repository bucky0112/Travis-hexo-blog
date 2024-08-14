---
title: 用 Rust 寫一個網頁爬蟲
date: 2024-08-12 01:16:31
tags:
 - rust
categories: Rust
keywords:
 - rust
 - web scraping
decription: 用 Rust 寫一個網頁爬蟲
---
通常要使用網頁爬蟲，會想到用 Python 來寫，但其實蠻多程式語言都可以做到，蠻好奇如果用 Rust 來寫的話怎麼樣，所以這一篇文章會示範如何用 Rust 來寫網頁爬蟲。

## 建立 Rust 專案

第一步我們會先使用 Cargo 來建立一個新的 Rust 專案，這邊我們會使用 `cargo new` 來建立一個新的專案，專案名稱為 `rust_web_scraping`。

```bash
cargo new rust_web_scraping
```

## 安裝相關的套件

接下來需要移動到專案的目錄下，並且安裝相關的套件：

```bash
cargo add tokio -F full
```

`tokio` 是 Rust 中用來實現非同步的處理，我們還要安裝 `reqwest` 和 `scraper` 這兩個套件，這兩個套件分別用來發送 HTTP 請求和解析 HTML。

```bash
cargo add reqwest
cargo add scraper
```

目前的 `Cargo.toml` 應該會長這樣：

```toml
[dependencies]
reqwest = "0.12.5"
scraper = "0.20.0"
tokio = { version = "1.39.2", features = ["full"] }
```

## 實作網頁爬蟲

### 引入相關的套件

首先我們會先引入相關的套件：

```rust
use reqwest::Client;
use scraper::{Html, Selector};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // code
}
```

使用 `reqwest::Client` 來發送 HTTP 請求。而 `scraper::{Html, Selector}` 則是幫助解析 HTML 和選擇元素，它提供了一種類似 CSS 選擇器的方式來取得資料。

`#[tokio::main]` 和 `async fn main()` 讓我們的 `main()` 變成非同步，這一步驟是必要，如果沒這個步驟，接下來都不用玩了。

然後 return `Result<(), Box<dyn std::error::Error>>`，這樣就可以使用 `?` 來進行錯誤處理。

### 使用 Client 發送 HTTP 請求

接下來我們會使用 `reqwest::Client` 來發送 HTTP 請求，這邊我們會使用 `get` 方法來發送 GET 請求，並且將回應的內容存到 `res` 變數中。

我們要連結的網址是 `https://books.toscrape.com/`，這個網站主要是一個可以練習網頁爬蟲的網站，裡面有很多書籍的資料，我們可以從這個網站來練習網頁爬蟲。

當然也可以去爬其他網站，只是本篇是以教學為目的，所以就以這個網站來做示範。

![20240812124025](https://i.imgur.com/w5DiCVA.png)

```rust
let client = Client::new();
let res = client
    .get("https://books.toscrape.com/")
    .send()
    .await?;

let html_content = res.text().await?;
let document = Html::parse_document(&html_content);
```

先使用 `Client::new()` 來建立一個新的 `Client`，然後使用 `get` 方法來發送 GET 請求，並且使用 `send` 方法來發送請求，最後使用 `await?` 來等待回應。

接著使用 `res.text().await?` 來取得回應的內容，然後使用 `Html::parse_document` 來解析 HTML 內容。

### 選擇元素

然後這一步驟需要看網頁的 HTML 架構，例如我們預計要爬蟲的這個網站的 HTML 架構是這樣的：

![20240812215834](https://i.imgur.com/xRRdENB.png)

看起來每一本書都是一個 `<li>` 元素，我們再往下一層看：

![20240812220525](https://i.imgur.com/zIoTGvT.png)

到這一層似乎就可以看到我們想要的東西了，看起來這裡就可以找到圖片、書名、價格等資訊。

```rust
let product_selector = Selector::parse("article.product_pod").unwrap();
let products = document.select(&product_selector);

for product in products {
    // 處理每一本書的步驟
}
```

這邊使用 `Selector::parse("article.product_pod").unwrap()` 來選擇所有的 `<article class="product_pod">` 元素，然後使用 `document.select(&product_selector)` 來取得頁面上的每本書，會使用 `for` 迴圈來處理。

### 取得每本書的資料

接下來我們在迴圈裡面要取得每本書的資料，這邊我們會取得每本書的圖片、書名、價格等資訊。

```rust
let image_url = product
    .select(&Selector::parse("img.thumbnail").unwrap())
    .next()
    .and_then(|img| img.value().attr("src"))
    .map(|src| format!("https://books.toscrape.com/{}", src));

let product_price = product
    .select(&Selector::parse(".price_color").unwrap())
    .next()
    .map(|p| p.text().collect::<String>());

let title = product
    .select(&Selector::parse("h3 a").unwrap())
    .next()
    .map(|a| a.text().collect::<String>());
```

以圖片為例，會先使用 `product.select(&Selector::parse("img.thumbnail").unwrap())` 來選擇每本書的圖片，然後使用 `next()` 來取得第一個元素，再使用 `and_then(|img| img.value().attr("src"))` 來取得圖片的 `src` 屬性，最後使用 `map(|src| format!("https://books.toscrape.com/{}", src))` 來組合完整的圖片網址。

剩下的步驟也是類似的，只是選擇的元素不同，例如價格是選擇 `.price_color` 這個 class，書名是選擇 `h3 a` 這個元素。

最後再印出來看看：

```rust
println!("標題: {:?}", title);
println!("圖片URL: {:?}", image_url);
println!("價格: {:?}", product_price);
```

會看到下圖中的結果：

![20240812223114](https://i.imgur.com/8LLE3T4.png)

你應該會感到奇怪，為什麼會有 `Some` 的字出現？

### 解決 `Some` 的問題

這是因為在 Rust 中，`Option` 會用來表示一個值可能存在或不存在，當值存在時，會是 `Some`，當值不存在時，會是 `None`。

而在目前的程式碼中，使用了很多可能失敗的操作，例如：

```rust
let image_url = product
    .select(&Selector::parse("img.thumbnail").unwrap())
    .next()
    .and_then(|img| img.value().attr("src"))
    .map(|src| format!("https://books.toscrape.com/{}", src));
```

在 `.next()` 回傳 `Option<Element>`，因為 Selector 可能找不到任何元素，然後 `.map()` 保留了 `Option` 的結果，所以會有 `Some` 的字出現。

解決的方法很多，這邊我們使用 `unwrap_or`，這樣就可以避免出現 `Some` 的字。

```rust
let title = product
    .select(&Selector::parse("h3 a").unwrap())
    .next()
    .map(|a| a.text().collect::<String>())
    .unwrap_or_else(|| String::from("未知標題"));
```

這樣就不會有 `Some` 的字出現了。

![20240813012215](https://i.imgur.com/zIpsof4.png)

附上 repo 的[連結](https://github.com/bucky0112/rust_scraping)，有興趣可以參考一下。