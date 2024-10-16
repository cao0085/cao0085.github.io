---
title: "Install Jekyll with ChatGPT"
date: 2024-08-07
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### Jekyll 是一款用於靜態網站生成的開源工具，使用者只需撰寫 Markdown 或 HTML 格式的文件即可創建網站。Jekyll 生成的網站運行快速且易於部署，特別適合在 GitHub Pages 等靜態託管平台上使用。

<!-- 正文 -->

### 開始安裝

Jekyll 是使用 Ruby 編寫的網站生成器，須先安裝**Ruby**、**Bundler** (用於安裝和管理 Jekyll 及其依賴的gems)。若是有下載過先確認目前電腦內的 Ruby、Bundler的版本，這次安裝我犯的錯就是不知道 Ruby、Bundler、Jekyll 有版本相容性的問題，導致詢問ChatGPT時鬼打牆。


- 此次的問題
  1. 我使用Mac M1，我要下載哪個版本的Ruby、Bundler?
  2. 把安裝好的 ruby ＆ bundler (Terminal回覆的)版本列出來，重複詢問是否相容？  
  3. 一樣把ruby ＆ bundler 版本寫出來 + "我要該安裝什麼版本的Jekyll"？
  4. gem install jekyll version

順利的話到這就安裝完成，但是我因為手賤亂動檔案就要重新安裝！  

<br>

#### 1.移除後重新安裝
        # "嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。
        具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem"
        # 先依照error解除安裝
        gem uninstall jekyll
        gem uninstall google-protobuf
        gem uninstall sass-embedded
        gem install jekyll -v '4.3.3'
        #安裝失敗
        "ERROR: While executing gem ... (Gem::FilePermissionError)
        You don't have write permissions for the /usr/bin directory."

- 詢問此 Error 原因得到以下解法：
  - 使用 --user-install 選項安裝
  - 檢查 Ruby 的安裝目錄 (安裝rbenv or rvm)
  - 確認系統設定的權限

#### 2.檢查 Ruby 的安裝目錄解法試試:
    
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

#### 3.執行與重新確認版本兼容性

在Terminal check版本，這時候我發現我的 ruby 版本被控制成 ruby 3.2.5 (原為ruby 2.6.10p210) 、 Bundler version 2.5.17 (Bundler version 2.2.3)把版本資訊再丟回去GPT是否Ruby、Bundler、Jekyll 是否有兼容，沒問題的話就創建空白資料夾：

        # 在該資料夾內執行
        jekyll new . --force
        bundle exec jekyll serve

<br>

### 總結
1.學習安裝框架，理解事前準備與環境的重要性  
2.和 GPT 該怎麼交流，有意識的挑戰它

<br>