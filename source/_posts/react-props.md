---
title: React 的 props 運用
tags:
  - react
  - component
  - props
date: 2020-11-26 19:19:24
categories: React 學習系列
keywords:
  - react
  - component
  - props
decription: React 用 props 傳遞資料
---
在 React 中，元件之間資料的傳遞都是由父元件傳給子元件的單向數據流，本篇文章將會講到如何使用 props 來傳遞資料。
<!--more-->

上一篇講到 component 的使用，但是有些時候想要另外客製化不一樣的內容的話，不就還要做出好幾個不一樣的 component 嗎？

例如下面的 Codesandbox 有兩個 button，按下按鈕就會跳出訊息。

<iframe src="https://codesandbox.io/embed/weathered-tdd-wkmqr?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="weathered-tdd-wkmqr"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

但是如果想要做出一個 component，然後內容不要寫死可以自定義的情況下，該怎麼做呢？

## props 的運用

這時候就可以透過 props 來做出想要的效果，例如在 CustomButton.js 這個 component 中：

1. 加入參數 props。

```react
const CustomButton = (props) => {
  return <button></button>;
};
```

2. 定義一個變數並使用它，等等可以在父層輸入客製化內容。

```react
const CustomButton = (props) => {
  const msg = props.text;
  return <button>{msg}</button>;
};
```

3. 做出 alert，設定客製化內容。

```react
const CustomButton = (props) => {
  const msg = props.text;
  return <button onClick={props.handleClick}>{msg}</button>;
};
```

4. import 這個 CustomButton.js，並帶入我們想要顯示的內容。

```react
<CustomButton
	handleClick={() => {
  	window.alert("I'm OK!");
  }}
  text={"OK"}
/>
```

最後的 Demo 就像這樣：

<iframe src="https://codesandbox.io/embed/goofy-kare-xv6nw?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="goofy-kare-xv6nw"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## 解構賦值的寫法

在使用 props 時的寫法，也可以使用解構賦值，把需要的變數從 props 中取出來，例如：

```react
// 子層元件
const CustomButton = (props) => {
  const { msg, handleClick } = props;
  return (
    <button onClick={handleClick}>
      {msg}
    </button>
  );
};
```



```react
// 父層元件
<CustomButton 
  handleClick={() => {
		window.alert("Hello React");
	}}
	msg="Hello"
/>
```

