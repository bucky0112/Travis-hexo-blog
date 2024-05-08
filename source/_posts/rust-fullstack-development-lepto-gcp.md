---
title: 使用 Leptos 建立和部署 WASM 到 GCP
date: 2023-12-16 16:02:32
tags:
 - rust
 - leptos
 - gcp
 - app engine
categories:
 - Rust
keywords:
 - rust
 - leptos
 - gcp
 - app engine
decription: 2023 DevFest Taipei 議程之一，介紹如何使用 Leptos 建立和部署 WASM 到 GCP。
top_img: https://i.imgur.com/RKVo9NP.png
---
這篇文章是由 Google Developer Group(GDG) 主辦的 DevFest Taipei 2023 其中的一個議程，主題是 - 使用 Leptos 建立和部署 WASM 到 GCP。

## 什麼是 Leptos

近年來，Rust 語言越來越受到關注，而有關 Web 端的生態系更是蓬勃發展，不管是前端還是後端，都有許多優秀的框架可以使用，而 Leptos 就是其中一個框架。

那麼 Leptos 有什麼特別的呢？

Leptos 標榜是一個全端框架，話雖如此，但還是以前端為主，真要說的話，其角色有點類似於 Next.js。而 Leptos 提供了兩種 render 的方式，分別是：

- Client-side rendering(CSR)
- Server-side rendering(SSR)

而不管是哪一種 render 的方式，都是使用 WASM 來實現的。

![Leptos benchmark](https://i.imgur.com/RKVo9NP.png)

作為效能優異的 Rust 語言，Leptos 也不例外，其效能也是非常優秀的，如上圖 Leptos 官網所示，Leptos 的效能比起其他框架來說，是非常優秀的。

## 什麼是 WASM

WASM 是 WebAssembly 的縮寫，是一種可以在瀏覽器上執行的低階語言，可以將其他語言編譯成 WASM，然後在瀏覽器上執行。

根據 MDN 的說法，WASM 是一種可移植、大小、效能都很好的格式，可以在瀏覽器上執行，而且可以跨平台，不管是在桌面、行動裝置、IoT 裝置，都可以執行。

既然 WASM 這麼猛，是不是表示未來前端的趨勢就是 WASM 了呢？其實也不是，WASM 雖然可以在瀏覽器上執行，但是它並不是一種語言，而是一種格式，所以它只是一種載具，而不是一種語言，所以它並不會取代 JavaScript，而是會和 JavaScript 一起使用。

簡單來說，WASM 需要 JavaScript 來執行，所以 JavaScript 和 WASM 是互補的關係，而不是取代的關係。

## WASM 怎麼跟 JavaScript 搭配

WASM 可以透過 wasm-bindgen 來跟 JavaScript 搭配使用，wasm-bindgen 可以讓 JavaScript 和 WASM 互相交換數據，可以把 wasm-bindgen 想像成一個橋樑，讓 JavaScript 和 WASM 互相溝通。

![wasm-bindgen 橋樑示意圖](https://i.imgur.com/OxBO8IW.png)

## Leptos 初體驗

首先，先選擇使用哪種 render 的方式，這裡選擇使用 Server-side rendering。

接著，按照以下步驟執行：

```bash
cargo install cargo-leptos
cargo leptos new --git https://github.com/leptos-rs/start
cd [your project name]
cargo leptos watch
```

稍微講一下 `cargo-leptos`，這是一個 cargo 的 工具，可以讓我們使用 cargo 來建立 Leptos 專案，而 `cargo leptos new` 則是用來建立新專案，`cargo leptos watch` 則是用來啟動開發伺服器。

這裡預設是使用 actix-web 來處理後端 server，也可以選擇 Axum，不過這裡就不多做介紹了。

### IntoView

![Leptos-App](https://i.imgur.com/hSABXYf.png)

在 Leptos 中，`IntoView` 是一個重要的特徵（trait），用於將 Rust code 轉換成可以在瀏覽器中顯示的部分。

當使用 `view!` 來定義 UI 界面時，`IntoView` 讓這些 Rust component 可以轉換成 HTML。這樣，開發者就可以用 Rust 編寫 Web 應用的前端，而不是使用傳統的 HTML、CSS 或 JavaScript。這種方法有助於在 Rust 生態系統中保持類型安全和效能優勢，同時提供不同的前端開發體驗。

### create_signal

![Leptos 的狀態](https://i.imgur.com/nnKA04g.png)

在 Leptos 中，狀態是由 `Signal` 來管理的，而 `Signal` 可以透過 `create_signal` 來建立，`create_signal` 會回傳一個 `Signal`，而 `Signal` 可以透過 `get` 來取得目前的狀態，也可以透過 `set` 來設定狀態。

在圖片的例子中，更新狀態使用了 `update` 這個 function，這個 function 會接收一個 closure，而 closure 會接收一個 `&mut` 的參數，這個參數就是目前的狀態，而在 closure 中，我們可以對狀態做任何的操作，最後再回傳一個新的狀態，這樣就可以更新狀態了。

寫法上有點類似於 React 的 `useState`，如果有寫過 React 的話，應該會比較好理解。

### Actix-web 處理後端狀態

![actix-web](https://i.imgur.com/sgGEzAX.png)

如果選擇的是 SSR 的話，預設會使用 actix-web 來處理後端的狀態，而 actix-web 是一個非常優秀的 Rust web framework，效能非常好，而且也有很多的 middleware 可以使用。

## 建立一個新專案

接下來，就可以把 Leptos 專案部署到 GCP 上了，首先，先建立一個新專案，這裡以 `was-demo` 為例：

![gcp 專案](https://i.imgur.com/7DiszI3.png)

## 安裝 GCP SDK

然後到[這裡](https://cloud.google.com/sdk/docs/install)下載 SDK，選擇適合的作業系統，這裡以 MacOS 為例，我下載的是 macOS 64-bit 版本（ARM64, Apple M1 silicon）。

下載後會是一個壓縮檔，解壓縮後放入適合的位置，我是放在使用者的目錄下。並且執行以下指令來安裝：

```bash
./google-cloud-sdk/install.sh
```

完成之後，執行以下指令來初始化：

```bash
./google-cloud-sdk/bin/gcloud init
```

接下來就會進入初始化的流程，會要求你登入 Google 帳號，登入之後，會要求你選擇專案，選擇剛剛建立的專案。

## 建立 Docker Image

要部署 Leptos 專案，最好的方式就是使用 Docker。建立一個 `Dockerfile`，可以直接用官網提供的範例：

![dockerfile](https://i.imgur.com/BVQG8l7.png)

這個 `Dockerfile` 會建立一個 Docker Image，這個 Image 會包含我們的程式碼，以及執行程式碼所需要的環境。

接著，執行以下指令來建立 Docker Image：

```bash
docker build -t leptos-example .
```

然後，執行以下指令來啟動 Docker Container：

```bash
docker run -p 8080:8080 leptos-example
```

打開瀏覽器，輸入 `http://localhost:8080`，看到有畫面出現就代表成功了。

## 啟動 Google Container Registry API

到 [Google Cloud Console](https://console.cloud.google.com/) 啟用 Container Registry API。

![20231202150742](https://i.imgur.com/4GigPc1.png)

## 上傳 Docker Image 到 Google Container Registry

首先，確保在本機上已經登入 GCP：

```bash
gcloud auth login
```

也可以看一下自己的 config 是否正確：

```bash
gcloud auth configure-docker
```

並針對 Image 做標記：

```bash
docker tag leptos-example gcr.io/was-demo-406407/leptos-example
```

最後，上傳 Image 到 GCR：

```bash
docker push gcr.io/was-demo-406407/leptos-example
```

## 部署到 App Engine

首先，建立一個 `app.yaml`，內容如下：

```yaml
runtime: custom
env: flex

automatic_scaling:
  min_num_instances: 1
  max_num_instances: 2

resources:
  cpu: 1
  memory_gb: 1
  disk_size_gb: 10
```

這個部分，由於我們是使用 Docker Image 來部署，所以要使用 `custom` 這個 runtime，而 `env: flex` 則是使用 flexible 環境。

然後，執行以下指令來部署：

```bash
gcloud app deploy
```

會問你要部署到那一個區域，選擇適合的區域，這裡選擇 `asia-east1`。

接著就會開始部署，不過這裡遇到一個問題，似乎超過 10 分鐘就會 timeout，所以我們要設定一下 timeout 的時間：

```bash
gcloud config set app/cloud_build_timeout 1200
```

比較好的作法可能還是要調整一下 Docker Image 的大小，不過這裡就先這樣了。

再次執行以下指令來部署：

```bash
gcloud app deploy
```

部署成功之後，可以看到類似以下的畫面：

![20231202204511](https://i.imgur.com/Arn1kYT.png)

其中，會跟你說可以執行兩個指令，一個是 `gcloud app browse`，另一個是 `gcloud app logs tail -s default`，前者是開啟瀏覽器，後者是開啟 log。

以上就是如何將 Leptos 專案部署到 GCP 上的流程。

整體來說，這次 Leptos 的開發體驗很不錯，部署到 App Engine 的流程也算簡單，之後應該會把一些專案改寫成 Leptos 看看。