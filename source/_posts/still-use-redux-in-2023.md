---
title: 淺談在 2023 年的 React 還需要 Redux 嗎？
date: 2023-05-20 11:27:10
tags:
  - redux
  - redux toolkit
  - react
categories: 好用函式庫介紹
keywords:
  - redux
  - redux toolkit
  - react
decription: 淺談在 2023 年的 React 還需要 Redux 嗎？  
---
# 淺談在 2023 年的 React 還需要 Redux 嗎？

前幾天在推特上看到有人提問標題這個問題，有些網友回覆 Redux 太複雜，建議改用其他的狀態管理套件。也有網友提出 React 都有 context API 來處理跨元件狀態了，根本不需要 Redux。

在這個問題上，許多開發者可能會有不同的觀點，但讓我們先從最基本的 React context 開始比較，然後再探討 Redux。

首先，我們來看看 React 的 context API。React context 提供了一種方式來共享值，例如這些值是全域的，例如主題、語言偏好等等，而不需要手動將 `props` 傳遞過多層。然而，當我們使用 `useContext` 並搭配 `useReducer` 來作為全域狀態管理時，所有使用該 context 的元件將會在 state 變更時重新 render，不論他們使用的 state 部分是否有改變。如果你有一個沒有被 `useCallback` 包裹的處理器，那麼虛擬 DOM 比對將會失敗，這些元件將會導致 DOM 更新。當然你可以有多個小的 contexts，但這樣你就會有多個狀態的來源，這可能還是得根據你的需求和實作方式來判別這樣是好是壞。

另一方面，Redux 提供了一個集中式和預測性的 state 容器，讓你可以在整個應用程式中管理狀態。儘管在引入 hooks 和 context API 之後，React 提供了一種方式來在組件之間共享狀態，但 Redux 仍然有其價值。Redux 的主要優勢在於它的嚴謹的更新邏輯：所有的狀態更新都被集中處理並通過 actions 和 reducers 來描述。這種方法提供了很高的可預測性和一致性，這在複雜的應用程式中是很有價值的。

不過 Redux 出了名困難的學習曲線，對於開發者來說也是一個望而卻步的門檻。我之前也寫了一篇介紹 Redux 的文章，有興趣的朋友可以看我之前的[這篇文章](https://bucky0112.github.io/2022/03/10/libraries-redux/)去看看。

為了解決這個問題，Redux 也推出了 Redux Toolkit(RTK) 來進一步簡化 Redux 的使用。RTK 是 Redux 的官方工具集，其主要目標是解決 Redux 過於複雜的問題，並且提供了一組工具來協助你更簡單地寫 Redux，所以開發者可以寫出更精簡、更可讀的 Redux 代碼，並且有更好的開發者體驗。

RTK 提供了 `configureStore()` 函數來建立 store，這比過去的 `createStore()` 提供了更多的好處。`configureStore()` 會自動添加非同步的能力，並且支援 [Redux Devtools Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)，這可以讓開發者在瀏覽器開發者工具輕鬆看到狀態的變更，當然還有其他的好處，例如我們不再需要 `combineReducers()`，以下是用 RTK 所做的一個簡單的範例：

<iframe src="https://codesandbox.io/embed/naughty-waterfall-ibmvxr?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="naughty-waterfall-ibmvxr"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>



因此，儘管 React 的 context 提供了簡易的方式來處理跨元件的狀態，但從上面的 RTK 的範例來看，在不失去原本 Redux 強大的狀態管理之下，卻簡化了很多繁複的設定。

## 結論

React Context API 是一種非常有效的工具，它可以處理許多跨元件的狀態共享問題。然而，對於大型或複雜的應用程式來說，Redux 提供的集中式狀態管理能力可能會更適合。尤其是當你需要更嚴謹的狀態更新邏輯，或是與其他工具（例如 Redux DevTools）的搭配時。

而 Redux Toolkit 是 Redux 的現代化版本，它簡化了 Redux 的許多繁複部分，並加入了許多有用的功能，例如自動處理 action 和 reducer 的創建、處理異步邏輯和狀態的不可變性。因此，對於需要複雜狀態管理且尋求簡化開發體驗的專案，Redux Toolkit 是一個很好的選擇。

總的來說，選擇使用 Redux 或是 React Context，或是其他的狀態管理工具，應該基於你的專案需求，並考慮到每種工具的優缺點。在某些情況下，Redux 提供的嚴謹的狀態管理和健全的開發者工具可能是必要的；而在其他情況下，React Context 的輕量級和靈活性可能就足夠了。所以，在你選擇工具的時候，最重要的是理解你的專案需求和這些工具的適用情境。
