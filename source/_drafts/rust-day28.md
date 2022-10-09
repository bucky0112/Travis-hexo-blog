---
title: rust-day28
tags:
---

我們今天要來做 Auth，一個基本的身分驗證功能，不想要讓任何人都可以隨便使用我們的 API。

## 安裝套件

因為我們驗證的處理會使用到 base64 編碼，所以首先我們先在 `Cargo.toml` 加入 `base64` 這個 crate。

```toml
[dependencies]
base64 = "0.13"
```

由於目前 `main.rs` 中的 code 有點多，所以我們另外開一個檔案來處理。
直接在 `src` 資料夾下新增一個 `auth.rs` 檔案，並且在 `main.rs` 中加入 `mod auth;`。

在 `auth.rs` 中我們先來定義一個 `BasicAuth` 的 struct，並且在裡面定義一個使用者名稱和密碼的欄位。

```rust
pub struct BasicAuth {
    username: String,
    password: String,
}
```

```rust
use rocket::http::Status;
use rocket::request::{Request, FromRequest, Outcome};

pub struct BasicAuth {
    pub username: String,
    pub password: String,
}
impl BasicAuth {
    fn from_authorization_header(header: &str) -> Option<BasicAuth> {
        let split = header.split_whitespace().collect::<Vec<_>>();
        if split.len() != 2 {
            return None;
        }

        if split[0] != "Basic" {
            return None;
        }

        Self::from_base64_encoded(split[1])
    }

    fn from_base64_encoded(base64_string: &str) -> Option<BasicAuth> {
        let decoded = base64::decode(base64_string).ok()?;
        let decoded_str = String::from_utf8(decoded).ok()?;
        let split = decoded_str.split(":").collect::<Vec<_>>();

        // If exactly username & password pair are present
        if split.len() != 2 {
            return None;
        }

        let (username, password) = (split[0].to_string(), split[1].to_string());

        Some(BasicAuth {
            username,
            password
        })
    }
}

#[rocket::async_trait]
impl<'r> FromRequest<'r> for BasicAuth {
    type Error = ();

    async fn from_request(request: &'r Request<'_>) -> Outcome<Self, Self::Error> {
        let auth_header = request.headers().get_one("Authorization");
        if let Some(auth_header) = auth_header {
            if let Some(auth) = Self::from_authorization_header(auth_header) {
                return Outcome::Success(auth)
            }
        }
        
        Outcome::Failure((Status::Unauthorized, ()))
    }
}
```

接著我們定義了一個 `from_authorization_header` 的 function，這個 function 會接收一個 `header` 的參數，並且回傳一個 `Option<BasicAuth>`。

主要是在處理 `Authorization` 的 header，如果 header 的長度不是 2，或是 header 的第一個值不是 `Basic`，就會回傳 `None`。

如果 header 的長度是 2，且第一個值是 `Basic`，就會呼叫 `from_base64_encoded` 這個 function，並且將 header 的第二個值傳入。

然後 `from_base64_encoded` 主要是將傳入的值進行 base64 解碼，並且轉成 `String`，接著再將這個 `String` 以 `:` 進行分割，並且將分割後的值傳入 `BasicAuth` 的 struct 中。

最後我們定義了一個 `FromRequest` 的 trait，這個 trait 會在我們的 API 被呼叫時，會先進行驗證，如果驗證成功，就會將驗證的結果傳入 API 中。

在 `from_request` 中，我們先取得 `Authorization` 的 header，如果有的話，就會呼叫 `from_authorization_header` 這個 function，並且將 header 傳入。

如果驗證成功，就會回傳 `Outcome::Success(auth)`，如果驗證失敗，就會回傳 `Outcome::Failure((Status::Unauthorized, ()))`。

## 處理 API

接下來我們在 `main.rs` 中加入我們剛剛所定義的 `BasicAuth`。

```rust
mod auth;

use auth::BasicAuth;
```

然後先在 `get_demo` 這個 API 中加入 `BasicAuth` 的參數運作看看：

```rust
#[get("/demo")]
fn get_demo(_auth: BasicAuth) -> Value {
    json!([{ "id": 1, "name": "Bucky" }, { "id": 2, "name": "Tom" }])
}
```

執行完打開瀏覽器會發現頁面顯示 401 的錯誤，代表我們剛剛所做的驗證有開始運作，所以我們接著嘗試在 Postman 的 header 中加入 `Authorization`。

![CleanShot 2022-10-09 at 23.01.51@2x](https://i.imgur.com/y36n1kN.png)

我們將 `Authorization` 的值改成 `Basic YnVja3k6YnVja3k=`，這個值是 `bucky:bucky` 這個字串進行 base64 編碼後的結果，我們把這個值當成 token 來驗證之後就可以成功取得資料了。

接著就可以在所有的 methods 都加上 `BasicAuth`，這樣我們所有的 methods 都會進行驗證了。