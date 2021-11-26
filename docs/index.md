# Flutter Plug-In 開發

今年（2021）我在工作中大量使用 Flutter，除了使用 Flutter 開發交付給客戶的產品，也使用 Flutter 開發公司內部的測試工具，平台則包括 iOS、Android 以及 Windows。

當中除了 App 開發外，也免不了要開發一些 Plug-in—目前在 iOS、Android 這兩個平台上，Flutter 已經有很豐富的生態系，很多你可以想像到的 Plug-in，在官方的套件網站 [pub.dev](https://pub.dev/) 上都可以找到，但是在網頁與桌面平台上，包括 Windows、macOS、Linux 則還是一大塊處女地，猜想是因為畢竟今年 Flutter 才正式支援這些桌面平台，開發者的投入原本就比較晚一些，另一方面隨著 2007 年 iPhone 問世到現在，智慧型手機大大普及，手機 App 的開發者應該也遠勝桌面 App 的開發者，而 Flutter 官方的文件也幾乎著重在 App 開發，像是 Widget 介紹、App 架構、狀態管理手法等，對於怎麼開始開發 Plug-In，也缺少文件介紹。

## 什麼時候需要寫 Flutter Plug-In？

在這樣的狀況下，我也就迷迷糊糊跌跌撞撞寫了點東西出來。

講一下我最近（2021 年 11 月）寫的幾個可以公開的東西：[flutter_platform_alert](https://github.com/zonble/flutter_platform_alert)，這個 Plug-in 可以讓你的 App 發出錯誤提示聲，也可以使用原生的 Alert Dialog 跳出提示；寫這個東西的動機是，一般用 Flutter 寫出的 Desktop App，在遇到錯誤的時候，是不會發出錯誤音效的，在手機上沒有錯誤音效應該大家可以習慣，但是在桌面平太上，這個體驗就很奇怪，與整個桌面環境的體驗之間有些格格不入，所以就接了系統提示聲，就順便把各種平台的 Alert Dialog 也接了一下。

另外寫了一個 [flutter_window_close](https://github.com/zonble/flutter_window_close)，功能是可以讓用戶在桌面環境下的 Flutter App 中按下視窗的關閉按鈕時，確認用戶是否真的想要關閉或是誤按。在手機平台上，用戶是將 App 從 App 列表中「滑掉」關閉，關閉時也不會有特別提示，但是在桌面平台上，這卻是非常常見的需求與功能。

寫這兩個 Plug-in，目的就在於，我們需要擴充 Flutter 開發框架原本所沒有的能力。大概可以分成兩個方向，其一是呼叫原本不在 Flutter 當中的原生 API，像我們要讓 App 發出錯誤提示聲，在 Windows 上要呼叫 [MessageBeep](https://docs.microsoft.com/zh-tw/windows/win32/api/winuser/nf-winuser-messagebeep)，macOS 上要呼叫 [NSSound](https://developer.apple.com/documentation/appkit/nssound/2903487-beep)，Linux 上則是 [gtk_widget_error_bell](https://docs.gtk.org/gtk3/method.Widget.error_bell.html)。另一個則是要讓 Flutter 接受原本沒有收到的通知，像是在 Windows 上想要收到視窗關閉的通知，就需要額外收聽 [WM_CLOSE](https://docs.microsoft.com/zh-tw/windows/win32/winmsg/wm-close) 通知。

在 Flutter 中，Plug-in 與 Package 的差別，就在於 Plug-in 中多了與平台上原生程式碼溝通的部份，像是在 iOS 與 macOS 上使用 Objective-C 或 Swift 語言、Android 上使用 Java、Kotln 或 JNI，以及在 Windows 及 Linux 上使用 C 或 C++ 撰寫的程式等（其實這樣並不精確，畢竟也可以用 Go 或是 Kotlin Native 等語言）。至於全部只使用 Dart 程式語言，不依賴各種原生的 API 的話，撰寫 Package 即可。

## Flutter 與 Plug-In 之間的通訊方式

## 前置作業
