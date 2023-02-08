---
title: ruby-loop
tags:
---

## loop

Ruby 的迴圈分別有：

- for
- while
- loop
- method
- 迭代式迴圈

### for

```ruby
for i in 1..5
  puts i
end
```

或是跑陣列：

```ruby
for i in [1, 2, 3, 4, 5]
  puts i
end
```

### while

```ruby
i = 0

while i < 5
  puts i
  i += 1
end
```

#### until

```ruby
i = 0

until i == 5
  puts i
  i += 1
end
```

### loop

```ruby
i = 0

loop do
  puts i
  i += 1
  break if i == 5
end
```

do ... end 也可以寫成 {}，但是 {} 不能寫在同一行，所以要換行。

```ruby
i = 0

loop {
  puts i
  i += 1
  break if i == 5
}
```

### method

```ruby
10.times do |i|
  puts i
end
```

### 迭代式迴圈

```ruby
list = [1, 2, 3, 4, 5]

list.each.with_index do |item, index|
  puts "#{index}: #{item}"
end
```