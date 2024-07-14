---
title: Next.js 如何在 App Router 中使用 Context？
date: 2024-07-14 15:57:00
tags:
  - next.js
  - context
categories:
  - React
keywords:
  - next.js
  - context
decription: 介紹如何在 Next.js 中使用 Context。
---

自從 Next.js 推出 App Router 後，讓開發者可以輕鬆使用強大的路由功能，但是同時也帶來一些在使用 React Hooks 的限制。本篇文章將探討如何在 App Router 中有效使用 `useContext` 等 hooks 來傳遞 props，並解釋為什麼在某些情況下不能在 Server Component 中使用 hooks。

## 什麼是 App Router？

App Router 是 Next.js 13.4 版本引入的一項重大更新，它徹底改變了我們使用 React 的方式。這個新的路由系統是基於 React Server Components，為開發者提供了更強大、更靈活的工具來建立高效能的網頁應用。

### App Router 的核心特性：

- React Server Components：這是 App Router 的基礎。Server Components 在 Server 端運作，而不是在 Client 端。這代表著它們可以直接使用伺服器資源（如 DataBase），而不用將這些資源暴露給 Client 端或通過網絡傳輸不必要的數據。

- 巢狀路由和 Layout：你可以輕鬆人性化地建立複雜的路由結構，如 `/news/[id]/page`，並為多個頁面定義共享的 layout，不用再設定複雜的路由設定。

- 簡化的資料取得：通過使用 React Server Components 中或從 API 取得資料，不用寫複雜的邏輯或管理狀態。

- Streaming 渲染：App Router 可以將資料從 Server Components 以 JSON chunk response 的形式傳輸到 Client 端，而不是一次性傳輸全部數據。這可以明顯升級效能和用戶體驗，特別是在網路較慢的情況下。

- 內建 SEO 支持：App Router 自動產生 meta 標籤、sitemaps 和 robots.txt 文件，可以讓搜索引擎更好地找到你的網站。

- 更靈活的靜態和動態渲染：開發者可以選擇最適合的渲染策略，實現更精準的效能優化。

### 為什麼使用 App Router？

總的來說，App Router 不僅是 Next.js 的一個新功能，它為開發者提供了建立更快、更容易維護的 Web App 的工具。
然而，正如本文接下來將討論的，這種新的開發模式也帶來了一些挑戰，特別是在使用 React Hooks 時。理解如何在 App Router 環境中正確使用 Context 等功能，對於充分利用這個強大的新工具非常重要。

## 為什麼不能在 Server Component 中使用 Hooks？

在討論具體的使用情境之前,我們需要了解為什麼不能在 Server Components 中使用 hooks。

Server Components 是在 Server 端渲染的，而 React hooks 的生命週期和狀態管理，這些在 Server 端是不存在的。因此，在 Server Components 中使用 hooks 會導致錯誤。

解決這個問題的方法是將需要使用 hooks 的部分拆分為 Client Components。只要在該 component 最上方加上 `'use client'`，這個作法就會把一個 component 標記為 Client Component，Next.js 就會知道這個 component 是在 Client 端運行的，而不是在 Server 端，所以在這裡可以安全地使用 hooks。

## 情境： 在頁面中將 Props 傳遞到 Child Component

讓我們來看一個非常常見的情況，展示如何在一個 Next.js 頁面中使用 Context 將 props 傳遞到 child component，甚至是更深層的 child component。

![20240714183523](https://i.imgur.com/Vrv5oXr.png)

第一步，我們先建立一個 Context：

```jsx
'use client'
import { createContext, useContext, useState, ReactNode } from 'react'

type ThemeContextType = {
  theme: string
  setTheme: (theme: string) => void
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined)

export const ThemeProvider: React.FC<{ children: ReactNode }> = ({
  children
}) => {
  const [theme, setTheme] = useState('light')

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export const useTheme = () => {
  const context = useContext(ThemeContext)
  if (context === undefined) {
    throw new Error('useTheme must be used within a ThemeProvider')
  }
  return context
}
```

稍微介紹一下這個 Context。一開始最重要的就是 `use client`，這樣就可以在 Client Components 中使用 hooks。接著我們建立了一個 `ThemeContext`，並且建立了一個 `ThemeProvider` 來提供 theme 的值，最後我們建立了一個 `useTheme` hook 來使用這個 Context。

接下來，在 Page 中使用這個 Context：

```jsx
import { ThemeProvider } from '@/context/themeContext'

export default function Home() {
  return (
    <ThemeProvider>
      <main></main>
    </ThemeProvider>
  )
}
```

到這一步，不管是在畫面中的哪個地方，只要是在 `ThemeProvider` 的子元素中，都可以使用 `useTheme` 來取得 theme 的值。

然後可以測試一下如果把 Context 的 `'use client'` 拿掉，會發生什麼事情。

畫面上就會出現這個錯誤：

![20240714170606](https://i.imgur.com/r08TNFR.png)

錯誤訊息會跟你說，你要使用 React 的 hook，就必須要在 Client Component 中使用，因為你不宣告 `'use client'`，所以這個 component 預設就會被認為是 Server Component。

接下來我們建立兩個 child component，分別是 `ParentComponent` 跟 `ChildComponent`：

```jsx
'use client'
import { useTheme } from '@/context/themeContext'
import ChildComponent from './Child'

export default function ParentComponent() {
  const { theme } = useTheme()

  return (
    <div>
      <p>Current theme: {theme}</p>
      <ChildComponent />
    </div>
  )
}
```

```jsx
'use client'
import { useTheme } from '@/context/themeContext'

export default function ChildComponent() {
  const { theme, setTheme } = useTheme()

  return (
    <button
      type='button'
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
    >
      Toggle Theme
    </button>
  )
}
```

然後在 Home Page 中 import `ParentComponent`：

```jsx
import ParentComponent from '@/components/Parent'

export default function Home() {
  return (
    <ThemeProvider>
      <main>
        <ParentComponent />
      </main>
    </ThemeProvider>
  )
}
```

透過以上的方法，我們就可以做到不管在哪個 component 中，只要是在 `ThemeProvider` 的子元素中，都可以使用 `useTheme` 來取得 theme 的值，並且可以透過 `setTheme` 來改變 theme 的值，這樣就可以在 Next.js 中使用 Context 來傳遞 props。

下面提供給大家一個 CodeSandbox 試玩：

<iframe src="https://codesandbox.io/p/devbox/boring-mcclintock-fy8wqn?file=%2Fapp%2Fpage.tsx&embed=1"
     style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;"
     title="boring-mcclintock-fy8wqn"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
