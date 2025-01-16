---
title: Drizzle v.s Prisma
date: 2024-12-19 10:55:45
tags: 
  - orm
  - express
categories: 好用函式庫介紹
keywords:
  - orm
  - express
  - drizzle
  - prisma
decription: 比較 Prisma 跟 Drizzle 兩個 ORM 的一些差異。
---
目前的 JavaScript 的生態圈中，有很多出色的 SQL ORM，這些 ORM 都是為了幫助開發者能夠有更好的開發體驗，而新的 [2024 JavaScript Rising Stars](https://risingstars.js.org/2024/en) 榜單出爐了，這篇文章將會來介紹 Prisma 跟 Drizzle 這兩個熱門的 ORM，並且比較兩者有什麼差異。

## 安裝比較

這篇文章將會以 Express 來做安裝 ORM 的比較，並且只以 JavaScript 而不是 TypeScript。

一開始的 Express 專案是以最簡易的架構來安裝，現在 Express 的架構大概是這樣：

```
├── 📜 package-lock.json
├── 📜 package.json
└── 📂 src
    └── 📜 index.js
```

現在的 `index.js` 這樣可以拿到 Hello World：

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

### Drizzle

先從 Drizzle 的安裝開始，drizzle 支援許多資料庫，像是 PostgreSQL 和 MySQL 都有，這次會使用 PostgreSQL 的方式。

#### 1. 安裝 drizzle 和 pg

這裡可以直接按照官網的方式來安裝：

```bash
npm i drizzle-orm pg
npm i -D drizzle-kit @types/pg
```

#### 2. 連接資料庫

接下來在專案中新增一個 `.env` 的環境變數，裡面可以連接 PG 的 database，例如：

```
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/drizzle_database
```

安裝 dotenv 來處理環境變數：

```bash
npm i dotenv
```

然後使用 drizzle 連接 PG：

```javascript
const express = require("express");
const app = express();
const dotenv = require("dotenv");
const { drizzle } = require("drizzle-orm/node-postgres");
const { Pool } = require("pg");
dotenv.config();

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

const db = drizzle(pool);
```

#### 3. 新增 table

這裡的例子是從零開始，所以我們需要自己建立一個 schema，建議在 src 資料夾底下新增一個資料夾，命名為 `db`，並新增一個檔案 `schema.ts`：

```javascript
const { integer, pgTable, varchar, text, timestamp } = require('drizzle-orm/pg-core');

const usersTable = pgTable('users', {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  name: varchar({ length: 50 }).notNull(),
  age: integer().notNull(),
  email: varchar({ length: 255 }).notNull().unique(),
  created_at: timestamp().defaultNow(),
  address: varchar({ length: 255 }).notNull()
});

const postsTable = pgTable('posts', {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  title: varchar({ length: 100 }).notNull(),
  content: text().notNull(),
  user_id: integer().references(() => usersTable.id),
  created_at: timestamp().defaultNow(),
})

module.exports = {
  usersTable,
  postsTable
};
```

這裡我們試著建立 users 和 posts 的資料表，如果要使用各種資料庫的型別，必須從 drizzle 裡面取得相關的 function，例如 integer，就要使用 `integer()`。然後要設定關聯的話，例如 posts 跟 users 有關聯，可以在 postsTable 的 user_id 設定 `integer()`，並使用它裡面的 `references()

#### 4. 設定 Drizzle config

設定完資料的 schema 之後，還需要讓 Drizzle 知道這個專案有哪些 schema，還有連結什麼資料庫，並且設定之後 migration 產生的檔案會產生在哪裡。

所以我們需要在根目錄新增一個 `drizzle.config.js`：

```javascript
const dotenv = require("dotenv");
const { defineConfig } = require('drizzle-kit');

dotenv.config();

module.exports = defineConfig({
  out: './drizzle',
  schema: './src/db/schema.js',
  dialect: 'postgresql',
  dbCredentials: {
    url: process.env.DATABASE_URL
  }
});
```

#### 5. 更新資料庫

執行 generate 之後，就會在專案中建立一個 `drizzle` 的資料夾，並且自動新增一些 sql 的檔案。

```bash
npx drizzle-kit generate
```

接下來需要透過 ORM 去更新 PG 資料庫：

```bash
npx drizzle-kit migrate
```

接下來可以檢查 PG 資料庫，應該就可以看到資料庫新增 users 和 posts 的 table。

### Prisma

Prisma 的部分，一樣是沿用 Drizzle 所使用的 Express 架構，或者是要重新建立一個也可以。

#### 1. 安裝 Prisma

首先第一步，就是安裝 Prisma ORM：

```bash
npm install prisma @prisma/client
```

這一步安裝，跟 Drizzle 不一樣的是，它會幫專案自動產生 `.env` 和 prisma 的資料夾，而且裡面自動產生一個 `schema.prisma` 的特殊檔案，並且預設是連結 postgresql 的設定，當然 Prisma 也提供了很多不同資料庫，有興趣的朋友可以再考慮切換。

#### 2. 連接資料庫

接下來要連接資料庫，就在 Prisma 幫我們產生的 `.env` 環境變數中接上資料庫位置。

#### 3. 新增 table

前面的步驟在 prisma 的資料夾中，產生了一個 `schema.prisma` 的檔案，這個檔案如名稱所示，就是拿來放資料庫的 schema，如果是按照前面的 users 和 posts 兩個 table 架構的話，會是這樣：

```
model posts {
  id         Int       @id @default(autoincrement())
  title      String    @db.VarChar(100)
  content    String
  user_id    Int?
  created_at DateTime? @default(now()) @db.Timestamp(6)
  users      users?    @relation(fields: [user_id], references: [id], onDelete: NoAction, onUpdate: NoAction, map: "posts_user_id_users_id_fk")
}

model users {
  id         Int       @id @default(autoincrement())
  name       String    @db.VarChar(50)
  age        Int
  email      String    @unique(map: "users_email_unique") @db.VarChar(255)
  created_at DateTime? @default(now()) @db.Timestamp(6)
  address    String    @db.VarChar(255)
  posts      posts[]
}
```

#### 4. 更新資料庫

建立完 table 的架構之後，要執行 migrate 讓本地的資料庫做更新：

```bash
npx prisma migrate dev --name "init"
```

執行完之後，就會在 prisma 資料夾裡面產生 migrations 的紀錄，並且 postgresql 的資料庫也會新增相對應的 table。

## 實作 API

以上是安裝兩種 ORM 的差別，接下來我們來試著在 Express 上實作 API，做新增跟讀取 USER 的功能。
首先我們先看 Drizzle 的做法：

### Drizzle

```javascript
// index.js

const { usersTable, postsTable } = require("./db/schema");

app.use(express.json());

app.get("/users", async (req, res) => {
  const users = await db.select().from(usersTable);
  res.json(users);
});

app.post("/users", async (req, res) => {
  const user = await db.insert(usersTable).values({
    name: req.body.name,
    age: req.body.age,
    email: req.body.email,
    address: req.body.address
  });
  res.status(201).json({ message: "新增成功" });
});

app.post("/posts", async (req, res) => {
  const post = await db.insert(postsTable).values({
    title: req.body.title,
    content: req.body.content,
    user_id: req.body.user_id
  });
  res.status(201).json({ message: "新增成功" });
});

app.get("/posts", async (req, res) => {
  const posts = await db.select().from(postsTable);
  res.json(posts);
});
```

記得要使用 `express.json()` 不然會沒辦法正確新增 JSON 的格式。
接下來沒意外的話應該就可以正常使用 user 和 posts 的新增和讀取 API 了。

### Prisma

Prisma ORM 的方式，會需要透過 PrismaClient 來建立一個實例，並且透過它在我們設定的架構下，分別去做資料庫的操作。

```javascript
const express = require("express");
const app = express();
const dotenv = require("dotenv");
dotenv.config();
const { PrismaClient } = require("@prisma/client");

const prisma = new PrismaClient();

app.use(express.json());

app.get("/users", async (req, res) => {
  const users = await prisma.users.findMany();
  res.json(users);
});

app.post("/users", async (req, res) => {
  const user = await prisma.users.create({
    data: {
      name: req.body.name,
      age: req.body.age,
      email: req.body.email,
      address: req.body.address
    }
  });
  res.status(201).json({ message: "User created" });
});

app.get("/posts", async (req, res) => {
  const posts = await prisma.posts.findMany();
  res.json(posts);
});

app.post("/posts", async (req, res) => {
  const post = await prisma.posts.create({
    data: {
      title: req.body.title,
      content: req.body.content,
      user_id: req.body.user_id
    }
  });
  res.status(201).json({ message: "Post created" });
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

## 比較

### 安裝

安裝以及設定的部分，在 Prisma 比較簡單，只要下一個指令，就會自動在專案中自動建立一些檔案，例如 schema 以及相關的設定檔；相反 Drizzle 就稍微比較麻煩一點。

### schema

兩邊在 schema 的使用各有優缺點：

#### Prisma

- 優點：有一個獨有的檔案來管理，視覺上方便閱讀，對於 table 的架構一目了然，型別以及是否必填也蠻清楚。

![](https://i.imgur.com/P5c3cHr.png)

- 缺點：不太好撰寫，如果是想要在 `schema.prisma` 手寫的話有點學習成本，需要按照 Prisma 特有的語法去寫。

#### Drizzle

- 優點：寫法就是一般的 JS，而且跟原本的 SQL 非常相似，學習成本比較低。

![](https://i.imgur.com/boVP7JB.png)

- 缺點： 架構上跟 Prisma 相比，比較沒這麼好閱讀，但熟悉 JS 的開發者應該也能很快上手。

### 操作

在操作上，Drizzle 跟原本 SQL 語法非常類似，這對於熟悉 SQL 的開發者來說，是一個巨大的優勢。

![](https://i.imgur.com/dOXHmhL.png)

Prisma 有自己的方法，所以勢必要學習一下，可能會有一些陣痛期。

![](https://i.imgur.com/q2sayGl.png)

## 結論

以目前的 JS 生態來說，兩個 ORM 都是很棒的工具。Prisma 目前在 GitHub 擁有 40.7k 的星星數，而 Drizzle 則是擁有 25.7k 的星星數。
而且目前兩者的下載量明顯有一些差距，代表著 Prisma 是比較熱門的選項。
![npm trends](https://i.imgur.com/Lf95O0R.png)
> 圖片來源 npm trends

但 Drizzle 其實是非常年輕的 ORM，根據 [Drizzle 官網](https://orm.drizzle.team/docs/latest-releases)頁面顯示，最早是在 2022 年 7 月推出。在這之前的 [JavaScript Rising Stars](https://risingstars.js.org/2024/en) 報告中，都是 Prisma 在這個榜單上，Drizzle 出現之後就看似取代了 Prisma 了，是一個可以觀察並且參考的部分。

在開發選擇的情境上，如果團隊比較熟悉 SQL 的話，那麼選擇 Drizzle 是一個比較好的選擇，如果說想要有比較完整的工具鏈，那麼可以考慮選擇 Prisma。