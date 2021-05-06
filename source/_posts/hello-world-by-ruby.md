---
title: Ruby[0] - 從安裝環境到 Hello World
date: 2021-05-06 09:37:47
tags:
  - ruby
  - install
categories: 從零開始寫 Ruby 
keywords:
  - ruby
  - install
decription: 從安裝 Ruby 環境到寫出 Hello World 
---

前端玩久了，最近想接觸一下後端看看。但是後端語言這麼多，改選哪種呢？最後我選了 Ruby，她的語法看起來很有趣，而且平常工作上用不到，這樣才有想玩的動力吧XD

<!--read more-->

## Ruby 環境安裝

首先從安裝環境開始。可以直接從[官網](https://www.ruby-lang.org/zh_tw/downloads/)下載，或是使用 RVM 來管理並下載任何版本，本篇文章介紹的是使用後者的方法。

（本篇文章是用 Mac 環境安裝）

### 什麼是 RVM？

*RVM* 是一種 Ruby 的版本管理套件，它可以讓使用者根據不同需求來安裝不同的版本使用。

相關介紹可以在 [RVM 網站](https://rvm.io/)可以看到更多。



### 開始安裝 RVM

1. 在 Terminal 輸入：

   ```bash
   $ \curl -sSL https://get.rvm.io | bash -s stable
   ```

2. 接著確認有無安裝成功，輸入：

```bash
$ rvm -v  // 有顯示版本就代表安裝成功
```



### 開始安裝 Ruby

1. 看看目前有哪些版本提供安裝：

```bash
$ rvm ls known
```

2. 接著輸入想要安裝的版本，例如 3.0：

```bash
$ rvm install 3.0
```

3. 確認有無安裝成功：

```bash
$ ruby -v  // 有顯示版本就代表安裝成功
```



## Ruby 的 Hello World

要學一門程式語言，大部分都會先從 Hello World 開始，所以這邊也不免俗地來輸出 Hello World 吧～

首先要先知道怎麼把內容顯示出來，如果是在瀏覽器上用 JavaScript 的話，就會是用 `console.log()`：

```js
console.log('Hello World') // 顯示 Hello World
```

而在 Ruby 中，有幾種語法：

1. `puts`
2. `print`
3. `p`

要執行的話就如同下方語法：

```ruby
puts 'Hello World'
print 'Hello World'
p 'Hello World'
```

那麼要在哪裏執行 Ruby 呢？要再來介紹一下 Ruby 執行程式碼的幾種方法：

1. 在 terminal 使用 `ruby -e` 執行：

```ruby
$ ruby -e "puts 'Hello World'" 
# 印出 Hello World
```

2. 在 terminal 使用 IRB 模式執行：

```ruby
# 先輸入 irb，進入 irb 模式中
$ irb
# 接著直接執行程式碼
$ puts 'Hello World'

# 如果要退出 irb 模式，輸入 exit 後再按 Enter
```

3. 在 terminal 執行寫好的 .rb檔：

```ruby
# 例如建立一個 demo.rb，並在裡面輸入一些程式碼
puts 'Hello World'
print 'Hello World--------'
p 'Hello World'

# ----------------分隔線--------------------------

# 在 terminal 執行檔案
$ ruby demo.rb
```

執行的結果會是下圖：

![image-20210505232233343](https://i.imgur.com/SVCSfdU.png)

### 3種輸出方式有什麼差別：

就如同上面執行後的結果，可以看到有一些差別：

1. `puts`：把內容顯示出來，並且會換行。
2. `print`：就只是把內容顯示出來，不會換行。
3. `p`：把所有資訊內容顯示出來，例如字串的話，會把 `""` 一同顯示。

