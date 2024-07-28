---
title: Type or Interface?
date: 2024-07-28 15:55:31
tags:
 - TypeScript
 - Type
 - Interface
categories:
 - TypeScript
keywords:
 - TypeScript
 - Type
 - Interface
decription: TypeScript 的 type 和 interface 有什麼不同？
---

在開發 TS 一段時間後，發覺 type 跟 interface 這兩個東西看起來很像，一直不太清楚使用的時機是什麼，所以研究了一下，寫了一篇文章記錄一下。

站在巨人的肩膀上，我參考了這部影片，有些觀點很有趣：

<iframe width="560" height="315" src="https://www.youtube.com/embed/oiFo2z8ILNo?si=Y616iyggvOgHnG56" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

在開始之前，先簡單介紹一下這兩種東西，並比較一下兩者的差異。

## 什麼是 type？

type 是 TypeScript 中用來定義型別別名的關鍵字，它可以定義任何型別。

```typescript!
type UserType = {
    name: string
    age: number
}

const user: UserType = {
    name: "Bucky",
    age: 18,
}
```

## 什麼是 interface

interface 是用來定義物件型別的一種方式，它主要用於描述物件的形狀。

```typescript!
interface UserInter {
    name: string
    age: number
}

const user: UserInter = {
    name: "Bucky",
    age: 18,
}
```

## TypeScript 官方文件推薦哪一個？

在官方文件中，[Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)這一段落最後有下一個結論：

>For the most part, you can choose based on personal preference, and TypeScript will tell you if it needs something to be the other kind of declaration. If you would like a heuristic, use interface until you need to use features from type.

## interface 的局限性

在 Kyle 的影片當中，他提到了 interface 只能用在物件上，而不像 type 比較靈活，例如：

```typescript!
type StringType = string

const user: StringType = "Bucky"
```

但是 interface 沒辦法這樣做，這樣做就會壞掉：

```typescript!
interface StringInter = string

const user: StringInter = "Bucky"
```

會跟你說：

```
'string' only refers to a type, but is being used as a value here.
```

那麼 type 還可以做到什麼呢？

## type 的靈活

例如一個變數可能有多個型別，type 可以定義聯合型別：

```typescript!
type IdType = string | number

const orderId: IdType = "123"
```

type 還可以用於定義交叉型別：

```typescript!
type A = { a: string }
type B = { b: number }
type AB = A & B

const ab: AB = { a: "hello", b: 42 }
```

## interface 的擴充性

interface 可以通過 `extends` 關鍵字進行擴充：

```typescript!
interface A {
  age: number
}

interface User extends A {
  name: string
}

const user: User = {
  name: "John",
  age: 30,
}
```

但是 type 也可以通過交叉型別實現類似的擴充：

```typescript!
type User = {
  name: string
} & {
  age: number
}

const user: User = {
  name: "John",
  age: 30,
}
```

## type 的獨特之處

type 可以用於定義聯合型別的物件：

```typescript!
type User = {
  name: string
} | {
  age: number
}

const user1: User = { name: "John" }
const user2: User = { age: 30 }
const user3: User = { name: "John", age: 30 } // 這也是合法的
```

這是 interface 無法直接實現的功能。

## interface 的獨特特性：宣告合併

前面講了這麼多 type 做得到，但 interface 做不到的地方。不過 interface 確實是有一個 type 無法實現的特性是宣告合併（Declaration Merging）。

可以讓你在多個地方定義同一個 interface，TypeScript 會自動將這些定義合併。例如：

```typescript
interface Order {
  id: number
}

interface Order {
  name: string
}

interface Order {
  age: number
}

const order: Order = {
  id: 1,
  name: "John",
  age: 30,
}
```

但我想不到這樣做的好處有什麼，反而會降低可讀性。

## 總結

如果你需要定義複雜的型別（如聯合型別、交叉型別、條件類型等），使用 type。

如果你主要想定義物件的結構，並且可能需要在未來擴展這個結構，那就可考慮使用 interface。

最後，無論選擇 type 還是 interface 都是可以的，選擇哪一個主要取決於個人偏好和團隊約定。最重要的還是在整個專案中保持一致性，程式碼的可讀性和可維護性還是最重要的。