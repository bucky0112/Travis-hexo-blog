---
title: rust-day-23-build-a-http-server-part-3
tags:
---

昨天我們在處理回傳錯誤的部分還沒有完成，今天將繼續接著做。

```rust
// ...以上省略
use std::str;
use std::str::Utf8Error;

pub struct Request {
    path: String,
    query_string: Option<String>,
    method: Method,
}

impl TryFrom<&[u8]> for Request {
    type Error = ParseError;

    fn try_from(buf: &[u8]) -> Result<Self, Self::Error> {
        let request = str::from_utf8(buf)?;
        unimplemented!()
    }
}

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

impl From<Utf8Error> for ParseError {
    fn from(_: Utf8Error) -> Self {
        Self::InvalidEncoding
    }
}

// ...以下省略
```

上面的程式碼的意思是，我們從 `src/http/request.rs` 中 import `Request`，然後從 `std::convert::TryFrom` 中 import `TryFrom`，接著我們在 `match Request::try_from(&buffer[..])` 這邊，我們會嘗試將 `buffer` 轉換成 `Request`，如果成功的話，就會回傳 `Ok`，並且會回傳一個 `Request` 型態的值，如果失敗的話，就會回傳 `Err`，並且回傳一個 `String` 型態的值。

