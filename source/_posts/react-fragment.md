---
title: React Fragment 的標籤好適合懶人工程師啊～
tags:
  - react
  - react fragment
  - jsx
date: 2020-10-16 00:08:31
categories: React 學習系列
keywords:
  - react
  - react fragment
  - jsx
decription:
---
在使用 Vue 的 template 模板時，如果有多個 HTML 標籤的話，就必定需要一個 `<div>` 包起來才能運行。而 React 在 JSX 中也是一樣的，只是它多了一個選擇可以用，而且更精簡，看起來就跟沒有一樣呢～
<!--more-->

## JSX 只能夠有一個最外層元素

JSX 只能夠有一個最外層元素是什麼意思？就像下面的例子中，如果把 `<div className="container">` 給拿掉的話就會無法運作，所以一定要有一個 `<div>` 在最外層包起來。

```js
const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div className="container">
      <div
        className={`plus ${count >= 10 && "hidden"}`}
        onClick={() => setCount(count + 1)}
      >
        +
      </div>

      <div className="num">{count}</div>

      <div
        className={`minus ${count < 1 && "hidden"}`}
        onClick={() => setCount(count - 1)}
      >
        -
      </div>
    </div>
  );
};
```

## 試試看 React Fragment 吧

有些人對於多包一層 `<div>` 可能會覺得看起來很累贅，不舒服（我是還好啦）。沒關係，React 似乎聽到這些人的需求了，於是給了一個解決方案，

![react fragment](https://i.imgur.com/jEtqR7p.jpg)

就是 React Fragment 的標籤，那怎麼使用呢？
非常簡單，就跟一般的 `<div>` 標籤一樣，用 `<React.Fragment></React.Fragment>` 包起來就可以了。

```js
const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <React.Fragment>
      <div
        className={`plus ${count >= 10 && "hidden"}`}
        onClick={() => setCount(count + 1)}
      >
        +
      </div>

      <div className="num">{count}</div>

      <div
        className={`minus ${count < 1 && "hidden"}`}
        onClick={() => setCount(count - 1)}
      >
        -
      </div>
    </React.Fragment>
  );
};
```

你如果懶人性格發作的話，你就直接用 `<></>` 來包也可以。完美符合工程師的要求。

![1593052250482](https://i.imgur.com/ryCI33O.jpg)

## 這樣跟 `<div>` 有什麼差異？

可能會有人有疑問說，欸不是啊，這樣跟用 `<div>` 有什麼差別？
有差啦，哪次沒有差，檢查一下元素就會發現原本使用 `<div>` 的話就會在外層有一個包著。

![AF0805A8-75EE-46C1-9F15-8397761C8CD4](https://i.imgur.com/HDXDiaI.png)

而使用 React Fragment 的標籤則是不會有無用的 `<div>` 干擾閱讀，看起來清爽許多。

![25B7101D-1543-476D-9C5A-8FE85A456382](https://i.imgur.com/zfxt6yM.png)

這東西 Vue 好像沒有類似的功能，不過有查到有人使用指令來解決這個需求，詳情請看 => [連結](https://zhuanlan.zhihu.com/p/141238031) 