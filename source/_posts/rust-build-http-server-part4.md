---
title: 來用 Rust 建立一個伺服器吧！Part IV
date: 2022-10-10 01:03:43
tags:
 - rust
categories: 30 天快快樂樂學 Rust
keywords:
 - rust
decription: 用 Rust 建立一個伺服器
---

昨天我們新增了一個檔案，專門來做執行緒池的部分，但還沒完成，所以今天就接著繼續完成。

## 建立執行緒池的儲存空間

首先我們要建立一個儲存執行緒的空間，這個空間會儲存所有的執行緒，然後我們可以從這個空間取出執行緒來使用。

```rust
use std::thread::JoinHandle;

pub struct ThreadPool {
    threads: Vec<JoinHandle<()>>,
}

impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);
        let mut threads = Vec::with_capacity(size);
        for _ in 0..size {
            // 產生執行緒並儲存至向量
        }
        ThreadPool { threads }
    }

    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
    }
}
```

這邊我們先建立一個 `ThreadPool` 的結構，然後在裡面建立一個 `threads` 的向量，這個向量會儲存所有的執行緒，然後我們會在 `new` 方法中產生執行緒並儲存至向量中。

## 完成執行緒池的建立

接下來就直接先上完成的 code，然後再來解釋。

```rust
use std::sync::{
    mpsc::{self, Receiver, Sender},
    Arc, Mutex,
};
use std::thread::{self, JoinHandle};

type Job = Box<dyn FnOnce() + Send + 'static>;

pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Sender<Job>,
}

impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);

        let (sender, receiver) = mpsc::channel();

        let receiver = Arc::new(Mutex::new(receiver));

        let mut workers = Vec::with_capacity(size);

        for id in 0..size {
            workers.push(Worker::new(id, receiver.clone()));
        }

        ThreadPool { workers, sender }
    }

    pub fn execute<F: FnOnce() + Send + 'static>(&self, f: F) {
        let job = Box::new(f);

        self.sender.send(job).unwrap();
    }
}

struct Worker {
    id: usize,
    thread: JoinHandle<()>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();

            println!("Worker {} got a job; executing.", id);

            job();
        });

        Worker { id, thread }
    }
}
```

首先，從 `use` 引入的部分，我們引入了 `mpsc` 的 channel，這個 channel 會用來傳遞執行緒的工作，然後我們也引入了 `Arc` 和 `Mutex`，這兩個是用來做多執行緒的共享資源的。

接下來，我們定義了一個 `Job` 的型別，這個型別是一個 `Box`，裡面放的是一個 `FnOnce` 的 trait，這個 trait 會用來定義執行緒的工作，然後我們會在 `execute` 方法中將這個 trait 傳遞給 channel。

再來，我們在 `ThreadPool` 的結構中，新增了一個 `workers` 的向量，這個向量會儲存所有的執行緒，然後我們會在 `new` 方法中產生執行緒並儲存至向量中。

接著，我們在 `new` 方法中，先建立一個 channel，然後我們會將 channel 的接收端傳入執行緒中，這樣執行緒就可以從 channel 中取得工作了。

最後，我們在 `execute` 方法中，將工作傳遞給 channel。

## 執行緒池的測試

接下來，我們來測試一下我們的執行緒池。

```rust
// src/main.rs
extern crate server;
use std::io::prelude::*;
use std::net::{TcpListener, TcpStream};
use std::{fs, path::Path, thread, time::Duration};
use server::ThreadPool;

const PORT: i32 = 3000;
const WORKER_SIZE: usize = 4; // 這裡設定執行緒數量

fn main() {
    let listener = TcpListener::bind(format!("127.0.0.1:{}", PORT)).unwrap();
    let pool = ThreadPool::new(WORKER_SIZE);

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| { handle_connection(stream); });
    }
}

// 省略...
```

現在當我們執行程式，並且在瀏覽器開啟多個分頁，我們可以透過終端機來觀察執行緒池的執行情況，並且可以發現，現在可以非同步處理多個連線，而且收到大量請求時，也不會因為執行緒不夠而導致無法回應。