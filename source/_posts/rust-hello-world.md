---
title: 先從意料中的 Hello World 開始
date: 2022-09-17 16:54:15
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: BJ4，就只是 Hello World
---

今天就先從最基本的 Hello World 開始我們的 Rust，但第一步要先安裝環境，那麼就開始吧！ 

## 安裝

基本上在安裝過程中沒有太多的難度，也不用繁複的設定，就可以在各個系統中順利安裝。

由於我是使用 Mac 來安裝，所以只需要開啟終端機，並輸入：

```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

這裡是使用 rustup 這個版本管理工具來進行安裝，接著會如下圖中，問你要怎麼安裝，如果沒有什麼特殊需求的話，我們只需要輸入 1 用預設的就好。

![https://ithelp.ithome.com.tw/upload/images/20220917/20120293Wl6wOM6UUg.jpg](https://ithelp.ithome.com.tw/upload/images/20220917/20120293Wl6wOM6UUg.jpg)

沒什麼意外的話，就會跟你說安裝已完成，但他還提示我們需要設定環境變數，一樣照著他指示的做，在終端機輸入：

```
$ source "$HOME/.cargo/env"
```

![https://ithelp.ithome.com.tw/upload/images/20220917/20120293By6OOpPKct.png](https://ithelp.ithome.com.tw/upload/images/20220917/20120293By6OOpPKct.png)

如果上述的過程中都沒遇到問題的話，那就來檢查是否安裝成功，輸入：

```
$ rustc -V
```

或是：

```
$ rustc --version
```

如果成功安裝的話，就會顯示安裝的版本，截至目前最新的版本是 1.63.0

如果是其他系統的使用者的話，進入到 [Rust 官網](https://www.rust-lang.org/) 進入 Install 的頁面，就會自動偵測使用者系統，並且給予安裝的說明或是下載連結，非常貼心。

## Hello World

接著就是我們今天的主要目標，用 Rust 來寫一個 Hello World。

在開始寫 Code 之前，先打開我們慣用的編輯器，我個人是使用 Visual Studio Code，也就是 VSCode，然後推薦先安裝套件 — [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)，提供了提示程式碼的部分，更重要的是會即時的提示你哪邊有錯誤，可以幫助你在後續寫 code 的時候會比較舒適。

首先建立一個 `hello_world.rs` 檔案（副檔名為 rs，Rust 的文件命名通常會用 Camel Case 也就是駝峰式的寫法），並且在檔案中輸入：

```rust
fn main() {
    println!("Hello, world!");
}
```

`fn main() {}` 是 Rust 第一個優先執行的程式碼，是必需的一個函式，不會有參數，也不會回傳任何東西。

Rust 的排版風格是 4 個空格，而不是一個 tab。

`println!()` 是 Rust 拿來印出內容的一個巨集（marco）。那麼什麼是巨集呢？他跟函式不太一樣，先把他想成是一種 Rust 的功能集合，之後會再找機會來講解這個部分。

接著把 “Hello World” 作為參數帶入，然後最後記得加上 `;`，結尾非常重要，不加的話肯定會報錯給你看。

存檔後試著執行，在終端機輸入：

```
$ rustc hello_world.rs
```

`rustc` 的 *c* 是編譯（compiler）的意思。

接著在該資料夾會出現一個執行檔，可以直接點擊該檔案，或是繼續在終端機輸入：

```
$ ./hello_world
```

接著就會在終端機顯示 Hello World!

以上就是我們今天簡單的 Hello World 了，是不是很簡單呢？
(ゝ∀･)b
