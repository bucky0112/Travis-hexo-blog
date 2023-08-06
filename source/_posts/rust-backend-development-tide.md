---
title: Rust Web 之旅：新時代的後端開發
date: 2023-08-06 16:52:40
tags:
 - rust
categories:
 - Rust
keywords:
 - rust
 - backend
 - COSCUP
 - Tide
decription: 2023 COSCUP Rust 議程軌之一，介紹 Rust 在後端開發的應用，以 Tide 為例。
---

這篇文章是 2023 COSCUP Rust 議程軌之一，介紹 Rust 在後端開發的應用，以 Tide 為例。

Rust 已經連續多年成為 Stack Overflow 最受歡迎的程式語言，在 2023 年的調查也毫不意外的再度奪冠。

![Rust no1](https://i.imgur.com/ewN57VU.png)

許多公司紛紛將 Rust 應用在產品上，例如：Amazon、Meta、Microsoft、Google 這些大公司，並且帶起一股似乎什麼都可以用 Rust 來重新改寫的風潮。

![rewrite it in rust](https://i.imgur.com/xVLtmSH.png)

既然 Rust 這麼猛，那麼是不是也可以運用在網頁開發上呢？答案是肯定的，現在已經有許多的框架可以用來開發，想開發前端沒問題，後端框架更是如雨後春筍般的冒出來，例如：Rocket、Actix、Warp、Tide 等等。

## 從 JavaScript 的角度學習 Rust

身為網頁開發者，多多少少都會使用 JavaScript，所以從 JavaScript 的角度來學習 Rust 會比較容易理解。

### 宣告變數

```js
let x = 5; // mutable
const y = 10; // constant
```

```rust
let x = 5; // immutable
let mut y = 10; // mutable
const Z: i32 = 15; // constant
```

JavaScript 使用 `let` 宣告可變變數，而 `const` 用於宣告常數。
Rust 默認使用 `let` 宣告不可變變數，需要加 `mut` 關鍵字來宣告可變變數。`const` 在 Rust 中也用於宣告常數，但需要明確指定型別，並且變數名稱為大寫。

### 型別

```js
let a = "hello"; // string
let b = 5; // number
let c = 3.14; // number
let d = true; // boolean
```

```rust
let a: &str = "hello"; // 字串切片
let b: i32 = 5; // 數字 i32
let c: f64 = 3.14; // 浮點數 f64
let d: bool = true; // bool
```

JavaScript 是動態型別語言，所以它不需要在變數宣告時指定型別。
Rust 是靜態型別語言，通常會在變數宣告時指定型別，但 Rust 的型別推斷也很強大，可以在多數情況下自動判斷型別，所以以這個例子來說，Rust 的型別宣告可以省略。

### 函式

```js
function calculate(x, y) {
  return x + y;
}
```

```rust
fn calculate(x: i32, y: i32) -> i32 {
  x + y
}
```

在 JavaScript 中，使用 `function` 關鍵字定義函式。
在 Rust 中，使用 `fn` 關鍵字定義函式，並在參數和返回值時指定型別。
補充一下說明，Rust 的函式可以省略 `return`，最後一行的運算式會自動當作返回值，並且可以省略分號。

### if...else

```js
if (x > y) {
  console.log("x 大於 y");
} else {
  console.log("x 不大於 y");
}
```

```rust
if x > y {
  println!("x 大於 y");
} else {
  println!("x 不大於 y");
}
```

這兩種語言的 if...else 結構在語法上很相似，但 Rust 不需要括號來包裹條件語句。補充一下， JavaScript 的 `console.log` 在 Rust 中等同是 `println!`。

### 迴圈

```js
for (let i = 0; i < 10; i++) {
  console.log(`i is ${i}`);
}

while (x < 10) {
  console.log(`x is ${x}`);
  x+=1;
}
```

```rust
for i in 0..10 {
  println!("i is {}", i);
}

while x < 10 {
  println!("x is {}", x);
  x+=1;
}
```
兩種語言的迴圈語法也很相似，但在 Rust 中，`for in` 用於遍歷範圍或集合。補充一下，在 Rust 的 `println!` 中，`{}` 用於格式化輸出，`{}` 會被變數的值取代。所以這個 `for` 例子出來的結果會是 `i is 0` 到 `i is 9`。

### 錯誤處理

```js
function divide(x, y) {
  if (y === 0) {
    throw new Error("無法除以 0");
  }
  return x / y;
}

try {
  const result = divide(10, 0);
  console.log(result);
} catch (e) {
  console.log(e.message);
}
```

```rust
fn divide(x: i32, y: i32) -> Result<i32, String> {
  if y == 0 {
    return Err("無法除以 0".to_string());
  }
  Ok(x / y)
}

match divide(10, 0) {
  Ok(result) => println!("{}", result),
  Err(e) => println!("{}", e),
}
```

JavaScript 使用 `throw` 和 `try...catch` 來處理異常。
Rust 則使用 `Result` enum 來表示可能的錯誤，並使用 `match` 語句來處理這些結果。

### modules

```js
// calc.js
function calculate(x, y) {
  return x + y;
}

export default calculate;

// index.js
import calculate from "./calc.js";

console.log(calculate(5, 10));
```

```rust
// calc.rs
pub fn calculate(x: i32, y: i32) -> i32 {
  x + y
}

// main.rs
mod calc;
use calc::calculate;

fn main() {
  println!("{}", calculate(5, 10));
}
```

在 JavaScript 中，您可以使用 `export` 和 `import` 來管理模組。
Rust 使用 `mod` 關鍵字來聲明模組，並使用 `use` 關鍵字來引入它。公開的函式或結構需要使用 `pub` 關鍵字。

## 如何使用 Tide

首先建立一個新專案:

```bash
$ cargo new tide_crud
```

並且安裝相關的套件：

```
[package]
name = "tide_crud"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
tide = { version = "0.16.0" }
tokio = { version = "1.29.1", features = ["full"] } // 非同步處理
serde = { version = "1.0", features = ["derive"] } // 將資料轉換各種格式，例如 JSON、YAML、XML 等
```

安裝方法就是直接在 `Cargo.toml` 中 `dependencies` 底下加入 `tide`、`tokio` 和 `serde` 這三個套件，然後只要一執行 `cargo run` 就會自動安裝。

### Hello World

```rust
use tide::{Request, Result};

async fn hello(_req: Request<()>) -> Result<String> {
    Ok("Hello, world!".to_string())
}

#[tokio::main]

async fn main() -> Result<()> {
    let mut app = tide::new();
    app.at("/").get(hello);
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

接下來只要執行 `cargo run -q`，瀏覽器打開 `http://localhost:8080` 就可以看到 `Hello, world!`。

### 查詢參數

```rust
use tide::{Request, Result, new};

async fn handle_name(req: Request<()>) -> Result<String> {
    let name = req
        .url()
        .query_pairs()
        .find(|(key, _)| key == "name")
        .map(|(_, value)| value)
        .unwrap_or("world".into());

    Ok(format!("Hello, {}!", name))
}

#[tokio::main]
async fn main() -> Result<()> {
    let mut app = new();
    app.at("/").get(handle_name);
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

這個例子展示了如何從 URL 中獲取查詢參數。如果提供了 name 查詢參數，它將用於回應，否則預設為 "world"。

### JSON 處理

```rust
use tide::{Request, Result, new};

#[derive(serde::Deserialize)]
struct Rust {
    name: String,
    version: f32,
}

async fn create(mut req: Request<()>) -> Result<String> {
    let rust: Rust = req.body_json().await?;
    Ok(format!("Hello {}! Your Rust version is {}.", rust.name, rust.version))
}

#[tokio::main]
async fn main() -> Result<()> {
    let mut app = new();
    app.at("/rust").post(create);
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

這裡展示了如何使用 Tide 處理 JSON 資料。當你向 `/rust` 路徑發送 POST 請求並提供正確的 JSON 資料時，伺服器將回傳一個格式化的訊息。

這個例子，假如在 Postman 輸入以下資料：
```json
{
    "name": "Bucky",
    "version": 1.8
}
```

則會回傳：
```
Hello Bucky! Your Rust version is 1.8.
```

### 巢狀路由

```rust
use tide::{Request, Result, new};

#[tokio::main]
async fn main() -> Result<()> {
    let mut app = new();
    app.at("/rust").nest({
        let mut api = new();
        api.at("/").post(|_req: Request<()>| async move { Ok("Create") });
        api.at("/:id")
            .get(|_req: Request<()>| async move { Ok("Read") });
        api.at("/version").get(|_req: Request<()>| async move { Ok("1.0.0") });

        api
    });
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

這個例子展示了如何使用巢狀路由來組織你的應用程式。nest 方法允許你在一個給定的路徑下建立多個路由。

### status code

```rust
use tide::{new, Request, Response, Result, StatusCode};

async fn create(_req: Request<()>) -> Result {
    let res = Response::builder(StatusCode::Created)
        .body("Hello, world!")
        .build();
    Ok(res)
}

#[tokio::main]
async fn main() -> Result<()> {
    let mut app = new();
    app.at("/rust").post(create);
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

這個例子展示了如何使用 Tide 處理 status code。

如果要使用自定義的 status code，則在 `Response::new()` 時，參數傳入自定義的 status code 即可，例如：
```rust
let mut res = Response::new(200);
res.set_body("Hello, world!");
Ok(res)
```

### file server

```rust
use tide::{Result, new};

#[tokio::main]
async fn main() -> Result<()> {
    let mut app = new();
    app.at("/file.txt").serve_file("src/hello.txt")?;
    app.at("/test").serve_dir("./www")?;
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

這個例子展示了如何使用 Tide 來處理靜態檔案。

### logger

先安裝 `femme` 套件：

```toml
[dependencies]
femme = "2.2.1"
```

```rust
use tide::{Result, new, log};

#[tokio::main]
async fn main() -> Result<()> {
    femme::start();
    let mut app = new();
    app.with(log::LogMiddleware::new());
    app.at("/").get(|_| async {
        Ok("Hello, world!")
    });
    app.listen("0.0.0.0:8080").await?;
    Ok(())
}
```

透過 femme 在啟動伺服器後，每當有任何請求，在終端機中，就可以看到有關這次請求的日誌資訊。

### 建立 CRUD API

#### 建立 model

先新增一個 `models.rs` 檔案：

```rust
use std::collections::HashMap;

#[derive(serde::Deserialize, serde::Serialize)]
pub struct User {
    pub id: u32,
    pub name: String,
    pub email: String,
}

pub struct Repository {
    pub users: HashMap<u32, User>,
}

impl Repository {
  pub fn new() -> Self {
    Self {
      users: HashMap::new(),
    }
  }
}
```

在這個 `models.rs` 檔案中，定義了一個名為 `User` 的結構體，代表一個用戶，以及一個名為 `Repository` 的結構體，代表用戶的儲存庫。`Repository` 使用 `HashMap` 來儲存 `User` 實例，其中用戶的 ID 作為 key。

#### 建立 controller

再新增一個 `controller.rs` 檔案：

```rust
use crate::models::{Repository, User};
use tide::{Body, Request, Result, StatusCode};
use std::sync::Arc;
use tokio::sync::RwLock;

type State = Arc<RwLock<Repository>>;

pub async fn create(mut req: Request<State>) -> Result {
    let user: User = req.body_json().await?;
    let state = req.state();
    let mut repo = state.write().await;

    repo.users.insert(user.id, user);

    Ok(StatusCode::Ok.into())
}

pub async fn read(req: Request<State>) -> Result {
    let state = req.state();
    let repo = &state.read().await;

    Ok(Body::from_json(&repo.users)?.into())
}
```

在 `controller.rs` 檔案中，定義了幾個控制器函數，這些函數是用來處理 Web 請求的。這裡定義了 `create` 和 `read` 函數，分別用於新增和讀取。

- create：從請求的 JSON 主體中獲取用戶資料，然後添加到儲存庫中。
- read：從儲存庫中獲取所有用戶的資料，然後以 JSON 格式返回。

#### 在 main.rs 中使用 create & read

```rust
mod models;
mod controller;

use std::sync::Arc;
use tide::Result;
use tokio::sync::RwLock;
use models::Repository;
use controller::{create, read};

#[tokio::main]
async fn main() -> Result<()> {
  femme::start();
  let state = Arc::new(RwLock::new(Repository::new()));
  let mut app = tide::with_state(state);
  app.at("/users").post(create);
  app.at("/users").get(read);
  app.listen("0.0.0.0:8080").await?;
  Ok(())
}
```

在 `main.rs` 中，首先引入之前定義的模型和控制器，並建立了一個新的儲存庫並將其儲存在共享狀態中。然後定義了路由以處理使用者的新增與讀取操作。

#### update & delete

在 `controller.rs` 中新增以下程式碼：

```rust
pub async fn update(mut req: Request<State>) -> Result {
    let id: u32 = req.param("id")?.parse()?;
    let user: User = req.body_json().await?;
    let state = req.state();
    let mut repo = state.write().await;

    if repo.users.contains_key(&id) {
        repo.users.insert(id, user);
        Ok(StatusCode::Ok.into())
    } else {
        Ok(StatusCode::NotFound.into())
    }
}

pub async fn delete(req: Request<State>) -> Result {
    let id: u32 = req.param("id")?.parse()?;
    let state = req.state();
    let mut repo = state.write().await;

    if repo.users.contains_key(&id) {
        repo.users.remove(&id);
        Ok(StatusCode::Ok.into())
    } else {
        Ok(StatusCode::NotFound.into())
    }
}
```

在 `controller.rs` 添加兩個新的控制器函數：

- update：更新具有指定 ID 的用戶的資料。如果用戶存在，則更新其資料；否則，返回 404 Not Found。
- delete：刪除具有指定 ID 的用戶。如果用戶存在，則從儲存庫中刪除它；否則，返回 404 Not Found。

#### 在 main.rs 中使用 update & delete

```rust
use controller::{create, read, update, delete};

#[tokio::main]
async fn main() -> Result<()> {
  femme::start();
  let state = Arc::new(RwLock::new(Repository::new()));
  let mut app = tide::with_state(state);
  app.at("/users").post(create);
  app.at("/users").get(read);
  app.at("/users/:id").put(update);
  app.at("/users/:id").delete(delete);
  app.listen("0.0.0.0:8080").await?;
  Ok(())
}
```

最後在 `main.rs` 加入新的路由以使用上面定義的 `update` 和 `delete` 控制器函數。這樣就可以建立完一個完整的 CRUD API。

## 心得

感謝這次 COSCUP 所有的工作人員，沒有你們的協助，讓我有這次寶貴的機會並且順利完成這次分享，也要特別感謝 nukr 大大幫我主持，沒想過會這麼多人參加，謝謝大家的支持，希望大家都有獲得一些東西回去。(感謝 Patty 一路陪我並幫我拍照)

![20230806171203](https://i.imgur.com/JiNUdaE.jpg)

讓我炫耀一下被 Rust 大神 Luca Palmieri 發文的[推特](https://twitter.com/algo_luca/status/1685487697580969984)：
![20230806171749](https://i.imgur.com/si8mVlN.png)

最後有興趣的可以看一下當天的[簡報分享](https://gamma.app/public/Rust-Web--0a2uen43e3soifp)
