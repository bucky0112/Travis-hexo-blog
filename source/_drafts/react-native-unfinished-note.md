---
title: react-native-unfinished-note
tags:
---

## components 比較

| Web Browser | Android Native | iOS Native  | React Native |
| ----------- | -------------- | ----------- | ------------ |
| <div>       | android.View   | UIView      | <View>       |
| <input>     | EditText       | UITextField | <TextInput>  |
<!-- |             |                |             |              | -->

<View> 是一個容器，可以包含其他元件，也可以設定樣式。
但跟 <div> 不同的是，不能把文字直接放在 <View> 裡面，要用 <Text>。
因為 React Native 並沒有直接支援 HTML，所以要用 React Native 的元件。而且更加的嚴格，不能隨便放東西。

<View> 的裡面除了可以放其他元件以外，當然也可以放 <View>，這樣就可以做出一個容器裡面包容器的效果。

跟 HTML 不同的地方還有必須要引入元件，例如 <View> 要引入 `import { View } from 'react-native'`。

## style

React Native 的 style 跟 HTML 的 style 不太一樣，因為 React Native 並沒有直接支援 HTML，所以要用 React Native 的 style。

### style 的寫法

```js
<View style={{ backgroundColor: 'red' }} />
```

或是使用 StyleSheet

```js
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    backgroundColor: 'red',
  },
});

<View style={styles.container} />
```

## Layout

在 React Native 中，<View> 會預設使用 Flexbox 來做 Layout。
並且預設的 flex-direction 是 column，所以要改成 row 的話，要加上 `flexDirection: 'row'`。