---
title: 如何刪除 Hexo 已發佈的文章
tags:
  - hexo
  - delete post
  - 刪除文章
date: 2020-09-23 14:10:55
categories:
keywords:
  - hexo
  - delete post
  - 刪除文章
decription: 如何刪除 Hexo 已發佈的文章
---
不小心在 Hexo 發佈一篇不要的文章，本來以為直接刪除 posts 裡面的文章再重新發佈一次就可以了，結果沒用，在 stack overflow 找了一下就解開了，紀錄一下該怎麼解決。
<!--more-->
不囉嗦，直接上作法：

1. 直接在 `source/_post` 資料夾中刪除文章。
2. 在終端機執行 `hexo clean`，並在根目錄下刪除 `db.json`。
3. 執行 `hexo generate` 或是 `hexo g`。
4. 執行 `hexo hexo deploy` 或是 `hexo d` 重新發佈。

其實還蠻簡單的，只是官方文件找不到這個方法蠻意外的。

參考文件來源：[How do I delete a post in hexo
](https://stackoverflow.com/questions/27894210/how-do-i-delete-a-post-in-hexo)