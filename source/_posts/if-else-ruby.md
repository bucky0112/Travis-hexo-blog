---
title: Ruby 的邏輯判斷與流程控制
date: 2021-05-16 17:12:51
tags:
  - ruby
  - if else
categories: 從零開始寫 Ruby 
keywords:
  - ruby
  - if else
decription: Ruby 的邏輯判斷與流程控制
---

本篇文章將會提到 Ruby 該如何使用判斷式。

<!--read more-->

## If

在 JavaScript 中，如果要做判斷式的話，會這樣做：

```js
let num = 10;

if (num !== 5) {
  console.log('false');
}
// 結果會印出 false
```

而在 Ruby 中，也是差不多的作法：

```ruby
num = 10

if num != 5
  puts 'false'
end
# 結果會印出 false
```

不過 Ruby 倒是提供了不一樣的寫法：

### if not

這個用法感覺起來比較像英文的寫法，上面的例子如果用 `if not` 來表示的話，就會是：

```ruby
num = 10

if not num == 5
  puts 'false'
end
# 結果會印出 false
```

### if 倒裝句

這個寫法就更像英文了，如果寫起來就像是英文的倒裝句一樣，但是這個的用法只限一行的程式碼使用：

```ruby
num = 10

puts 'false' if not num == 5
# 結果會印出 false
```

### unless

`if not` 也可以使用 `unless` 來替代：

```ruby
num = 10

unless num == 5
  puts 'false'
end
# 結果會印出 false
```

當然也可以使用倒裝句：

```ruby
num = 10

puts 'false' unless num == 5
# 結果會印出 false
```



## if...else 跟 if...elsif...else 還有 case...when

### if...else

如果要使用到判斷的話，一般情況下都會有多種情形判別，例如：

```ruby
deposit = 30000
option = ''

if deposit >= 30900
  option = '可以買 M1 Macbook Air'
else
  option = '繼續存錢QQ'
end

puts option
# 繼續存錢QQ
```

也可以使用三元運算子：

```ruby
deposit = 31000
option = ''

option = (deposit >= 30900) ? '可以買 M1 Macbook Air' : '繼續存錢QQ'
puts option
# 可以買 M1 Macbook Air
```

### if...elsif...else

如果有兩種以上的選擇的話，就可以使用這個寫法，*要特別注意是 `elsif`*：

```ruby
deposit = 40000
option = ''

if deposit >= 39900
  option = '可以買 M1 MacBook Pro'
elsif deposit >= 30900
  option = '可以買 M1 Macbook Air'
else
  option = '繼續存錢QQ'
end

puts option
```

### case...when

如果在 JavaScript 中，有多種條件判別的話，可以使用 `swtich`：

```js
const skill = 'JavaScript';
let option = '';

switch (skill) {
  case 'Ruby':
    option = 'Ruby on Rails';
    break;
  case 'C#':
    option = 'ASP.Net';
    break;
  case 'JavaScript':
    option = 'Express';
    break;
  default:
    option = 'Learn something else';
}

console.log(option)
```

而在 Ruby 中，也有類似的語法可以使用，那就是 `case...when`：

```ruby
skill = 'PHP'
option = ''

case skill
when 'Ruby'
  option = 'Ruby on Rails'
when 'PHP'
  option = 'Laravel'
when 'JavaScript'
  option = 'Express'
else
  option = 'Learn something else'
end

puts option
# 印出 Laravel
```

