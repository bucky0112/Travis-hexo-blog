---
title: react-native-event-handle
tags:
---

## 事件處理

React Native 的事件處理跟網頁的事件處理有點不一樣，網頁的事件處理是直接在 HTML 標籤上加上事件處理的屬性，而 React Native 則是在元件上加上事件處理的屬性。

### 事件處理的屬性

TextInput 的事件處理屬性：

- onChangeText

```jsx
const handleInput = (text) => {
  console.log(text)
}

;<TextInput
  style={{ height: 40 }}
  placeholder='Type here to translate!'
  onChangeText={handleInput}
/>
```

Button 的事件處理屬性：

- onPress

```jsx
const handlePress = () => {
  console.log('press')
}

;<Button onPress={handlePress} title='Press Me' />
```
