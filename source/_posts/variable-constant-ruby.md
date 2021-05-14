---
title: Ruby 的變數與常數使用
date: 2021-05-15 01:18:35
tags:
  - ruby
  - 變數
  - 常數
categories: 從零開始寫 Ruby 
keywords:
  - ruby
  - 變數
  - 常數
decription: Ruby 的變數與常數使用 
---

聽說 Ruby 的常數跟別的程式語言不一樣？本篇就來看看 Ruby 的變數與常數該怎麼使用。

<!-- read more -->

## 變數的宣告與使用

Ruby 的變數宣告非常簡單，只要在變數的後面加上等號跟值就宣告成功了，就像這樣：

```ruby
my_name = 'Bucky'
num = 10
```

也可以一次宣告多個變數：

```ruby
x, y, z = 10, 4, 6
```

而且字串跟數字的使用也是非常靈活，例如：

```ruby
last_name = 'Bucky'
first_name = 'Chu'
age = 20

girls_name = 'Patty'
girls_age = 18

puts "My name is #{last_name} #{first_name}, I'm #{age} years old."
puts "My age bigest than #{girls_name} #{age - girls_age} years old."

# 印出 My name is Bucky Chu, I'm 20 years old.
# 印出 My age bigest than Patty 2 years old.
```

接著來玩個變數指定的遊戲，假設有兩個變數並且都有值，如果想要把兩個變數的值互換的話，可以怎麼做呢？

```js
// 如果以 JavaScript 的作法可能會是：
let x = 5;
let y = 9;

let temp = x;
x = y;
y = temp;
console.log(x, y); // 9, 5

// 或是用陣列的作法也一樣可行
[x, y] = [y, x];
console.log(x, y); // 9, 5

```

而在 Ruby 中，由於前面有介紹到可以一次宣告多個變數，所以就可以這樣使用：

```ruby
x = 5
y = 9

x, y = y, x
puts x, y # 9, 5
```



## Ruby 的常數

在 Ruby 的常數宣告方式很簡單，只要在變數名稱的首字為大寫就是宣告為常數，例如：

```ruby
My_name = 'Bucky'
USER_NUM = 5
```

而在其他程式語言中，如果宣告常數並賦予值後，是不能再更改值的，例如 JavaScript：

```js
const name = 'Steve';
name = 'Mike';
console.log(name) // 會出現錯誤，指出常數不能再更改值
```

不過在 Ruby 的世界中，常數是可以再更改值的：

```ruby
My_phone = 'Pixel5'

My_phone = 'iPhone13'

puts My_phone
```

![image-20210515022600231](https://i.imgur.com/5HbhevE.png)

執行後就像上圖，雖然有跳出警告，但是最後結果還是可以成功修改常數的。