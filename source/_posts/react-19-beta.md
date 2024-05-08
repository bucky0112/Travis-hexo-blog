---
title: React 19 Beta 新武器解鎖體驗
date: 2024-05-08 17:26:20
tags:
 - react
 - react 19
categories:
 - React
keywords:
 - react
 - react 19
decription: 介紹 React 19 Beta 新 Hooks。
top_img: https://i.imgur.com/ZFWUjPq.jpeg
---
經過長時間的等待，自從 React 18 在 2022 年推出以來，React 19 Beta 版終於在近期與開發者見面了。作為 React 18 的下一個主要版本，React 19 帶來了許多讓開發者心動的新特性，這次我們就先來一探究竟有什麼新的功能。

## useTransition

在前端操作中，經常會碰到非同步的執行，例如：使用者點擊按鈕後，需要等待 API 回傳資料後再處理下一步的動作。

以下是一個範例，當使用者輸入名字後，點擊按鈕，會呼叫 API 更新名字，並且在更新過程中，按鈕會變成不可點擊的狀態。`updateNameAPI` 這個函式會模擬 API 的行為。

```jsx!
import { useState } from 'react'

const updateNameAPI = async (newName) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const shouldError = newName.includes('error')
      if (shouldError) {
        reject(`名字不能包含"error"`)
      } else {
        resolve(`你的新名字是: ${newName}`)
      }
    }, 1000)
  })
}

const Pending = () => {
  const [name, setName] = useState('')
  const [error, setError] = useState(null)
  const [isPending, setIsPending] = useState(false)

  const handleSubmit = async () => {
    setIsPending(true)
    const error = await updateNameAPI(name)
    setIsPending(false)
    if (error) {
      setError(error)
      return
    }
  }

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        {isPending ? '更新中...' : '更新名字'}
      </button>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  )
}
```

可以看到，這段程式碼中，我們使用了三個 `useState` 來管理狀態，並且在 `handleSubmit` 中，使用 `setIsPending` 來控制按鈕是否可點擊，以及在 API 回傳錯誤時，使用 `setError` 來顯示錯誤訊息。

在 React 19 中，我們可以使用 `useTransition` 來簡化這段程式碼：

```jsx!
import { useState, useTransition } from 'react'

const Pending = () => {
  const [name, setName] = useState('')
  const [error, setError] = useState(null)
  const [isPending, startTransition] = useTransition()

  const handleSubmit = async () => {
    startTransition(async () => { // 使用startTransition包裹非同步操作
      const error = await updateNameAPI(name)
      if (error) {
        setError(error)
        return
      }
    })
  }
  
  // startTransition 會自動處理 pending 狀態
  // 在非同步操作期間,isPending 為 true 
  // 操作完成後,isPending 變為 false

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        {isPending ? '更新中...' : '更新名字'}
      </button>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  )
}
```

這裡我們簡單地使用 `useTransition` 來取代 `setIsPending`，在 `useTransition` 每次非同步操作時，會自動幫我們處理好 pending 的狀態，讓我們不需要再手動管理。

## useActionState

前面展示了 `useTransition` 已經簡化了一些程式碼，接下來看到 `useActionState` 這個新 hook，可以更進一步簡化我們的程式碼：

```jsx!
import { useActionState } from 'react'

const UseActionStateExample = () => {
  const [error, submitAction, isPending] = useActionState(
    async (state, formData) => {
      const newName = formData.get('name')
      const error = await updateNameAPI(newName)
      if (error) {
        return error
      }
    }
  )

  const handleSubmit = (e) => {
    e.preventDefault()
    const formData = new FormData(e.target)
    submitAction(formData) // 調用 submitAction 提交表單
  }
  
  // useActionState 自動管理 error 和 isPending 狀態
  // 在非同步操作期間，isPending 為 true
  // 操作完成後，根據返回值更新 error 狀態

  return (
    <form onSubmit={handleSubmit}>
      <input name='name' />
      <button type='submit' disabled={isPending}>
        {isPending ? '更新中...' : '更新名字'}
      </button>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </form>
  )
}
```

`useActionState` 分別回傳了三個值，第一個是錯誤訊息，第二個是一個函式，用來提交表單，第三個是一個布林值，用來判斷是否正在執行中。

這個例子中，我們完全不需要 `useState`，只需要使用 `useActionState` 就可以完成前面使用 `useTransition` 所有的操作，這樣可以讓我們的程式碼更加簡潔。不過眼尖的你，可能也發現到這個例子中，使用的是 `form` 表單的操作，所以這也表示 `useActionState` 只能用在表單操作上。

## useFormStatus

在以前的 React 中，如果按鈕元件獨立出來，要讓按鈕元件知道目前表單是否正在執行中，需要透過 `props` 傳遞，這樣會讓程式碼變得複雜，不過在 React 19 中，我們可以使用 `useFormStatus` 來取得表單的狀態：

```jsx!
import { useActionState } from 'react'
import { useFormStatus } from 'react-dom'

const SubmitButton = () => {
  const { pending } = useFormStatus()
  // 從 useFormStatus 獲取 pending 狀態
  return (
    <button type='submit' disabled={pending}>
      {pending ? '更新中...' : '更新名字'}
    </button>
  )
}

// useFormStatus 讀取父 <form> 的 pending 狀態
// 無需通過 props 傳遞，降低了組件耦合

const UseFormStatus = () => {
  const [error, submitAction] = useActionState(async (state, formData) => {
    const newName = formData.get('name')
    const error = await updateNameAPI(newName)
    if (error) {
      return error
    }
  })

  const handleSubmit = async (formData) => {
    submitAction(formData)
  }

  return (
    <form action={handleSubmit}>
      <input name='name' />
      <SubmitButton />
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </form>
  )
}
```

在上面的例子中，示範了使用 `useFormStatus` 搭配 `useActionState` 來簡化表單的操作，前者可以不需要傳遞 `props` 就可以直接幫我們處理運作中的狀態，這樣後者也可以不需要取出 `isPending` 來判斷是否正在執行中。

## useOptimistic

在某些情境下，當使用者提交表單時，我們可以先假設使用者的操作是成功的，然後再等待 API 回傳結果，這樣可以讓使用者感受到更快的回饋，這個概念就是所謂的 `Optimistic UI`，在 React 19 中，我們可以使用 `useOptimistic` 來實現這個功能：

```jsx!
import { useState } from 'react'
import { useOptimistic } from 'react'

const OptimisticExample = () => {
  const [title, setTitle] = useState('React 18')
  const [optimisticTitle, setOptimisticTitle] = useOptimistic(title)

  const submitForm = async () => {
    const newTitle = 'React 19'
    setOptimisticTitle(newTitle) // 立即渲染 optimistic 狀態
    const res = await new Promise((resolve) => {
      setTimeout(() => {
        resolve(`${newTitle} Beta`)
      }, 1000)
    })
    setTitle(res) // 更新為最終結果
  }
  
  // useOptimistic 使得我們可以優雅地實現 optimistic UI
  // 先展示期望的最終狀態，等待非同步操作完成後再更新為實際結果

  return (
    <form action={submitForm}>
      <h1>{optimisticTitle}</h1>
      <button type='submit'>更新</button>
    </form>
  )
}
```

在這個例子中，我們使用 `useOptimistic` 來取代 `useState`，這樣就可以讓我們在提交表單時，先更新 UI，然後再等待 API 回傳結果，這樣可以讓使用者感受到更快的回饋。

## use

在之前版本的 React 中，如果想讀取一些外部的 API 資料，一般都會使用 `useEffect` 來處理，可以先看以下的範例：

```jsx!
import { useState, useEffect } from 'react'

const Messages = ({ messages }) => {
  return (
    <ul>
      {messages.map((message) => (
        <li key={message.id}>
          <h2>{message.title}</h2>
          <p>{message.body}</p>
        </li>
      ))}
    </ul>
  )
}

function FetchExample() {
  const [messages, setMessages] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    const fetchData = async () => {
      try {
        const res = await fetch('https://jsonplaceholder.typicode.com/posts')
        const data = await res.json()
        setMessages(data)
        setLoading(false)
      } catch (err) {
        console.error(err)
      } finally {
        setLoading(false)
      }
    }

    fetchData()
  }, [])

  if (loading) {
    return <p>⌛Downloading message...</p>
  }

  return <Messages messages={messages} />
}
```

在 React 19 中，我們可以使用 `use` 來取代 `useEffect`，這樣可以讓我們的程式碼更加簡潔：

```jsx!
import { use, Suspense } from 'react'

const fetchData = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts')
  return res.json()
}

const Messages = () => {
  const messages = use(fetchData())
  // use 讀取非同步資源，並暫停渲染直到完成

  return (
    <ul>
      {messages.map((message) => (
        <li key={message.id}>
          <h2>{message.title}</h2>
          <p>{message.body}</p>
        </li>
      ))}
    </ul>
  )
}

// use 簡化了非同步讀取資源的實現
// 不需要使用 useEffect 提高可讀性

function UseExample() {
  return (
    <Suspense fallback={<p>⌛Downloading message...</p>}>
      <Messages />
    </Suspense>
  )
}
```

## Context

最後來介紹 React 19 的 Context，這個部分算是有點小變動，以往在使用 Context 的時候，需要加上一個 `Provider`，像是 `<Context.Provider>`。

但在 React 19 中，我們可以直接使用 `Context` 來取代 `Provider`，這樣可以讓我們的程式碼更加簡潔：

```jsx!
import { createContext, useContext } from 'react'

// 新建 Context
const ThemeContext = createContext('light')

function ThemeProvider({ children }) {
  const theme = 'light'

  // 使用新語法渲染 <Context> 作為 provider
  return <ThemeContext value={theme}>{children}</ThemeContext>
}

function ThemedButton() {
  // 使用 Context
  const theme = useContext(ThemeContext)

  return (
    <button
      style={{
        backgroundColor: theme === 'light' ? 'white' : 'black',
        color: theme === 'light' ? 'black' : 'white'
      }}
    >
      I am a {theme} themed button
    </button>
  )
}

const ContextExample = () => {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  )
}
```

在這個例子中，當傳遞 `value` 為 `light` 時，`ThemedButton` 會顯示一個白色的按鈕，當傳遞 `value` 為 `dark` 時，`ThemedButton` 會顯示一個黑色的按鈕。

## 結論

React 19 Beta 還有其他功能，這篇文章只是挑出一些常用的情境可使用的 hook 和 API 來介紹，想了解更多的話，在 React 19 正式版推出再來做更加詳細的介紹。

總的來說，React 19 Beta 為開發者帶來了許多改變，讓非同步渲染、數據獲取和更新的體驗獲得極大提升。雖然這只是一個 Beta 版本，但這些新功能和改進都值得我們熱烈期待 React 19 的正式到來。最後讓我們拭目以待，並為即將到來的 React 開發新紀元做好準備吧！
