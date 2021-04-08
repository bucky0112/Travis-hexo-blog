---
title: City Weather App 開發筆記
tags:
  - javascript
  - scss
  - City Weather App
  - 開發筆記
  - openweather
  - api
date: 2020-09-26 16:41:51
categories: 開發筆記
keywords:
  - javascript
  - scss
  - City Weather App
  - 開發筆記
  - openweather
  - api
decription:
---
這次單純使用 JavaScript 來串接 API 開發一個天氣小程式，也練習一下我的 SCSS，寫了一下英文文件發在 GitHub 上可以練習我的英文，這篇就稍微介紹一下我的開發過程吧～
<!--more-->

## 天氣資料
首先要找天氣資料來源，發現 [Open Weather](https://openweathermap.org/) 有提供 API，不過要先去註冊申請拿到 API key 才能開始，註冊方面我就不多加解釋了。
它有很多 API 可以玩，不過目前只需要拿到當前天氣狀況的資料，就選擇 **Current Weather Data**。
![FF12C12D-BD8D-45E7-8EF6-26989F41A14B](https://i.imgur.com/57dcG0v.png)

這邊就直接跳到拿到 key 之後可以透過 API call 去拿資料：

* 預設API call：`https://api.openweathermap.org/data/2.5/weather?q={city name}&appid={API key}`

API call 的網址也可以試著加入想要的資料，例如：
測量單位可以選：預設是 standard，選擇 `metric` ，溫度為攝氏，所以加上 `&units=metric`
* 語系也可以選擇：`http://api.openweathermap.org/data/2.5/weather?id=524901&lang=zh_tw&appid={API key}`，但是只有顯示在 description

就會得到以下資料是 json 格式：
```json
{
    "coord": {
        "lon": 121.53,
        "lat": 25.05
    },
    "weather": [
        {
            "id": 803,
            "main": "Clouds",
            "description": "broken clouds",
            "icon": "04d"
        }
    ],
    "base": "stations",
    "main": {
        "temp": 299.54,  // 溫度。單位默認值：凱式
        "feels_like": 302.86,
        "temp_min": 298.71,
        "temp_max": 300.15,
        "pressure": 1011,
        "humidity": 74
    },
    "visibility": 10000,
    "wind": {
        "speed": 1.5,
        "deg": 130
    },
    "clouds": {
        "all": 75
    },
    "dt": 1601011898,
    "sys": {
        "type": 1,
        "id": 7949,
        "country": "TW",
        "sunrise": 1600983814,
        "sunset": 1601027252
    },
    "timezone": 28800,
    "id": 1668341,
    "name": "Taipei",
    "cod": 200
}
```

## 介紹功能
拿到資料以後就可以來完成作品了，以下是我的簡單功能介紹：
![完成作品圖](https://camo.githubusercontent.com/86d34800873cb62899f0a7d9a030ba7f6c8ea814/68747470733a2f2f692e696d6775722e636f6d2f364436646243692e706e67)

* 進入畫面直接顯示目前時間、溫度、天氣狀況，及最低到最高溫度。
* 預設地點是台中。
* 可搜尋城市，可透過輸入框鍵入地點（API 只提供英文），按 Enter 鍵或是按旁邊的按鈕都可以。
* 如果輸入錯誤名稱就會跳提示，提醒你輸入錯了。

## 後記
目前大概是這些功能的小作品，未來如果想到什麼新的再加進去，或是發現哪邊需要改的 bug 再做修正。