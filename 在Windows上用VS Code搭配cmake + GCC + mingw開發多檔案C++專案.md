# 在Windows上用VS Code搭配cmake + GCC + mingw開發多檔案C++專案

ok好來記錄一下標題打得這麼一長串的東東要怎麼讓它執行起來，相信會google或找到或存到這篇的你應該是會有這個機掰的需求，畢竟這東西當初搞了我整整兩個禮拜，不同電腦狀況又都不一樣QAQ

但撰寫這篇文章時，有重開一個windows sandbox的環境進行全新安裝測試，所以應該會是90%成功的方式了吧！

我的作法有點倒果為因，可能不是正統做法，但我也只是要讓他可以跑起來而已QAQ

專案的CMakeLists.txt為下圖結構
![](https://i.imgur.com/hLa6CCQ.png)

## 事前準備：
[VS code **System Installer** (安裝過程會提醒你要裝對)](https://code.visualstudio.com/#alt-downloads)
[正確版本的GCC+mingw](https://sourceforge.net/projects/winlibs-mingw/files/)
(codeblocks用的是GCC8.1.0，筆者自己用9.2.0，學長用10.3.0，撰稿當下最新版為12.2.0)
(其他參數的判斷請洽下文)
[mingw-get-setup](https://osdn.net/projects/mingw/downloads/68260/mingw-get-setup.exe/)
[CMake GUI](https://cmake.org/download/)非必要，因為功能有包在VS extension，但還是可以裝！

:::success
筆者自己有留一份
VS code 1.71.2 + GCC 9.2.0 + mingw
的安裝檔備份，假使哪天這些連結有失效的話，文章最底有相關聯絡資訊
:::

## 安裝步驟 抓穩囉！
### 1. 先解壓縮指定的GCC+mingw版本到你想要的地址(本文指定在C:\mingw64)
線上安裝檔(mingw-w64-install)理論上不能用(所以我的事前準備也沒有他)，而且停在舊版，所以要自己手動下載需要用到的7z
![](https://i.imgur.com/KwEdH9B.png)
這裡要選你要的版本，而筆者的版本在最底部XD
![](https://i.imgur.com/Z9uwVKi.png)
因為9.2.0太乾淨惹(?)所以截了很長的代碼，並提供如下解釋

:::success
i686代表為32bit only(無法compile出64bit exe)，**x86_64**為32+64bit都有
**posix** vs win32，大致上為多執行續的編譯方式，win32不支援多執行續，所以基本上會裝posix
**seh** vs sjlj vs dwarf
在處理exceptions時會用不同的方法return，有點複雜我不是很清楚架構，不過最基礎的是seh，所以我也選用這個版本
:::

### 2. 將剛剛那個地址及內部的bin加入到PATH
![](https://i.imgur.com/CDOStTJ.png)
:::success
打開PATH的方法，左下角開始按右鍵>系統>找到最右邊的進階系統設定>環境變數>path按編輯>key入地址
:::

### 3. 安裝VS CODE，並安裝以下3個extensions，安裝完成後關閉VS code
C/C++, cmake, cmake tools
(此步驟截圖略，應該是也不需要啦XD)

### 4. 安裝mingw-get-setup，請以系統管理員身分執行，並指定安裝地址在剛剛的GCC+mingw的解壓縮地址
![](https://i.imgur.com/tEiDuBQ.png)
請再次確認地址有跟GCC+mingw的解壓縮地址相同
### 5. 打開MinGW Installation Manager，在all packages裡面找到mingw32-make-bin，安裝他！
![](https://i.imgur.com/QGo2UsV.png)
左邊點選All Packages，右邊開始拉找到mingw32-make-bin
![](https://i.imgur.com/eLag2fv.png)
在回到左上角的installation，選apply changes
### 6. 回到VS code，按下ctrl+shift+P 選擇 C/C++:Edit Configurations(UI)
![](https://i.imgur.com/PPDKQbf.png)

![](https://i.imgur.com/gKrlbTW.png)
這裡請確認有選定到正確的compiler path
### 7. 打開專案檔，點左下角Cmake tools的kit選擇compiler位置，理應自己抓到惹，沒有的話要把第5步解除安裝make-bin後重新安裝！
![](https://i.imgur.com/pe06CAW.png)

### 8. 按run，大功告成，有夠感動痛哭流涕QAQ
![](https://i.imgur.com/VKRaE72.png)
開心灑花，可以到debug console去確認程式的執行狀況囉！

::: danger
如果有人知道怎麼設定這種情況下的breakpoint，霸脫教我QAQ
:::

:::warning
CopyRight belongs to CrazyRyan庭競，未經許可嚴禁商業轉發，學術用途傳承不在此限
[CrazyRyan庭競 的 Github](https://github.com/CrazyRyan0812/)
[CrazyRyan庭競 的 攝影集](https://www.crazyryan.xyz/)
:::