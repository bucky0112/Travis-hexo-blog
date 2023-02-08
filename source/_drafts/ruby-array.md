---
title: ruby-array
tags:
---

## array

Ruby 的陣列跟 JavaScript 的陣列很像，都是用 `[]` 包起來，裡面的元素用 `,` 隔開。

```ruby
list = [1, 2, 3, 4, 5]

puts list[0] # 1
```

```ruby
list = %w(1 2 3 4 5)

puts list[0] # 1

# %w() 會把裡面的元素轉成字串
```

## index

Ruby 的陣列的 index 是從 0 開始的。

```ruby
list = [1, 2, 3, 4, 5]

puts list[0] # 1
puts list[1] # 2
puts list[-1] # 5
puts list[-2] # 4
```

```ruby
list = [1, 2, 3, 4, 5]

puts list.first # 1
puts list.last # 5
puts list[0..2] # [1, 2, 3]
puts list[0...2] # [1, 2]
puts list[0, 2] # [1, 2]
```

## method

Ruby 的陣列有很多方法，可以用來操作陣列。

```ruby
list = [1, 2, 3, 4, 5]

puts list.length # 5
list << 6
puts list # [1, 2, 3, 4, 5, 6]

list.pop
puts list # [1, 2, 3, 4, 5]
```

### map

```ruby
list = [1, 2, 3, 4, 5]

puts list.map { |i| i * 2 } # [2, 4, 6, 8, 10]
```

類似 JavaScript 的 `map`，會回傳一個新的陣列。

或是也可以使用 `collect`。

```ruby
list = [1, 2, 3, 4, 5]

puts list.collect { |i| i * 2 } # [2, 4, 6, 8, 10]
```

### select

```ruby
list = [1, 2, 3, 4, 5]

puts list.select { |i| i % 2 == 0 } # [2, 4]
```

類似 JavaScript 的 `filter`，會回傳一個新的陣列。

相反的方法是 `reject`。

```ruby
list = [1, 2, 3, 4, 5]

puts list.reject { |i| i % 2 == 0 } # [1, 3, 5]
```

### reduce

```ruby
list = [1, 2, 3, 4, 5]

puts list.reduce(0) { |sum, i| sum + i } # 15
```

類似 JavaScript 的 `reduce`，會回傳一個值。

list = [1, 2, 3, 4, 5] => [1, 3, 5, 7, 9]

這裡可以使用 `map`。

```ruby
list = [1, 2, 3, 4, 5]

puts list.map { |i| i * 2 - 1 } # [1, 3, 5, 7, 9]
```

或是也可以使用 `each_with_index`。

```ruby
list = [1, 2, 3, 4, 5]

list.each_with_index do |i, index|
  list[index] = i * 2 - 1
end

puts list # [1, 3, 5, 7, 9]
```

### sort

```ruby
list = [5, 4, 3, 2, 1]

puts list.sort # [1, 2, 3, 4, 5]
```

類似 JavaScript 的 `sort`，會回傳一個新的陣列。

### uniq

```ruby
list = [1, 2, 3, 4, 5, 1, 2, 3, 4, 5]

puts list.uniq # [1, 2, 3, 4, 5]
```

## range

```ruby
list = (1..5).to_a

puts list # [1, 2, 3, 4, 5]
```

```ruby
chars = ('a'..'z').to_a

puts chars # ["a", ... , "z"]
```

```ruby
# 1 到 100 的單數
puts (1..100).select { |i| i % 2 == 1 } # [1, 3, 5, ... , 99]

# 1 到 100 的總和
puts (1..100).reduce(0) { |sum, i| sum + i } # 5050

# 5 個小於 100 不重複的亂數
puts (1..100).to_a.shuffle[0..4] # [1, 2, 3, 4, 5]
```
