---
title: rust-day30
tags:
---

終於到最後一天了，今天我們來玩一下 Rust 跟 React 的結合吧！

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
然後會在專案中看到一個 wasm-lib 的資料夾，裡面會有一個 src 資料夾，裡面會有一個 lib.rs 的檔案，這個檔案就是我們的 Rust library。

![CleanShot 2022-10-11 at 17.18.51](https://i.imgur.com/AUNRQuP.png)

因為我們預計想要做一個可以點擊加減的按鈕，然後回傳計算結果，所以我們可以在 lib.rs 中加入：

```rust
pub fn calculator(a: i32, b: i32) -> i32 {
    a + b
}

#[test]
fn cal_test() {
    assert_eq!(1 + 1, calculator(1, 1));
}
```

然後 cd 到 wasm-lib 資料夾，並且測試一下：

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

然後就會生成一個 pkg 的資料夾，裡面會有一個 wasm_lib_bg.wasm 的檔案，這就是我們的 WebAssembly 檔案。

![CleanShot 2022-10-11 at 17.50.36@2x](https://i.imgur.com/O6rvmyB.png)

並且我們可以看到我們剛剛建立的 `calculator` 函式已經生成在 `wasm_lib.d.ts` 中了。

![CleanShot 2022-10-11 at 18.00.17@2x](https://i.imgur.com/TbRxl8D.png)

然後我們還需要把他加進 node_modules 中，所以我們要在終端機中執行：

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