---
title: ruby-hash
tags:
---

## Hash

Hash 是一種資料結構，它是一種鍵值對的集合，鍵值對的鍵是唯一的，但值可以重複。

### 建立 Hash

```ruby
# 建立一個空的 Hash
hash = {}

# 建立一個 Hash
hash = { :a => 1, :b => 2, :c => 3 } # 這是 Ruby 1.9 之前的寫法
hash = { a: 1, b: 2, c: 3 } # 這是 Ruby 1.9 之後的寫法
```

雖然使用新的寫法看起來很像 JSON，但其實印出來跟舊的寫法還是一樣：

```ruby
hash = { a: 1, b: 2, c: 3 }
puts hash # {:a=>1, :b=>2, :c=>3}
```

### Hash 的操作

也因為語法糖的關係，所以其實要取得 Hash 的值，也是用舊的寫法：

```ruby
hash = { a: 1, b: 2, c: 3 }
puts hash[:a] # 1
```

或者是像一般的物件一樣，用 `.` 來取得值：

```ruby
hash = { a: 1, b: 2, c: 3 }
puts hash.a # 1
```

也可以使用 `key` 跟 `value` 來取得鍵值對：

```ruby
hash = { a: 1, b: 2, c: 3 }
puts hash.keys # [:a, :b, :c]
puts hash.values # [1, 2, 3]
```

### Hash 的遍歷

Hash 的遍歷可以使用 `each` 來遍歷：

```ruby
hash = { a: 1, b: 2, c: 3 }

hash.each do |key, value|
  puts "#{key} => #{value}"
end
```
