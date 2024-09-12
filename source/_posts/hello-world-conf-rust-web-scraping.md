---
title: Rust 從零開始網頁爬蟲(2024 Hello World)
date: 2024-09-13 00:43:33
tags:
 - rust
categories: Rust
keywords:
 - rust
 - web scraping
 - Hello World Dev Conference
 - 2024
decription:
---
本篇文章是 2024 Hello World Dev Conference 工作坊的實作流程，可以搭配 [Github repo](https://github.com/bucky0112/scraping_rust) 一起使用。

## Rust 簡介
Rust 是一個近年來引起非常多討論的程式語言，因為效能出色，並且安全性極佳，寫起來非常有趣，在每一年的 Stackoverflow 調查中已經連續蟬聯多年的最受歡迎程式語言（most-admired programming language），可以參考今年 2024 的[調查](https://survey.stackoverflow.co/2024/technology/#admired-and-desired)。也因為討論度很高，並且很多國際知名企業也紛紛投入使用，可以感受到似乎引領了一股風潮，好像什麼都要用 Rust 來改寫原本的產品，或者是出現以 Rust 開發的新東西。

## 安裝 Rust
要安裝 Rust 非常簡單，只要進到[官網](https://www.rust-lang.org/zh-TW/)，然後按下“立即開始”的按鈕，就會偵測你目前電腦的系統，並給你安裝的建議與說明。像是我的電腦是 macOS 系統，所以只要按照指示，在終端機執行指令，就可以開始安裝。

安裝 Rust 這個步驟主要會安裝兩個東西，分別是：

- Rustup
- Cargo

Rustup 是 Rust 的安裝與版本管理工具，而 Cargo 則是 Rust 的建置與套件管理工具。

如果想確認是否安裝成功，可以在終端機下指令：

```bash!
rustc -V
```

或者是：

```bash!
cargo -V
```

這兩個都能檢查目前安裝的版本（截至目前為止的版本為 1.81.0），例如下圖：

![20240913005124](https://i.imgur.com/3tIP2Jq.png)

## Hello World，建立專案

既然安裝好了，不免俗的讓我們來做一個 Hello World，這裡會直接使用 Cargo 來建立一個 Rust 專案，請在終端機執行：

```bash!
cargo new demo
```

這段指令代表要使用 Cargo 來建立一個名稱 `demo` 的專案，當然名稱可以自由命名。然後進入到該專案之後，並使用習慣的編輯器打開，例如 VSCode。

### 建議 VSCode 套件

如果是習慣用 VSCode 的人，強烈建議 [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) 這個套件。在開發中可以有一些提示，或者是錯誤的提醒。

現在你應該使用編輯器打開專案了，並且可以看到專案的架構：

```
.
├── src
│   └── main.rs
├── target
├── .gitignore
├── Cargo.lock
└── Cargo.toml
```

整個專案最主要的檔案是在 `src` 底下的 `main.rs`，它是 Rust 專案的 entrypoint，任何邏輯都會經由這裡來發動執行，那麼我們來看看 `main.rs` 這個檔案。

```rust!
fn main() {
    println!("Hello, world!");
}
```

可以看到建立專案已經幫我們處理好 Hello World 了，但是這個 `println!()` 是什麼？

以目的來說，他可以幫我們印出 Hello World，雖然看起來很像是一個函式，但在 Rust 中其實不是函式，而是一個巨集（Marco），可以先大概了解就可以。

### 執行專案

在 Cargo 專案中，要執行並且運作程式碼，可以在終端機下指令：

```bash!
cargo run
```

這一段指令會編譯並且執行目前的程式碼，所以可以在終端機看到 `Hello World` 的結果。

## 開始爬蟲

在開始之前，需要先安裝這個專案需要用的套件。

### 套件的搜尋

如果是熟悉網頁開發的朋友，大概都會知道有 NPM 這個東西，而 Rust 也有一個類似 NPM 的網站，那就是 [crates.io](https://crates.io/)

![20240913005147](https://i.imgur.com/w0EM3SF.png)

我們需要安裝這些套件：

- [tokio](https://crates.io/crates/tokio)
- [reqwest](https://crates.io/crates/reqwest)
- [scraper](https://crates.io/crates/scraper)

tokio 主要是處理非同步的操作；reqwest 是處理 HTTP 請求；scraper 則是處理網頁元素的選擇。

### 套件安裝的方式

套件安裝的方式有分成在終端機下指令安裝，和在專案中的 `Cargo.toml` 手動輸入。以 tokio 為例，可以在右邊的 **Install** 區塊看到安裝方式。

![20240913005454](https://i.imgur.com/jQNPm5Q.png)

第一種方式是在終端機下指令：

```bash!
cargo add tokio
```

第二種方式可以打開 `Cargo.toml` 這個檔案，然後在 `[dependencies]` 底下輸入：

```toml!
tokio = "1.40.0"
```

### 安裝 tokio 需要注意的地方

由於 crates 大都預設是基本的安裝，所以當需要開啟某些功能，還需要開啟設定，安裝的方式可以這樣做：

第一種在終端機下指令：

```bash!
cargo add tokio -F full
```

第二種方式也可以透過 `Cargo.toml` 來安裝：

```toml!
tokio = { version = "1.40.0", features = ["full"] }
```

如果是經由 `Cargo.toml` 來安裝的話，還需要在終端機下指令來進行編譯：

```bash!
cargo build
```

目前的 `Cargo.toml` 應該會是這樣：

![20240913005210](https://i.imgur.com/Oi9WD2y.png)

## 要進行爬蟲的資料來源

這次工作坊要進行的爬蟲練習來源是[這個網站](https://books.toscrape.com/)。這是一個專門練習爬蟲的網站。

![20240913005230](https://i.imgur.com/9LeonzC.png)

## 試著取得網頁內容

接下來就可以寫一些程式碼，先試著連到這個網站並取得網頁內容：

```rust!
use reqwest;       // 使用 reqwest

#[tokio::main]     // 在 main 標記使用 tokio 處理非同步
async fn main() {  // 在 main 前面加上 async
    let url = "https://books.toscrape.com/"; // 連結的 URL
    let response = reqwest::get(url).await.unwrap(); // 使用 get
    let body = response.text().await.unwrap(); // 將 response 轉成可辨識的文字

    println!("{}", body); // 印出 body
}
```

然後執行 `cargo run`，應該會看到印出整個網頁的內容。

## 解析 HTML

確認網頁內容沒問題之後，就可以進行下一步來解析 HTML，這裡需要借助 scraper。

```rust!
use scraper::Html; // 使用 scraper 的 HTML 方法

// ...

let document = Html::parse_document(&body); // 解析 body

println!("{}", document);
```

這時候執行 `cargo run` 之後，會發現到有錯誤。

![20240913005245](https://i.imgur.com/3G01s3z.png)

Cargo 會提供完整的錯誤提示，甚至會跟你說你應該怎麼做比較好，所以就可以按照它的建議在 `println!()` 中的 `{}` 改成 `{:?}` 或者是 `{:#?}`。

可以改成以下幾種方式：

```rust!
println!("{:?}", document); // debug 格式化

println!("{:#?}", document); // pretty-print 格式

println!("{}", document.html()); // 轉回字符串，就是轉回原來的格式
```

## 決定抓取哪些資料

我們主要是要抓每本書的名稱以及價錢，打開開發者工具檢查 HTML 架構，可以發現這裡是用 `<li>` 來區分每本書，但我們需要更方便辨識的方法，也就是按照 `<li>` 中的 `<article>`，這裡有 class 可以辨識，也就是 `product_pod`，這樣就可以去選取每一本獨立的書。

當選擇完每本書之後，就要去抓取書的書名以及價錢，可以在 `<h3>` 和 `.product_price`。

![20240913005302](https://i.imgur.com/9d2KXjp.png)

## 建立選取器

```rust!
use scraper::{Html, Selector};
// ...
let book_selector = Selector::parse("article.product_pod").unwrap();
let title_selector = Selector::parse("h3 a").unwrap();
let price_selector = Selector::parse("div.product_price .price_color").unwrap();
```

## 使用迴圈撈取資料

接下來就可以使用迴圈來撈取資料：

```rust!
for book in document.select(&book_selector) {
    let title = book
        .select(&title_selector)
        .next()
        .unwrap()
        .text()
        .collect::<String>();
    let price = book
        .select(&price_selector)
        .next()
        .unwrap()
        .text()
        .collect::<String>();

    println!("書名: {}", title);
    println!("價格: {}", price);
    println!("---");
}
```

執行 `cargo run` 之後，這樣就可以把每本書的書名以及價格全部印出來。

## 利用屬性取得完整書名

執行完之後雖然可以取得書名以及價格，但是應該可以發現有些書名並不是完整的書名。

![20240913005317](https://i.imgur.com/U09lOci.png)

這是因為網站本身在處理書名時，可能是針對太長的書名有進行修改。沒關係，可以看一下 HTML 架構中是否有提供完整的書名。

![20240913005328](https://i.imgur.com/rYQZwJE.png)

我們發現在 `<h3>` 中的 `<a>` 有一個 `title` 的屬性，所以就可以利用這個屬性去取得完整的書名。

```rust!
let title_element = book.select(&title_selector).next().unwrap();
        // 使用 title 屬性獲取完整書名
let title = title_element.value().attr("title").unwrap_or("Unknown Title");
```

現在再次執行 `cargo run` 就可以取得完整的書名了。

## 錯誤處理

如果看現在的程式碼，應該可以發現有大量的 `unwrap()`。其實這個是錯誤處理的方式之一，但是並不能算是非常好的作法，因為當有碰到錯誤的時候，在 `unwrap()` 這裡就會立即終止所有程式，不會再繼續往下走了，如果是拿來開發初期可能很好用，但是還是要預想遇到錯誤會怎麼繼續往下跑你的程式碼。

這裡就會用到 Rust 本身自帶的工具，從 `std` 也就是 Rust 的 standard library 中使用 `Error`：

```rust!
use std::error::Error;
```

那麼要怎麼使用呢？

首先會用到 `Result` 這個 enum，它是一個這樣的結構：

```rust!
pub enum Result<T, E> {
    Ok( /* … */ ),
    Err( /* … */ ),
}
```

原本應該也要使用關鍵字 `use` 來使用：

```rust!
use std::result::Result;
```

不過可能因為 `Result` 是比較常用，所以就不需要特別使用 `use` 來引入，而是可以直接用。

```rust!
async fn main() -> Result<(), Box<dyn Error>> {
    // ...
    Ok(())
}
```

由於 `Result` 是一個 enum，只有兩條路，不是正確就是錯誤，所以在 `main` 這裡設置 `Result` 也就代表了要回傳東西，我們在最後面擺上一個 `Ok(())` 就好。

### 取代 unwrap()

接下來其他的錯誤就可以把所有的 `unwrap()` 都拿掉，但是選取 HTML 的部分如果壞掉，好像也不需要繼續往下執行，所以替換成 `?` 表示暫不處理。

```rust!
let response = reqwest::get(url).await?;
let body = response.text().await?;
let document = Html::parse_document(&body);

let book_selector = Selector::parse("article.product_pod")?;
let title_selector = Selector::parse("h3 a")?;
let price_selector = Selector::parse("div.product_price .price_color")?;
```

### 可以加上失敗後顯示的文字

而在迴圈的部分如果找不到相對應的元素就再提示並顯示錯誤訊息。

```rust!
for book in document.select(&book_selector) {
    let title_element = book.select(&title_selector).next().ok_or("找不到 Title 元素")?;
    let title = title_element
        .value()
        .attr("title")
        .ok_or("找不到 Title 屬性")?;
    let price = book
        .select(&price_selector)
        .next()
        .ok_or("找不到 Price 元素")?
        .text()
        .collect::<String>();

        // ...
}
```

## 拿取其他頁面的資料

由於最下面有分頁，代表其他頁面也有資料還沒撈取，而目前只有爬第一頁的資料，所以我們接下來試著爬完全部頁面的資料。

我們可以先觀察第一頁跟其他頁的差異在哪。

其實整個 HTML 架構都差不多，最大的差別只在於 URL 而已，例如第一頁是 `https://books.toscrape.com/`，而第二頁則是 `https://books.toscrape.com/catalogue/page-2.html`，剩下幾頁的差別都只是在 page 之後的數字差別而已，接下來比較方便的作法則是建立一個 client。

## 建立 client 並測試是否可以爬到第二頁

這一步驟要先使用 `reqwest` 建立一個 client，比較方便去 get 不同頁面的 URL。下面的程式碼會用迴圈先試跑個兩圈，看看兩頁是不是都能成功連到，然後印出爬到的頁面跟狀態碼是否正確還有符合我們想要的。

```rust!
async fn main() -> Result<(), Box<dyn Error>> {
    let client = reqwest::Client::builder().build()?;

    for page in 1..=2 {
        let url = if page == 1 {
            "https://books.toscrape.com".to_string()
        } else {
            format!("https://books.toscrape.com/catalogue/page-{}.html", page)
        };

        println!("正在爬取頁面: {}", url);

        let response = client.get(&url).send().await?;
        println!("狀態: {}", response.status());

        if !response.status().is_success() {
            println!("狀態碼: {}", response.status());
            continue;
        }
    }

    Ok(())
}
```

這裡可以試試看把網頁改成錯誤的會發生什麼事，例如把 URL 故意打錯。

![20240913005349](https://i.imgur.com/EbXBMgj.png)

到這一步應該都能成功讓兩個頁面的狀態都是 200，所以就可以繼續往下進行。

## 計算每頁抓到的書是否正確

如果檢查每一頁應該可以發現每一頁的書都是固定的 20 本，所以檢查一下用選取器在第二頁是否也正常。

```rust!
for page in 1..=2 {
    // 省略
    let body = response.text().await?;
    let document = Html::parse_document(&body);
    let book_selector = Selector::parse("article.product_pod")?;
    // 省略

    let mut book_count = 0;

    for _book in document.select(&book_selector) {
        // 省略
        book_count += 1;
    }

    println!("在第 {} 頁找到 {} 本書", page, book_count);
}
```

`book_count` 是一個會一直往上加的值，代表這個值需要一直改變，並且最後會使用這個改變後的變數，所以要使用 `mut` 這個關鍵字來表示這個變數是可變。

然後 for 迴圈的 `book` 沒有加上 `_` 的話，或者應該說在 Rust 的程式碼如果有定義但沒使用，在終端機應該可以看到一些警告，這不是錯誤，只是 Rust 會很 nice 地（雞婆）提醒您有變數沒使用，所以可以註解掉該變數，或者在變數前面加上 `_`，表示目前暫時用不到。

當然如果你不介意這些警告，也可以不用理他。

執行 `cargo run` 之後應該會看到兩頁都能撈到 20 本書，所以就可以試著跑完全部的頁面，只要把迴圈的數字改成最大頁數就好，以這個網站來看，改成 50 之後跑看看，觀察一下每一頁狀態是不是正常，找到的書本是不是正確的數字。

![20240913005408](https://i.imgur.com/HS1TzKe.png)

## 如果超過頁面會怎麼樣？

我們知道現在總共有 50 頁，但如果想要看看跑超過 50 頁會怎麼樣，我們來試試看，改成 51。

其實就跟前面故意把 URL 改成錯誤一樣，在第一個迴圈就會出現錯誤，然後按照我們設想的錯誤並回傳錯誤訊息，所以使用正確的錯誤處理是非常好的一件事，這裡如果是用 `unwrap()` 雖然也可能會回傳錯誤訊息，但這種錯誤是我們可以掌握並處理的。

## 儲存完整資料 -> JSON

其實現在跑下來是可以在終端機顯示全部並且完整的書名以及價錢，但 50 頁的資料量滿多的，而且也不好閱讀，所以讓我們試著把資料存成 JSON。

### 安裝 crates 來處理 JSON

我們需要兩種 crates 來處理 JSON，這裡推薦：

- [serde](https://crates.io/crates/serde)
- [serde_json](https://crates.io/crates/serde_json)

安裝 serde 需要開啟完整的功能，所以在安裝可以這樣：

```bash!
cargo add serde -F derive
```

serde 會按照我們設定的 struct 去解析資料，然後再經由 serde_json 轉出 JSON。

![20240913005422](https://i.imgur.com/F33sAl4.png)

struct 是 Rust 中定義型別的一種方式，可以想像成類似 TS 的 interface。

### 使用 serde 以及 File 並定義 struct

這一步需要使用剛剛安裝的 serde 然後還需要使用 Rust 內建的 File 來轉出檔案，然後還需要定義 JSON 的格式，也就是 struct，然後使用 serde 來標記，最後才能給 serde_json 使用。

```rust!
use serde::{Deserialize, Serialize};
use std::fs::File;

#[derive(Serialize, Deserialize)]
struct Book {
    title: String,
    price: String,
}
```

### 建立一個 Vec

建立完 JSON 的格式之後，由於 JSON 是由一個陣列包著物件這樣的格式，所以我們還需要建立一個陣列來使用。

不過在 Rust 的世界中，陣列（array）只能是一個固定長度，並且有長度限制，所以除非你確切知道你的資料長度，所以一般動態資料會使用向量 (vector)。

```rust!
let client = reqwest::Client::builder().build()?;
let mut books = Vec::new();
```

### 把每本書塞進 JSON

接下來就可以在 `book_selector` 迴圈中把每本書都塞進 `books` 這個 vector 當中。

```rust!
 for book in document.select(&book_selector) {
    book_count += 1;
    let title_element = book.select(&title_selector).next().unwrap();
    let title = title_element
        .value()
        .attr("title")
        .ok_or("找不到 Title 屬性")?;
    let price = book
        .select(&price_selector)
        .next()
        .ok_or("找不到 Price 元素")?
        .text()
        .collect::<String>();

    books.push(Book {
        title: title.to_string(),
        price,
    });

    println!("書名: {}", title);
    // println!("價格: {}", price);
    println!("---");
}
```

然後使用 serde_json 和 File 將資料寫成一個 json 檔案。

```rust!
let file = File::create("books.json")?;
serde_json::to_writer_pretty(file, &books)?;

println!("資料已存到 books.json");

Ok(())
```

然後你可能會覺得有點奇怪，為什麼將 `title` 跟 `price` 塞入的時候，`price` 並沒有像 `title` 那樣轉成 string。

這是因為 `Book` 在 struct 中定義成 String，然後 `price` 最後有轉成 String，而 `title` 沒有，它的型別原本是 &str（字串切片）。所以你要在 push 的時候轉成 String 或者是這樣也可以：

```rust!
for book in document.select(&book_selector) {
    book_count += 1;
    let title_element = book.select(&title_selector).next().unwrap();
    let title = title_element
        .value()
        .attr("title")
        .ok_or("找不到 Title 屬性")?
        .to_string();
    let price = book
        .select(&price_selector)
        .next()
        .ok_or("找不到 Price 元素")?
        .text()
        .collect::<String>();

    books.push(Book {
        title,
        price,
    });
}
```

然後你就可以執行 `cargo run` 並且將資料存成 JSON 了。

## 如果想存成 Excel 可以嗎？

沒問題！Rust 的 crates 也可以找得到專門處理 excel 的套件，那就是 [xlsxwriter](https://crates.io/crates/xlsxwriter)。

### 安裝 xlsxwriter 並使用

按照說明安裝完之後並使用：

```rust!
use xlsxwriter::Workbook;
```

```rust!
let workbook = Workbook::new("books.xlsx")?;
let mut sheet = workbook.add_worksheet(None)?;

sheet.write_string(0, 0, "書名", None)?;
sheet.write_string(0, 1, "價格", None)?;

let mut row = 1;
```

變數 `row` 會按照資料筆數一直增加欄位，所以需要讓他是一個可變變數。
然後在迴圈中寫入：

```rust!
for book in document.select(&book_selector) {
    book_count += 1;
    let title_element = book.select(&title_selector).next().unwrap();
    let title = title_element
        .value()
        .attr("title")
        .ok_or("找不到 Title 屬性")?;
    let price = book
        .select(&price_selector)
        .next()
        .ok_or("找不到 Price 元素")?
        .text()
        .collect::<String>();

    sheet.write_string(row, 0, title, None)?;
    sheet.write_string(row, 1, &price, None)?;

    books.push(Book {
        title: title.to_string(),
        price,
    });

    row += 1;
}
```

你可能發現到 sheet 在寫入價格欄位時，`price` 寫成了 `&price`。
`&` 是 Rust 中所有權常用的一個借用概念。

如果拿掉的話，這裡 Cargo 會提示你使用 `&`，不然會出錯。

執行 `cargo run` 應該就可以將資料存成 excel。

## 自動取得全部頁面資料

目前已經可以爬完全部頁面的資料，不過如果未來網站資料變多的話，那麼是不是代表會超過 50 頁？
雖然是可以再手動修改頁數，但我們可以去偵測全部的總頁數，然後就可以自動爬完全部頁數的資料了。

我們可以透過一個自定義的 function 來做這件事。


```rust!
async fn get_total_pages(client: &reqwest::Client) -> Result<u32, Box<dyn Error>> {
    let url = "https://books.toscrape.com/index.html";
    let response = client.get(url).send().await?;
    let body = response.text().await?;
    let document = Html::parse_document(&body);

    let pager_selector = Selector::parse("ul.pager li.current")?;
    let pager_text = document
        .select(&pager_selector)
        .next()
        .ok_or("無法找到分頁資料")?
        .text()
        .collect::<String>();

    let total_pages = pager_text
        .split_whitespace()
        .last()
        .ok_or("無法取得總頁數")?
        .parse::<u32>()?;

    Ok(total_pages)
}
```

這裡會借用 `client` 來使用選取器，然後並透過分頁的部分去計算總頁數。

![20240913005439](https://i.imgur.com/Izj4Nyn.png)

這裡就可以取代掉原本的數字。

```rust!
    // 取得總頁數
let total_pages = get_total_pages(&client).await?;
println!("總頁數: {}", total_pages);
```

執行後，一樣可以正常寫入，但這樣的函式可以幫助我們動態去計算總頁數，然後取得全部的資料。