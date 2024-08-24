---
title: "How to Install Jekyll with ChatGPT"
date: 2024-08-07
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### Jekyll 是一款用於靜態網站生成的開源工具，使用者只需撰寫 Markdown 或 HTML 格式的文件即可創建網站。Jekyll 生成的網站運行快速且易於部署，特別適合在 GitHub Pages 等靜態託管平台上使用。

<!-- 正文 -->
### 預先準備
**Ruby**：Jekyll 是使用 Ruby 編寫的網站生成器。可使用版本管理工具如 rbenv 來管理 Ruby。  
**Bundler**：Bundler 是 Ruby 的一個套件管理工具，用於安裝和管理 Jekyll 及其依賴的gems。 
  
    

### 前言

首次安裝時順順下指令給ChatGPT就成功，後來刪除到Jeykll生成某些檔案無法正常開啟。重新安裝就遇到很多障礙，弄了三個小時後冷靜下來才成功。在操作安裝前想先模擬一個情境，分享目前對於使用ChatGPT的理解，看能不能讓和我一樣的初學者有共感。

###### 情境：今天你是一位需要解決方案的B2B的Sales，要去拿下一個新案子。<br>1. 我需要詢問客戶需求（解決Jekyll的安裝問題）、目前有上有什麼資源（Ruby、Bundler）<br>2. 把客戶的資訊(解決安裝問題、有什麼版本的Ruby、Bundler)回報給公司技術團隊(ChatGPT)<br>3.挑選團隊(ChatGPT)提供的可執行方案給客戶(Terminal)。<br>4.執行上若出現問題(Terminal Error)就需要當作中間的橋樑(把Error貼給ChatGPT)。<br>5.若還是無法解決就找外部資源(Youtube、Stack Overflow)。

### 安裝

**先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本**，我犯下最大的錯誤就是沒有意識到 Ruby、Bundler、Jekyll 有版本相容性的問題，導致後來ChatGPT在幫我debug時回答的情境錯亂。

1. 
  - 我使用Mac M1，我要下載哪個版本的Ruby、Bundler?
  - 把安裝好的 ruby ＆ bundler (Terminal回覆的)版本列出來，重複詢問是否相容？  
  - 一樣把ruby ＆ bundler 版本寫出來 + "我要該安裝什麼版本的Jekyll"？
  - gem install jekyll version

順利的話到這就安裝完成但是...，但是因為我是重新安裝有刪除過部分檔案的問題就延伸出以下：
"嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem"  

<br>

### 移除後重新安裝
        # "嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。
        具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem"
        # 先依照error解除安裝
        gem uninstall jekyll
        gem uninstall google-protobuf
        gem uninstall sass-embedded
        #安裝失敗
        gem install jekyll -v '4.3.3'
        "ERROR: While executing gem ... (Gem::FilePermissionError)
        You don't have write permissions for the /usr/bin directory."

1. 詢問此 Error 原因得到以下解法：
  - 使用 --user-install 選項安裝
  - 檢查 Ruby 的安裝目錄 (安裝rbenv or rvm)
  - 確認系統設定的權限

我選擇 檢查 Ruby 的安裝目錄 (安裝rbenv) 解法試試:
    
        # 須先安裝 Homebrew 才能安裝 rbenv。
        /bin/bash -c 
        "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
        # 一樣先確認版本再安裝rbenv
        brew -v
        brew install rbenv

        #安裝完成後，還需要將 rbenv 加入到您的 shell 配置文件中
        例如 ~/.zshrc 或 ~/.bash_profile），以便每次啟動終端時都能自動加載 rbenv。

        sudo nano ~/.bash_profile
        export PATH="$HOME/.rbenv/bin:$PATH" eval "$(rbenv init -)


<br>
### 執行與重新確認版本兼容性

在Terminal check版本，這時候我發現我的 ruby 版本被控制成 ruby 3.2.5 (原為ruby 2.6.10p210) 、 Bundler version 2.5.17 (Bundler version 2.2.3)把版本資訊再丟回去GPT是否Ruby、Bundler、Jekyll 是否有兼容，沒問題的話就創建空白資料夾：

        # 在該資料夾內執行
        jekyll new . --force
        bundle exec jekyll serve

<br>
### 總結
這次安裝就不停的：  
 1.重複敘述目前環境給ChatGPT(不能當作與真人溝通，省略某些對話前提)  
 2.挑選執行方案  
 3.把error貼回ChatGPT解決

<br>