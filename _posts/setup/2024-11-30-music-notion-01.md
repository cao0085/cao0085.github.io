---
title: "Music Notion（一）：設計架構＆困難"
date: 2024-11-30
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 熟悉框架前先寫基礎的JS。決定從 TS 寫一個專案，體驗看看之前多幸福。

<!-- 正文 -->
### [Side Project：Music Notion](https://www.chord-snap.com/)

### 設計架構功能
初起是想做簡易的打譜應用，隨著邊做想法越多。從自己專用改架成實用的網頁給大家使用，專案開發(包含重構)到真正上線花了一個多月。

1. 能夠儲存成 json 專案檔
2. 同步支援Ipad Air 、Ipad Pro。
3. 播放器整合成有 Spotify + Web Audio 兩種切換。
4. 擷取譜面內容轉成 PDF 

從個人使用到開放網站，要讓網站的操作性提升，技術上困難滿多的。

- 版面配置
    - Header(導覽列)
    - Content切成兩塊(Song Imformation + Sheet)
    - Footer (Player Bar)

環境配置使用 docker container node.js 18+ TS + Webpack，以下記錄一些遇到的難題。



### 同步更新使用者端的顯示資料&實際資料

DOM Element 初始建構 value 並不會與使用者操作時輸入同步，這個是我在設計下載功能才發現的，這個原廠設定讓我原以為只要畫畫格子排列好，變成有點麻煩。<br>

#### 優化歷程：
``` .md
1. 幫每個 Element 添加監聽事件，每次輸入都更新。
  - 這個方法背後有的問題就是會有不必要的性能耗損。
```
``` .md
2. 在需要更新前，一次刷新全部的值
  - 設計成在下載專案檔前，用父元件 queryselector 一次選取 element 刷新。
  - 遇到的新問題會是有些 function 是放在別的區域的，無法跨區域選擇。
```
``` .md
3. 導入Singleton概念
  - 建立一個全域物件(單例)，用來儲存資料，確保儲存資料的物件只有一個。
  - 幫每個需要被儲存的element添加"data-store"屬性。
```
``` .md
4. 上傳儲存的json檔案並回填
  - 因為是把資料儲存在Array，單純的上傳只能覆蓋掉原先的Array。
  - 再把元件添加 ID，讓回填資料可以找到正確的地方。
```
``` .md
5. 修改元件屬性
  - 功能設計完後調整CSS才發現 Input element可調性很低。
  - 改成editable element並添加textFormat用正規表達式轉換字符
```
``` .md
6. 添加翻頁＆PDF下載
  - 翻頁功能滿偷懶的，把架構定死(3頁)，作法是複製Content內其中一塊element，翻頁啟動時替換掉
  - PDF難的地方是我只要抓取 Content element 的內容，而且要是渲染過後的
  - 就設計成 -> 自動跳轉第一頁 -> 抓取 Content element 轉成 PDF -> 翻頁 ...
```

### 播放器整合

重做滿多次的，了解到定規格的重要性。最困難的地方是要同時處理邏輯不一樣的API&降低流程間的耦合性。

- Playbar Button 設計
    - 切換播放模式
    - 上傳選擇檔案
    - 播放進度條
    - 時間標記
    - 歌名標記
    - 音量控制條

``` .md
1. 如何讓使用者&後台控制播放進度和音量條...，並都及時反應在UI顯示正確
  - 實做模擬 React & Vue 的"雙向綁定"
  - 導入觀察/訂閱模式，統一管理監控狀態切換
```
``` .md
2. 整合兩個 PlayerAPI 在 UI Button 共用
  - 導入策略模式
  - 先把 Spotify API & Spotify Playback sdk 整合成一個自製 Spotify API。
  - 把 Web Audio / Spotify API 官方功能不同的地方列出，各自添加Function彌補上，統一回傳資料。
```
``` .md
3. 平台裝置限制
  - IOS Safari 規定滿多的，要額外埋一些無感的function在裡面
  - Spotify Oauth2.0 改成彈出式視窗符合限制
```


### 何時開始做版面配置

這部分也因為中途要支援 Ipad 重做了幾次，技術上還好但做的時機點滿重要。

```
1. 開源UI Component重復使用
  - 常常遇到元件掛掉問題，後來導入webpack後在debug效率才有效提高。
  - 釐清每個 Component 已有什麼功能、哪部分可以和 function 結合，哪部分要自己實做
```

```
2. RWD設計
  - 基本的網格設計
  - 沒有架設後端所以不同裝置的 800*1000 顯示內容必須是一樣的。
  - 我應用的是譜面設計應用，不是單純的"裝置能夠一個螢幕看到所有內容"，還需要保持一定比例。
  - 適當的妥協和調整，為了排版設計減少單頁內容回去添加翻頁功能。
```

我沒有引入華麗設計，以簡潔好使用優先，但是花的時間比預期的多很多，要怎讓開源UI元件聽話是個學問，中間有自己去試做一個slider component從UI開發者角度出發，理解一下設計邏輯。



### 部署網站＆優化搜尋
流程是走 Cloudflare 買網域 + Github Pages 部署，意外的流暢一天晚上就搞定了。比較不確定的地方就是要改 Spotify OAthur 的 callback router。接著就是基本的SEO和網站監控，用Google Sitemap + GA 埋一些東西，以後有空再看要怎麼內容行銷。