---
title: 使用 Jest 和 React Testing Library 進行測試的方法 - 測試列表
date: 2023-02-11 15:12:51
tags:
 - jest
 - react testing library
categories: React 測試
keywords:
 - jest
 - react testing library
decription: 介紹如何使用 Jest 和 React Testing Library 進行測試列表
---

## 前言

本文將涵蓋測試列表、測試列表內容、以及測試 App 元件。我們依然會使用 Jest + React Testing Library 來實現測試。

## 測試列表

接下來接續上一篇未完成的測試，我們來測試一個列表的元件，這個元件會接收一個陣列，並且將陣列中的每個元素渲染成一個列表項目。

如果不想翻到上一篇文章，可以直接看這個 Demo。

<iframe src="https://codesandbox.io/embed/ecstatic-sun-sigkc7?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="ecstatic-sun-sigkc7"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

那麼首先在測試之前，要先來看一下這個元件預期會怎麼呈現。

預期會透過 props 傳來一個 `users` 這個陣列，並且將陣列中的每個元素渲染成一個列表項目。

必須要確保是否正確呈現列表項目，所以我們可以這麼寫測試：

```js
import { render, screen, within } from '@testing-library/react'
import List from "./List"

test('it renders a list of users', () => {
  const users = [
    { name: 'John Doe' },
    { name: 'Jane Doe' }
  ]

  render(<List users={users} />)

  const list = within(screen.getByTestId('users')).getAllByRole('listitem')
  expect(list).toHaveLength(2)
})
```

首先我們先模擬一個 `users` 陣列的資料，並且將資料傳入到 `List` 元件中。

接著我們要知道 `List` 是不是正確渲染出來，我們到這個步驟可以先到 `List` 元件的 `ul` 標籤中加上 `data-testid="users"` 這個屬性，這樣我們就可以透過 `screen.getByTestId('users')` 來取得這個元素。

然後就可以透過 `within` 函式來取得 `ul` 標籤中的所有 `li` 標籤，並且透過 `getAllByRole` 來取得 `li` 標籤中的所有 `listitem`。

最後就可以透過 `toHaveLength` 來確認陣列的長度是否正確。

## 測試列表內容

這裡我們要來測試是不是有正確顯示內容，預期應該是要顯示名字出來。

然後因為應該會使用到一樣的模擬資料，所以我們可以先把上一個測試的 `users` 陣列抽出來，並且將它放在 `renderComponent` 函式中。

```js
const renderComponent = () => {
  const users = [{ name: 'john doe' }, { name: 'jane doe' }]

  render(<List users={users} />)

  return { users }
}
```

接著放入測試中：

```js
test('it renders the user name', () => {
  const { users } = renderComponent()
  users.forEach((user, index) => {
    const name = screen.getByTestId(`name-${index}`)
    expect(name).toBeInTheDocument()
  })
})
```

首先通過呼叫 `renderComponent` 函式呈現元件，並使用 `forEach` 函式跑一遍用戶列表，並使用 `screen.getByTestId` 函式查找具有 "data-testid" 屬性為 "name-index" 的元素，並使用 `index` 來當用戶的索引。

最後，通過使用 `toBeInTheDocument` 函式驗證元素是否存在，確認每個用戶的 `name` 是不是正確地呈現。

## 測試 App

最後，不要忘記還有 `App.js` 這個元件，因為裡面會是預設的測試內容，但我們已經有加入了兩個元件，所以現在跑測試的話，會發現 `App.js` 這個元件會失敗。

這個測試比較像是整合測試，因為我們要確認 `App.js` 這個元件是否正確地將 `List` 的內容和 `Form` 的動作呈現出來。

```js
import { render, screen } from '@testing-library/react';
import user from "@testing-library/user-event";
import App from './App';

test('can receive a new user and show it on a list', () => {
  render(<App />)

  const nameInput = screen.getByRole("textbox", { name: /name/i })
  const button = screen.getByRole("button");
  
  user.click(nameInput);
  user.keyboard("John Doe");
  user.click(button);


  const name = screen.getByTestId("name-0", { name: "John Doe" })
  expect(name).toBeInTheDocument()
})
```

首先使用 `screen.getByRole` 函式查找具有 "role" 屬性 "textbox" 且名稱包含 "name" 的元素，並將其儲存在 `nameInput` 變數中。

接下來，使用 `screen.getByRole` 函式查找具有 "role" 屬性 "button" 的元素，並將其儲存在 `button` 變數中。

然後，使用 `user.click` 函式模擬對 `nameInput` 元素的點擊，並使用 `user.keyboard` 函式向其中鍵入 "John Doe"。

接下來，使用 `user.click` 函式模擬對 `button` 元素的點擊。

最後，使用 `screen.getByTestId` 函式查找具有 "data-testid" 屬性 "name-0" 且名稱為 "John Doe" 的元素，並使用 `toBeInTheDocument` 函式驗證其是否在文件中存在。

以上就是簡單的展示一下如何測試 React 元件，感謝閱讀。
