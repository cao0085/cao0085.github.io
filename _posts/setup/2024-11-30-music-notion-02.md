---
title: "Music Notion（二）：設計模式"
date: 2024-11-30
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 在開發專案前只有 Vue3 使用經驗和看過 Clean Code & 知道設計模式的存在，所以就是把每個 TS 當作一個 Component 使用，並盡量維持單一職責去寫程式，並且注意命名原則，剩下的就是且戰且走。

<!-- 正文 -->

寫完專案後正在看 O'Reilly 出版的 JavaScript 設計模式，對於設計模式還在混亂的階段，下面紀錄PlayerBar算有使用到的概念。

### Strategy Pattern
要讓不同 Player 最終由同一組 UI 操控，GPT 推薦給我策略模式來設計。<br><br>
一開始的預想將不同播放器的功能抽象成統一的接口，讓主控邏輯（Main Strateger）可以調用具體實現。實做上以下幾個問題逐漸浮現：

1. 異步操作與實例化邏輯的分散：Spotify 有大量異步操作，所以這些操作要在 Main Strateger 實例化後才能完成某些組合邏輯。結果就是許多功能在 Spotify Strateger 內部無法即時完成，過度依賴於 Main Strateger 的上下文，導致具體行為難以和 Web Audio 統一。

2. UI 操控與狀態同步：要如何用 UI 操控 Main Strteager 並把實際狀態反 UI。例如曲目播放完畢 button 沒有切換成暫停icon，播放進度條&音量需要雙向綁定。

### Observer 、pub-sub pattern
在硬幹很久後，找到了這個設計概念，整個資訊傳遞變成
1.	UI主動&API自動觸發：使用者在 UI 上的操作（例如點擊按鈕或調整進度條）會觸發對應的功能，這些功能由 Main Strategist 負責執行。
2.	功能作用於 Proxy 狀態：主功能執行後，會通過改變 Proxy 的狀態來完成邏輯操作，例如播放進度更新、音量調整等。
3.	Proxy 狀態驅動 UI 更新：當 Proxy 的狀態改變時，會自動通知訂閱的 UI 元件進行同步更新，確保介面的顯示與內部邏輯保持一致。


### 自定義 API 設計與優化
在引入 Proxy Status 後，為了確保 Main Strategist 能夠在操作不同策略時對 Proxy 的行為邏輯保持一致，針對 Web Audio 與 Spotify API 的差異進行了不同封裝：

#### 自動播放進度監控
Web Audio 提供了 ontimeupdate 事件，可以自動監聽當前 track 的播放進度，而 Spotify 僅提供 getCurrentTime方法，且需要手動調用。
- 解決方案
    - Spotify Strategy play() 方法內嵌入一個定時器，通過定期調用 getCurrentTime() 來更新 Proxy 狀態。
    - pause() 中暫停定時器，停止不必要的性能耗損。

#### 獲取最新的 Track 資訊
Web Audio 可以在 track 更換時立即獲取新資訊。Spotify 的 getInformation() 則需要等待 player 返回資訊，可能出現延遲，導致返回舊資料，且整合返回的資料。
- 解決方案
    - 找各個策略明確的更新階段斷點，在不同地方放置 getInformation()
    - 也幫 track name 、 duration 靜態內容放上 proxy status

#### 統一時間規格
Web Audio 使用秒（s），而 Spotify 使用秒（ms），常因爲粗心導致奇怪的bug。
- 解決方案
	- 在所有 PlayerStrategy 的方法中，統一使用毫秒（ms）作為return。
	- 需要與 UI 互動的時候再寫 format 轉換（ 歌曲總長百分比用 ms 、顯示時間用 s）

### UI 選擇與彈性
功能設計遷入好後就是處理 UI ，比較難的地方就是讓開源 UI 輕易與 Proxy 的狀態雙向綁定，做很多測試後選擇 noUISlider，先去了解 noUISlider 提供什麼功能，還有動畫部分要怎麼保有流暢感。



