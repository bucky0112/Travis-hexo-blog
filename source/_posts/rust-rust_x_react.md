---
title: Rust x React
date: 2022-10-15 01:50:59
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
 - react
 - webassembly
decription: Rust x React 夢幻共演
---

終於到最後一天了，看到[這篇文章](https://www.cnblogs.com/yjmyzz/p/how-to-create-a-react-app-with-rust-and-wasm.html)覺得很有趣，所以今天我們來玩一下 Rust 跟 React 的結合吧！

## 開始專案

首先我們先建立一個新的專案，這邊我們使用 create-react-app 來建立一個新的專案，並且使用 yarn 來安裝相依套件。

```bash
$ yarn create react-app rust-react
$ cd rust-react
$ yarn
```

## 建立 Rust

接下來我們在專案中建立一個 Rust 的 library：

```bash
$ cargo new wasm-lib --lib
```
然後會在專案中看到一個 wasm-lib 的資料夾，裡面會有一個 `src` 資料夾，裡面會有一個 `lib.rs` 的檔案，這個檔案就是我們的 Rust library。

![CleanShot 2022-10-11 at 17.18.51](https://i.imgur.com/AUNRQuP.png)

因為我們預計想要做一個可以點擊加減的按鈕，然後回傳計算結果，所以我們可以在 `lib.rs` 中加入：

```rust
pub fn calculator(a: i32, b: i32) -> i32 {
    a + b
}

#[test]
fn cal_test() {
    assert_eq!(1 + 1, calculator(1, 1));
}
```

然後 cd 到 `wasm-lib` 資料夾，並且測試一下：

```bash
$ cd wasm-lib
$ cargo test
```

沒什麼問題的話，就會出現以下的結果：

![CleanShot 2022-10-11 at 17.25.42](https://i.imgur.com/CdjJOch.png)

接下來到 `Cargo.toml` 中加入以下的設定：

```toml
[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2.78"
```

這一段的意思是，我們要把這個 library 轉換成 WebAssembly 的格式，並且使用 wasm-bindgen 來幫我們轉換。

然後還要在 `src/lib.rs` 中加入設定：

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn calculator(a: i32, b: i32) -> i32 {
    a + b
}
```

這樣可以讓 `calculator` 這個函式可以在 wasm 中被使用。

接下來我們要安裝 wasm-pack：

```bash
$ cargo install wasm-pack
```

然後我們還需要讓 Rust 可以在這個專案中可以運作。
所以在 `package.json` 中加入 script:

```json
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject",
  "build:wasm": "cd wasm-lib && wasm-pack build --target web --out-dir pkg"
},
```

接著執行：

```bash
$ yarn build:wasm
```

然後就會生成一個 `pkg` 的資料夾，裡面會有一個 `wasm_lib_bg.wasm` 的檔案，這就是我們的 WebAssembly 檔案。

![CleanShot 2022-10-11 at 17.50.36@2x](https://i.imgur.com/O6rvmyB.png)

並且我們可以看到我們剛剛建立的 `calculator` 函式已經生成在 `wasm_lib.d.ts` 中了。

![CleanShot 2022-10-11 at 18.00.17@2x](https://i.imgur.com/TbRxl8D.png)

然後我們還需要把他加進 `node_modules` 中，所以我們要在終端機中執行：

```bash
$ yarn add ./wasm-lib/pkg
```

這樣就可以在 `node_modules` 中看到 `wasm-lib` 這個資料夾了，並且我們可以引入來使用他。

最後我們來 React 中使用他：

```jsx
import { useState } from 'react'
import init, { calculator } from 'wasm-lib'
import logo from './logo.svg'
import './App.css'

function App() {
  const [num, setNum] = useState(0)

  const handlePlus = () => {
    init().then(() => {
      setNum(calculator(num, 1))
    })
  }

  const handleMinus = () => {
    if (num > 0) {
      init().then(() => {
        setNum(calculator(num, -1))
      })
    }
  }

  return (
    <div className='App'>
      <header className='App-header'>
        <img src={logo} className='App-logo' alt='logo' />
        <p>{num}</p>
        <div>
          <button onClick={handlePlus}>+</button>
          <button onClick={handleMinus}>-</button>
        </div>
      </header>
    </div>
  )
}

export default App
```

然後執行 `yarn start`，就可以執行我們的專案，並且讓 Rust 運行在 React 中了。

## 同場加映

前面只是單純的使用 Rust 來做加減的動作，但是這樣看不出 Rust 的效能優勢，所以我們來做一些稍微複雜的運算玩玩看。

我們來做一個費氏數列的函式，並且使用 Rust 來做運算。

首先在 `wasm-lib/src/lib.rs` 中加入新的方法，並且寫一下測試：

```rust
#[wasm_bindgen]
pub fn fibonacci(n: i32) -> i32 {
    if n <= 1 {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}

#[test]
fn fib_test() {
    assert_eq!(1, fibonacci(1));
    assert_eq!(1, fibonacci(2));
    assert_eq!(2, fibonacci(3));
    assert_eq!(3, fibonacci(4));
    assert_eq!(5, fibonacci(5));
    assert_eq!(8, fibonacci(6));
}
```

跑一下測試，執行：

```bash
// 在 wasm-lib 資料夾中
$ cargo test
```

![CleanShot 2022-10-14 at 17.50.57@2x](https://i.imgur.com/E6huTQ5.png)

然後記得要重新 build：

```bash
// 要在根目錄中執行
$ yarn build:wasm
$ yarn add ./wasm-lib/pkg
```

然後在 `App.js` 加進我們的 `fibonacci` 函式：

```js
import init, { calculator, fibonacci } from 'wasm-lib'

function App() {
  // 省略...
  const handleFibonacci = () => {
    let start = new Date().getTime()
    init().then(() => {
      setFibonacciNum(fibonacci(inputNum))
    })
    let end = new Date().getTime()
    console.log(`Rust: 時間： ${end - start} ms`)
  }
  // 省略...
}
```

然後另外寫一個 JS 的費氏數列函式：

```js
const calFibonacci = (n) => {
  if (n <= 1) {
    return n
  } else {
    return calFibonacci(n - 1) + calFibonacci(n - 2)
  }
}

function App() {
  // 省略...
  const handleFibonacciJS = () => {
    let start = new Date().getTime()
    let result = calFibonacci(inputNum)
    setFibonacciNumJS(result)
    let end = new Date().getTime()
    console.log(`JS: 時間： ${end - start} ms`)
  }
  // 省略...
}
```

最後執行一下結果：

![CleanShot 2022-10-15 at 01.02.46](https://i.imgur.com/gFcFkIx.gif)

相信看到畫面，不用多說你應該也能感受到 Rust 的效能優勢了。

如果有興趣的話，可以直接參考我的 [GitHub Repo](https://github.com/bucky0112/rust_x_react)。

## 心得

這 30 天真的滿辛苦的，每天都要硬擠出時間來寫文章，本來有跟朋友一起寫 side project 的行程，但因為時間不夠的關係，只好先跟朋友暫緩，真的是對他們非常抱歉。

但是自己也覺得這 30 天真的學到很多東西，雖然寫到一半的時候，有點想要放棄，但是想到自己已經開始了，就不想要放棄，所以就繼續寫下去，最後也完成了。完賽的成就感真的是很棒，也讓我對自己的信心有了一點點的提升。

然後 Rust 真的是一個很棒的語言，雖然我還沒有寫到很複雜的東西，但是我相信未來會有更多的機會可以使用 Rust，也希望自己能夠繼續學習下去。

![ferris](https://i.imgur.com/gCQstja.gif)