---
title: React Search Photo Demo
date: 2021-07-11 22:11:26
tags:
  - react
  - sideproject
categories: 開發筆記
keywords:
  - react
  - hook
  - useEffect
  - useState
  - axios
decription: 在 Vite 使用 React，練習透過 Unsplash API 來搜尋圖片。
---

本次練習要在 Vite 上使用 React，並實作一個搜尋表單，輸入文字並透過 Unsplash API 來搜尋並顯示圖片。

<!-- Read Mode -->

## 前置作業

使用 Vite 建立專案，並使用 Semantic UI 的 CDN 來做基本的美化：

* Vite 建立專案 `react-form-demo`

```bash
$ yarn create @vitejs/app react-form-demo --template react
$ cd react-form-demo
$ yarn
```

* 在 index.html 貼上 Semantic UI CDN Link

```html
https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.4.1/semantic.min.css
```

* 完成基本介面

```jsx
// App.jsx
import React from 'react'
import SearchBar from './components/SearchBar'

function App() {
  return (
    <div className='ui container' style={{ marginTop: 20 }}>
      <SearchBar />
    </div>
  )
}

export default App
```

```jsx
// .components/SearchBar.jsx
import React from 'react'

const SearchBar = () => {
  return (
    <div className='ui segment'>
      <form className='ui form'>
        <div className='field'>
          <label htmlFor='search'>Search</label>
          <input id='search' type='text' />
        </div>
      </form>
    </div>
  )
}

export default SearchBar
```

## 測試輸入功能

1. 建立一個 function，並 `console.log` 出輸入結果

```js
const handleInput = (e) => {
    console.log(e.target.value)
}
```

2. 放到 `<input>` 中，要注意的是，是使用 `onChange`

```jsx
<input id='search' type='text' onChange={handleInput} />
```

3. 這時候輸入的結果，就會在 `console.log` 顯示

## 把輸入的結果放到 state 中管理

1. 使用 `useState` 這個 Hook，建立一個 state

```jsx
import React, { useState } from 'react'

const [state, setState] = useState({ inputText: '' })
```

2. 把輸入的結果，放到 `inputText`

```jsx
const handleInput = (e) => {
    setState({ inputText: e.target.value })
}
```

3. 使用 React Developer Tools，就可以看到 `inputText` 顯示的是輸入的字串

![](https://i.imgur.com/rMiR8vM.png)


4. 在 `<input>` 加上 `value={state.inputText}`

## 讓 App 可以跨組件拿到 SearchBar 輸入的資料

這一步驟是要在 `App` 建立一個函式，並透過 `props` 來讓 `SearchBar` 回傳輸入的結果。

1. 在 `App` 建立函式

```jsx
const onSearchSubmit = (input) => {
    console.log(input)
}
```

2. 在 `SearchBar` 傳入 `props`

```jsx
<SearchBar handleSubmit={onSearchSubmit} />
```

在 `SearchBar` 建立一個函式，讓輸入的結果，透過 submit 傳到 `App`

1. 先透過解構從 props 取出 `handleSubmit`，以利後面操作

```js
const { handleSubmit } = props
```



2. 在 `SearchBar` 建立函式，讓 `handleSubmit` 傳入輸入的資料，記得加入 `preventDefault` 以避免重整頁面

```jsx
const handleFormSubmit = (e) => {
    e.preventDefault()
    handleSubmit(state.inputText)
}
```

3. 在 `<form>` 使用 `onSubmit`

```jsx
<form className='ui form' onSubmit={handleFormSubmit}>
```

4. 可以在 `console.log` 看到是來自 `App` 的

![](https://i.imgur.com/bjaYelB.png)

## 將從 SearchBar 來的 State 存到 App 本身的 State

1. 建立 state

```jsx=
import React, { useState } from 'react'

const [inputState, setInputState] = useState({ text: '' })
```

2. 傳進 state 中

```jsx=
const onSearchSubmit = (input) => {
    setInputState({ text: input })
}
```

3. 透過 React Developer Tools 看到確實拿到輸入的值

![](https://i.imgur.com/JiO1DJE.png)



## 申請 Unsplash 金鑰

首先到 [Unsplash Developers](https://unsplash.com/developers)，申請帳號並登入（這部分就不仔細講解了）。

登入後請按上方的 Your apps，並且點擊 New Application。

![image-20210709231339764](https://i.imgur.com/T6lHzJk.png)

然後會有一些使用者條款，全部勾選再按接受就對了。

![image-20210709231926292](https://i.imgur.com/H82FP39.png)

接著請輸入名稱，並且簡單描述一下。

![image-20210709232223428](https://i.imgur.com/WQGdxEo.png)

然後就會進來你剛剛申請的專案，接著往下拉，最重要的就是你的 Access Key，之後就可以藉由這組 Key 透過 API 找圖片。

![image-20210709232601364](https://i.imgur.com/9iUGAMX.png)

接著去 Documentation 看文件，因為我要做搜尋的功能，所以按左邊的 Search photos，這邊就有待會要使用的相關功能，建議可以多看看。

![image-20210709233718975](https://i.imgur.com/fo5ne09.png)

## 搜尋關鍵字透過 API 拿到資料

* 安裝 axios

```bash
$ yarn add axios
```

* 使用 axios 管理 API

  新增一個 api 資料夾，並且新增一個 `unsplash.js` 來管理 API，在 Authorization 放上剛剛申請的 Access Key，下方範例中，`xxxxx` 那一段就是放 Key 的地方。

```js
import axios from 'axios'

export default axios.create({
  baseURL: 'https://api.unsplash.com',
  headers: {
    Authorization: 'Client-ID xxxxx'
  }
})
```

* 連結 API 測試

  import 剛剛建立的 unsplash，並且使用 `async...await` 連結 `/search/photos` 

```jsx
// App.jsx
import unsplash from '../api/unsplash'

const onSearchSubmit = async (input) => {
	const res = await unsplash.get('/search/photos', {
		params: {
    	query: input
    }
  })
  console.log(res.data) // 透過 console.log 觀察拿到哪些資料
}
```

* 將拿到的圖片資料放入 state 中管理

  使用 `useState` 建立一個陣列，將拿到的資料放入

```jsx
import React, { useState } from 'react'

const [state, setState] = useState({ images: [] })

const onSearchSubmit = async (input) => {
	const res = await unsplash.get('/search/photos', {
  	params: {
    	query: input
    }
	})
  setState({ images: res.data.results })
}
```

透過 React Developer Tools 看到 images 確實有灌進資料

![image-20210710204127741](https://i.imgur.com/ScDVubm.png)

## 將圖片顯示出來

* 建立新元件 - ImageList，並且在 App 匯入
* 利用 props 傳圖片資料給 ImageList

```jsx
// App..jsx
import ImageList from './ImageList'

<ImageList images={state.images} />
```

* 在 ImageList 拿到 props 傳來的資料，使用 map 迴圈建立一個圖片列表

```jsx
// ImageList.jsx
const ImageList = (props) => {
  const { images } = props  // 利用物件解構從 props 拿出
  
  const renderImage = images.map((image) => {
    return (
      <img
        className='ui medium rounded image'
        src={image.urls.regular}
        alt={image.description}
        key={image.id}
      ></img>
    )
  })
  
  return <div>{renderImage}</div>
}

export default ImageList
```

這樣基本上就大致完成了，可以看 [Demo](https://bucky0112.github.io/search_photo_app/)，有興趣可以到我的[專案](https://github.com/bucky0112/search_photo_app)。

