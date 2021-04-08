---
title: 圖片剪裁套件的使用 - Vue-Cropper
tags:
  - vue
  - 開發筆記
  - 圖片剪裁
date: 2021-01-24 17:23:27
categories: 開發筆記
keywords:
  - vue
  - 開發筆記
decription: 圖片剪裁套件的使用 - Vue-Cropper
---
Vue Cropper 這個套件使用起來非常方便，而且剛好符合我的需求，只需要看著文件設置一些設定就可以做出變化了。
<!--more-->

## 前言

當主管告知需要在後台將前台的圖片做剪裁修改後，並且能夠即時預覽結果，最後再將圖片存成新檔案。找了一下有沒有前人造的輪子，發現還真的有，所以設置了一下後就可以達到我要的效果了。

## Vue Cropper

1. 安裝

   ```js
   npm install vue-cropper
   
   yarn add vue-cropper
   ```

2. 在 component 裡使用

   ```vue
   import { VueCropper } from "vue-cropper";
   
   components: {
   	VueCropper,
   },
   ```

3. data 跟 methods 的設定：

```vue
data() {
    return {
      modelSrc: "",
      option: {
        img: "https://fakeimg.pl/450x300/", // 圖片來源
        outputType: "png", // 產生圖片的格式
        autoCrop: true, // 是否要有截圖框
        autoCropWidth: 350, // 截圖框寬
        autoCropHeight: 220, //截圖框高
      },
    };
  },
  methods: {
  // *********預覽目前截圖結果************
    preview() {
      this.$refs.cropper.getCropData((data) => {
        // console.log(data);
        this.modelSrc = data;
      });
    },
  // *********即時預覽函數************
    realTime(data) {
      this.previews = data;
      // console.log(data);
    },
	// *********得到截圖的 base64 數據************
    demo() {
      this.$refs.cropper.getCropData((data) => {
        console.log(data);
      });
    },
  },
```

4. 最後在 component 的綁定

   ```vue
   <vueCropper
   	ref="cropper"
     :img="option.img"
     :outputType="option.outputType"
     :autoCrop="option.autoCrop"
     :autoCropWidth="option.autoCropWidth"
     :autoCropHeight="option.autoCropHeight"
     @realTime="realTime"
    >
    </vueCropper>
   ```

這樣就會產生一個可以自由切割並且截圖的區塊，可以試著玩看看我建立的 CodeSandBox：

<iframe src="https://codesandbox.io/embed/blue-monad-6xlxu?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="blue-monad-6xlxu"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

最後要記得給這個 component 一個高度，不然會顯示不出來。

**資料來源：[vue-cropper](https://github.com/xyxiao001/vue-cropper)**  

