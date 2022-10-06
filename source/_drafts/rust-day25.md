---
title: rust-day25
tags:
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

