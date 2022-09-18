---
title: 用 Cargo 管理專案好棒棒
date: 2022-09-18 12:58:33
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: 專案太大怎麼管理？用 Cargo 就搞定！
---

昨天我們自己新增了一個副檔名為 `rs` 的檔案，然後在終端機執行 `rustc` 來編譯出結果。雖然這樣的作法沒什麼問題，但是只適合用在簡單的情況下。如果是一個龐大而且複雜的案例的話，顯然是沒辦法很有效地管理我們的專案，於是 Rust 也幫我們設想好，並且提供了 Cargo。

## 什麼是 Cargo

Cargo 可以做到套件管理、建構系統、跑測試，甚至是產生文件，Cargo 的角色就像是 JavaScript 的 NPM，或是 Python 的 Pip。

由於安裝 Rust 的時候，已經順便安裝了 Cargo，如果想確認有沒有安裝完成的話，可以在終端機輸入：

$ cargo --version

或是

$ cargo -V

正常顯示出版本號的話，就代表你的 Cargo 已經安裝好了。

## 建立專案

既然已經有了 Cargo，那麼接下來就來用 Cargo 來建立一個專案。
例如要建立一個名稱為 hello_world 專案的話請在終端機輸入：

```
$ cargo new hello_world
```

可以看到 Cargo 幫我們建立了一個 hello_world 專案，並且產生下圖中的檔案以及目錄。

![https://ithelp.ithome.com.tw/upload/images/20220918/20120293dWqiSba1l2.png](https://ithelp.ithome.com.tw/upload/images/20220918/20120293dWqiSba1l2.png)

可以看到 Cargo 幫我們建立了資料夾 src，還有一個 Cargo.toml 檔案，並且幫我們用 Git 來做版本控制。

資料夾 src 裡面有一個 main.rs 檔案，然後裡面理所當然地有一個主程式 `main()`，並且預設可以印出 "Hello, World"

![https://ithelp.ithome.com.tw/upload/images/20220918/20120293KG1JQRiPu1.png](https://ithelp.ithome.com.tw/upload/images/20220918/20120293KG1JQRiPu1.png)

而 Cargo.toml 是一個專案的設定檔，就像 NPM 的 package.json。

![https://ithelp.ithome.com.tw/upload/images/20220918/20120293EvWZHgTsAQ.png](https://ithelp.ithome.com.tw/upload/images/20220918/20120293EvWZHgTsAQ.png)

簡單介紹一下 Cargo.toml 裡面有什麼內容：

- [package] 代表一個區域，然後下面的內容是 Cargo 的一些設定。
- name 是我們用 Cargo 所建立這個專案的名稱。
- version 是這個專案的版本。
- edition 是目前所使用的 Rust 版本。
- [dependencies] 代表另一個區域，下面則是會列出這個專案所安裝的函式庫，Rust 稱其為 crates。

## cargo build

既然 Cargo 都已經幫我們建立並給了一些預設的程式碼，那麼我們就可以來試著印出 "Hello, World"。

在終端機輸入以下指令：

```
$ cargo build
```

或是

```
$ cargo b
```

這時 Cargo 就會自動產生出一個執行檔，路徑為 /target/debug/hello_world

如果要執行結果的話，就在終端機輸入該檔案的路徑，則會顯示出運行的結果，就像下圖這樣：

![https://ithelp.ithome.com.tw/upload/images/20220918/20120293QIKgnK4cwT.png](https://ithelp.ithome.com.tw/upload/images/20220918/20120293QIKgnK4cwT.png)

## cargo run

如果想要 build 並且執行專案結果的話，Cargo 也有提供更快的方式。

在終端機輸入：

```
$ cargo run
```

或是

```
$ cargo r
```

Cargo 就會 build 這個專案，並且執行得到結果。

## cargo check

如果只想要檢查這個專案的程式碼有無錯誤的話，可以省略產生執行檔的步驟。

在終端機輸入：

```
$ cargo check
```

或是

```
$ cargo c
```

## cargo release

如果專案已經準備好發佈的話，可以輸入：

```
$ cargo build --release
```

或是

```
$ cargo b -r
```

如果要順便執行的話，就輸入：

```
$ cargo run --release
```

或是

```
$ cargo r -r
```

那麼 release 跟 build 有什麼差別呢？

最大的不同是 Cargo 會幫你最佳化編譯結果，並且產生的執行檔會在 /target/release。不過要注意的是，編譯的時間會變得更久，但可以讓該程式跑得更快。

以上就是 Cargo 的一些簡單介紹，很多的步驟都可以使用指令來幫助開發者快速地管理專案，是不是很棒呢(⁎⁍̴̛ᴗ⁍̴̛⁎)‼
