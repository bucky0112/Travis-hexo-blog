---
title: 使用 Jest 和 React Testing Library 進行測試的方法
tags:
---

## 什麼是測試？以及為什麼需要？

好的，什麼是測試？

測試的目的主要是想驗證我們所開發出來的東西是否按照預期的功能運作。

測試也是確保軟體品質的重要步驟，在撰寫完程式碼之後對其進行檢查，以確保功能是正確的。
而測試也可以讓開發者在過程中確定自己的開發是否正確，既可以增加開發者的信心，也可以讓開發者在開發過程中更加清楚自己的開發方向。

當然我們可以透過手動測試來驗證，但是這樣的話會花費大量的時間，而且也不夠精準，因此我們需要一個自動化的測試工具，來幫助我們測試。

因為測試因為目的的不同，所以也有不同的種類，例如有分成 Unit Test、Integration Test、End-to-End Test 等等。當然還有其他的種類，但主要是以這三種為最常見，也最為流行的種類。

## Jest 和 React Testing Library

### Jest

[Jest](https://github.com/facebook/jest) 是一個由 Meta 所維護的 JavaScript 單元測試框架，由於提供了基於 JavaScript 的測試環境，適合讓開發 JavaScript 的程式進行測試，例如三大前端框架 React、Vue、Angular 都可以使用。它具有自動化測試、快速執行測試、簡單的斷言 API 等功能。 功能非常地全面。截止目前為止在 GitHub 的星星數高達 41.4k，是一個非常受歡迎的測試框架。

### React Testing Library

[React Testing Library](https://github.com/testing-library/react-testing-library) 是一個用於測試 React 應用程序的 library，它主要在模擬用戶在使用應用程序時的操作，以便更好地測試頁面元素和互動。它結合了 Jest 和 DOM 操作，提供了簡單而易用的 API，可以快速測試 React 元件的行為。

## 要測試的範例

以下是一個簡單的範例，主要是一個輸入框，還有一個按鈕，當我們輸入文字後，按下按鈕，就會在清單上顯示出我們輸入的文字。

<iframe src="https://codesandbox.io/embed/ecstatic-sun-sigkc7?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="ecstatic-sun-sigkc7"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## 開始測試

開始測試前要先安裝 Jest 和 React Testing Library，如果想跟著一起做的話，建議可以直接使用 Create React App (CRA) 來建立專案，因為 CRA 內建了 Jest 和 React Testing Library，所以我們不需要額外安裝，就可以直接開始測試了。

## 測試畫面是否正確

在這篇文章中，我們先測試表單的部分。列表的部分我們會在下一篇文章中測試。

首先我們要測試的是畫面的顯示是否符合我們預期的結果，也就是預期應該會出現一個輸入框和一個按鈕。

但第一步我們要先建立一個測試檔案，首先建立一個 `__tests__` 的資料夾，然後在裡面建立一個 `Form.test.js` 的檔案，這個檔案就是我們的測試檔案。或者是可以直接在該檔案的同一層建立一個 `Form.test.js` 的檔案，這樣也可以。

在這個檔案中，我們要先引入 React Testing Library，然後再引入我們要測試的元件，這邊我們要測試的元件就是 `Form.js`。

```jsx
import { render, screen } from "@testing-library/react"
import Form from "./Form"

test("it shows one inputs and a button", () => {
  render(<Form />);
  const inputs = screen.getAllByRole("textbox");
  const button = screen.getByRole("button");
  expect(inputs.length).toBe(1);
  expect(button).toBeInTheDocument();
})
```

首先，要先導入 React Testing Library 的 `render` 和 `screen` 函式。`render` 用於在測試環境中渲染組件，而 `screen` 提供了一組函式，可以更簡單地訪問渲染的組件內容。

然後，定義了一個名為 "it shows one inputs and a button" 的測試案例。這裡也可以用中文，只要方便測試者閱讀就好。這個案例使用 `render` 將 "Form" 組件渲染到測試環境中，然後使用 `screen.getAllByRole` 和 `screen.getByRole` 尋找頁面上的輸入框和按鈕元素。

最後，它使用 `expect` 函式，也就是斷言來檢查文本框的數量是否為 1，以及按鈕是否存在於頁面上。如果這些斷言都通過了，則代表這個測試案例成功；否則，這個測試案例將失敗。

然後在終端機中執行 `npm test` 或是 `yarn test`，就可以看到測試結果了。

## 測試提交表單

```jsx
test("it calls atAddUser when the form is submitted", () => {
  const mock = jest.fn()
  render(<UserForm atAddUser={mock} />);

  const nameInput = screen.getByRole("textbox", { name: /name/i })

  user.click(nameInput);
  user.keyboard("John Doe");

  const button = screen.getByRole("button");
  user.click(button);

  expect(mock).toHaveBeenCalled()
  expect(mock).toHaveBeenCalledWith({ name: "John Doe" })
})
```

這個測試用於檢查用戶表單的提交行為是否正常，並確保當表單提交時觸發了預期的回調函數。

首先，使用 Jest 創建了一個空的函數 `mock` 並將其作為參數傳遞給組件 UserForm。

然後，使用 `screen.getByRole` 函數查找到名稱包含 “name” 的輸入框，並對其進行了點擊操作，模擬用戶在其中輸入 “John Doe”。

接下來，使用 `screen.getByRole` 函數查找按鈕元素，並進行點擊操作，模擬用戶提交表單。

最後則是使用 `expect` 断言驗證了函數 `mock` 是否被調用，以及傳遞給它的參數是否為 { name: "John Doe" }。

## 測試按下按鈕後清除輸入框文字

```jsx
import user from "@testing-library/user-event";

test("it clears the form after submission", () => {
  render(<UserForm onUserAdd={() => {}} />);

  const nameInput = screen.getByRole("textbox", { name: /name/i })
  const button = screen.getByRole("button");

  user.click(nameInput);
  user.keyboard("John Doe");

  user.click(button);

  expect(nameInput).toHaveValue("")
})
```

這個測試是為了測試使用者表單在送出後會清空。首先要使用 `user` 來模擬使用者在 nameInput 中輸入 "John Doe"，再按 button。
最後，會試著檢查 nameInput 是否已清空。
