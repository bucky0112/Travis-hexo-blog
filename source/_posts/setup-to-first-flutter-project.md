---
title: 從手把手環境設定到建立第一個 Flutter 專案
date: 2024-09-14 00:08:54
tags:
 - flutter
categories: Flutter
keywords:
 - flutter
decription: 帶你手把手把 Flutter 的環境設定弄好！
---
環境設定永遠是開發者一開始遇到的新人殺手，尤其是當你下定決定想學一項技術，但在環境設定就卡關，然後...就沒有然後了😭。Flutter 的環境設定算有點麻煩，但希望本篇文章可以幫助你打贏第一個關卡。然後由於我手上只有 macOS，所以這篇文章會以 macOS 的用戶視角來進行環境安裝。

## 選擇環境

進到[官網](https://docs.flutter.dev/get-started/install)之後，可以選擇你的開發系統，並按照官方文件來一步一步完成環境設定的前置作業，這裡選擇 macOS 來示範。
![20240914001628](https://i.imgur.com/2CWMTTi.png)

接下來會問你主要想開發什麼類型的 App，由於 Flutter 是一個可以跨平台開發的框架，不管是 iOS 還是 Android，甚至是 Desktop App 還是 Web 都能勝任。不過我們的目標主要是要開發 mobile app，所以這裡我們選擇 iOS 或是 Android 都可以，這裡我們先選擇 iOS。

![20240914001714](https://i.imgur.com/ot17JUJ.png)

## 安裝必要的開發工具

進到 iOS 的設定頁面，會先看到密密麻麻的一堆文字，但不用擔心，接下來一步一步帶你設定。

1. 首先想要開發 iOS app，那麼就必須要安裝必要的 Xcode（這是一個在 App Store 上評分超低分，而且又超級肥的工具，但只能用它），後面會需要做一些設定。
![20240914001743](https://i.imgur.com/fZ9YXQD.png)

理論上安裝完這個東西你還可以順便得到 Git，但我想一個合格的工程師電腦上應該都有 Git 吧？

2. 接下來如果你的電腦是 Apple silicon，舉凡 M1、M2 等等的晶片，那麼你要在終端機執行以下的指令：

```bash!
sudo softwareupdate --install-rosetta --agree-to-license
```

## 安裝 Flutter SDK

進到 iOS 的設定頁面，直接畫面往下滑到 `Install the Flutter SDK` 的部分，這裡有兩個選擇：

- Use VS Code to install
- Download and install

那麼這兩個有什麼差別呢？主要是看你之後用什麼方式開發，並且用什麼來模擬。如果是選擇 VSCode，未來都可以在 VSCode 上面就好；反之如果你想要另外開終端機來輸入指令的話，那就可以選擇 Download and install。

1. 下載 SDK

我是建議直接下載 SDK，然後再視情況安裝 VSCode Extension。

![20240914001852](https://i.imgur.com/fGeZEvj.png)

如果是 Intel 晶片的就選 inter Processor；反之如果是 Apple Silicon 的就選另一個。

下載 zip 檔案之後解壓縮，會得到一個 `flutter` 的資料夾，裡面就是 Flutter 的 SDK，然後在你的根目錄建立一個 `development` 的資料夾，接著把整包 `flutter` 資料夾丟進去。所以你的架構應該會是：

```
~/development/flutter/
```

2. 加入 Flutter 到 PATH

移動到你的根目錄，打開你的終端機，並輸入：

```bash!
cd ~/
```

檢查看有沒有 `.zshenv` 這個檔案：

```bash!
ls -a
```

沒有的話就自己新增一個。接下來用你習慣的編輯器打開它，我是蠻推薦直接用 vim 來直接新增或者是編輯：

```bash!
vim .zshenv
```

然後把這一段直接複製貼上：

```
export PATH=$HOME/development/flutter/bin:$PATH
```

路徑就是剛剛在根目錄新增的 `/development/flutter/`。

存檔之後最好重開終端機，然後在終端機輸入：

```bash!
which flutter
```

應該可以看到類似的東西：

![20240914001920](https://i.imgur.com/sGy5bYM.png)

如果沒看到或是顯示錯誤，也不用擔心，代表可能是路徑錯誤，再仔細看一下有沒有打錯字。

最後在終端機輸入：

```bash!
flutter doctor
```

![20240914001940](https://i.imgur.com/ogMyCcD.png)

那麼你的 Flutter SDK 設定大致上就完成了👏。

## 設定 Xcode

前面提到想要開發 iOS 必定要有 Xcode，接下來打開你的 App Store，搜尋 Xcode。

安裝完 Xcode 之後，在終端機輸入指令：

```bash!
sudo sh -c 'xcode-select -s /Applications/Xcode.app/Contents/Developer && xcodebuild -runFirstLaunch'
```

再次輸入指令：

```bash!
sudo xcodebuild -license
```

這是一段協議書，需要你看完並同意，沒問題的話就直接輸入 `agree`。

## 模擬工具設定

在開發的過程中，我們需要畫面來查看是否正確，或者是操作上有沒有問題。

Xcode 提供了 Simulator 這個工具，可以讓你在 Mac 上直接模擬一台 iPhone 甚至是 iPad。

1. 安裝 iOS Simulator，在終端機輸入指令：

```bash!
xcodebuild -downloadPlatform iOS
```

2. 打開 Simulator：

```bash!
open -a Simulator
```

就可以看到一台 iPhone 的畫面呈現在你的電腦上。

![20240914002011](https://i.imgur.com/K02nq2q.png)

恭喜你，你的 Flutter 現在就可以開發 iOS app 了。

經歷完環境安裝的艱難過程後，終於可以開始來建立我們第一個 Flutter 專案了，這篇文章將會介紹幾個方式來建立專案。

## 1. 使用 Commend Line 來建立

首先打開終端機，然後在你想要建立專案的位置，例如 desktop，輸入：

```bash!
mkdir first_flutter && cd "$_"
```

這段指令會直接新增一個叫做 `first_flutter` 的資料夾，並移動進該 `first_flutter` 中。

然後接下來介紹一下 Flutter 的指令有哪些。

如果你一開始什麼都不知道的話，那麼可以輸入：

```bash!
flutter -h
```

這段指令就是 `flutter --help` 的簡易版，想成是要進入 Flutter 的遊客中心，裡面會列出基本的資訊，這是一段非常實用的指令，可以記一下。

如果想確認一下現在安裝在你電腦中的 Flutter 版本是哪一版的話，就輸入：

```bash!
flutter --version
```

例如可以看到我現在安裝的 Flutter 版本是 3.22.3，Dart 的版本是 3.4.4。

![20240914002418](https://i.imgur.com/VHAgDon.png)

然後如果你輸入：

```bash!
flutter -v
```

這一段不是 `flutter --version` 的簡易版喔，而是代表 Flutter “詳細”的選項。

如果你輸入 `flutter -v` 的話，應該可以發現跟 `flutter -h` 有點像，但是解釋的更詳細。在目前的版本中，`flutter -v` 就等同於 `flutter --help -v`。

所以如果你想得到某些指令更詳細的資訊的話，就在最後面加上 `-v`，例如：

```bash!
flutter --version -v
```

就可以顯示更詳細的 Flutter 版本資訊了。

![20240914002444](https://i.imgur.com/rHC7DBt.png)

稍微介紹了一下 Flutter 的指令之後，你可能有看到並知道怎麼輸入建立專案的指令了，沒錯，輸入：

```bash!
flutter create hello_world
```

後面的 `hello_world` 你可以帶入任何的名稱，不過最好是使用 Snake case，因為這個是慣例。

沒問題的話，就可以看到如同下面圖片的資訊：

![20240914002505](https://i.imgur.com/RODR486.png)

會跟你說接下來你應該移動到新建的專案資料夾中，接著執行 `flutter run`。

但我們先不要急，先移動到該新建的專案資料夾中，然後使用你慣用的編輯器打開資料夾，這裡會使用 VSCode 來示範。

使用 VSCode 開啟專案之後，接下來在 Extensions 搜尋 flutter，認明這個 Flutter 官方的 Extension。

![20240914002531](https://i.imgur.com/B1V2Qig.png)

或者是直接點[這裡](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter)下載。

接下來要來介紹第二種建立 Flutter 專案的方法。

## 使用 VSCode 建立專案

這個方法適合不喜歡開太多東西，只想開一個 VSCode 就搞定的人。

那麼一樣先建立一個空的資料夾，然後進去該新建的資料夾，例如：

```bash!
mkdir first_flutter && cd "$_"
```

然後用 VSCode 打開這個新增的資料夾，接下來鍵盤指令 combo，按下 `command` + `shift` + `p`，如果是 Windows 的用戶應該會是 `ctrl` + `shift` + `p`。

會在你的 VSCode 跳出一個 Command Palette。

![20240914002558](https://i.imgur.com/4UT8v5c.png)

然後輸入 flutter，然後選擇 `Flutter: New Project`

![20240914002615](https://i.imgur.com/5fj4fLE.png)

接下來會跳出：

![20240914002631](https://i.imgur.com/jmygdFd.png)

這裡有幾個 Template 可以選擇，沒特別要求的話，就選 Application 就好，這個就等同於指令 flutter create 建立新專案。

建立完你的 VSCode 就會跳出一個提示：

![20240914002711](https://i.imgur.com/n7AckkG.png)

跟你說 Flutter 專案已經建立完成，可以直接按 F5 就可以跑囉！

## 執行你的第一個 Flutter App

接下來介紹一下要怎麼執行 Flutter App。

## 1. 在 VSCode 上執行

第一種方法先介紹如何在 VSCode 上執行模擬器。

先使用 VSCode 打開你的專案，應該會看到如同下圖的畫面：

![20240914003051](https://i.imgur.com/XLTgKRL.png)

然後用 VSCode 打開這個新增的資料夾，接下來鍵盤指令 combo，按下 `command` + `shift` + `p`，如果是 Windows 的用戶應該會是 `ctrl` + `shift` + `p`。

會在你的 VSCode 跳出一個 Command Palette。

![20240914003122](https://i.imgur.com/hupC5W3.png)

然後輸入 flutter，然後選擇 `Launch Emulator`

![20240914003155](https://i.imgur.com/VhsA0dR.png)

接下來應該會跳出一些模擬器的選擇：

![20240914003215](https://i.imgur.com/XbEuNov.png)

例如有 iOS 的跟 android 的，這裡先示範 iOS 的。

當你選完 iOS 之後，就會跳出一個 Simulator，可以看到一個 iPhone 的畫面在你的電腦上，不過還沒有真正執行你的專案中的程式碼。

現在請你的 VSCode 上面，並按照兩個步驟點擊。可以參考下圖中的範例：

![20240914003246](https://i.imgur.com/6ku3khZ.png)

1. 找到 `lib` 資料夾中的 `main.dart` 檔案。
2. 在 `main()` 的上方，可以看到有一排文字：`Run | Debug | Profile`。

如果沒看到這排文字，可能代表你沒安裝 [Flutter](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter) 的官方 Extension。可以試著再安裝一次，或者重新開啟 VSCode。

然後點擊 `Run`，就可以看到 VSCode 右下角正在執行中：

![20240914003316](https://i.imgur.com/MkG5xk0.png)

成功運作的話，可以看到 VSCode 多了一個控制列。

![20240914003333](https://i.imgur.com/3EJKDiQ.png)

然後你的 iOS Simulator 畫面應該會是這樣：

![20240914003345](https://i.imgur.com/TRIRn0N.png)

接下來我們試著改變一下程式碼。

在 `main.dart` 這個檔案，往下滑找到：

```dart!
'You have pushed the button this many times:'
```

這一段就是在畫面上的顯示的文字，我們隨便改一下之後，畫面會隨著你修改之後馬上 Hot Reload 更新。

![20240914003412](https://i.imgur.com/Aq2TTRk.png)

有時候可能會遇到沒有更新的情況，沒關係。直接在 VSCode 的那個控制列，點擊 ⚡️ 的按鈕，這個是 Save and Hot Reload 的功能，這樣你的畫面應該就會按照你修改的更新了。

## 在終端機上執行

前面示範了如何在 VSCode 執行 Flutter 的專案，並且如何即時更新，接下來來示範如何在終端機做這些動作。

首先打開你的終端機，如果你直接執行：

```bash!
flutter run
```

應該會顯示以下的畫面：

![20240914003436](https://i.imgur.com/h0gsxjU.png)

這是代表我們並沒有連結任何的裝置，或是模擬器。

如果我們想確認一下現在 Flutter 有沒有連結任何裝置或模擬器，可以執行：

```bash!
flutter devices
```

應該會看到類似以下的訊息：

![20240914003457](https://i.imgur.com/MmVQRQW.png)

如果有連結的話，會顯示在上面，但如果沒有，他提示你說，可以執行：

```bash!
flutter emulators
```

執行完會跟你說，現在有哪些模擬器可以使用。

![20240914003516](https://i.imgur.com/Vio1JAi.png)

並且提示你目前你如果要執行模擬器，可以執行：

```bash!
flutter emulators --launch <emulator id>
```

以我目前的狀況，我想打開 iOS Simulator 就是輸入：

```bash!
flutter emulators --launch apple_ios_simulator
```

如果這裡都沒出現任何裝置的話，那麼他也提示你，可以輸入指令自己新增：

```bash!
flutter emulators --create [--name xyz]
```

通常只要模擬器已經開啟，那麼就可以直接執行 Flutter 專案了：

```bash!
flutter run
```

你的終端機會出現這樣的畫面：

![20240914003539](https://i.imgur.com/BcavgsG.png)

然後如果我們修改一下程式碼，可以發現可能沒辦法即時更新畫面。

沒關係，終端機有提示我們可以執行 `r` 來做 Hot reload，也可以做到更新程式碼的動作。

以上就是如何透過 VSCode 和終端機來執行 Flutter 專案的流程，各位可以選擇喜歡並且順手的方式。