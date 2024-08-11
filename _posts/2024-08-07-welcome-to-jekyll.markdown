---
layout: post
title:  "How to Install Jekyll Step by Step with ChatGPT"
date:   2024-08-07 23:56:28 +0800
categories: jekyll update
---

Jekyll 是使用 Ruby 語言編寫的網站生成器。因此需先要安裝 Ruby、Bundler：
	1.	安裝 Ruby：Jekyll 需要 Ruby 的支持。可以使用版本管理工具如 rbenv 來管理 Ruby 版本。
	2.	安裝 Bundler：Bundler 是 Ruby 的一個套件管理工具，用於安裝和管理 Jekyll 及其依賴的 gems（Ruby 的軟件包）


首次安裝時順順下指令給ChatGPT複製貼上指令安裝成功，後來刪除到Jeykll生成出的資料夾內檔案無法正常開啟。
重新安裝就遇到很多障礙，多弄了三個小時後冷靜下來才又成功，分享重新安裝的流程順便記錄一下目前對於工具使用上的思考。

在安裝之前想先模擬一個情境，看能不能讓和我一樣的初學者有共感：
今天你是一個B2B的sales，要去滿足客戶的需求(ex:解決Jekyll的安裝問題)。
-> 所以我首先要把客戶需求("解決Jekyll的安裝問題")回報給公司技術團隊(ChatGPT)。
-> 挑選可執行的方案給客戶(Terminal)，若出現問題的話(Terminal Error)就需要來溝通(把Error貼給ChatGPT)
-> 若還是無法解決就找外部資源(Youtube、Stack Overflow)

按照這個概念來解決 Jekyll 安裝的問題吧！

***先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本***
***先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本***
我重新安裝最大的錯誤就是沒有意識到 Ruby、Bundler、Jekyll 有版本相容性的問題，導致ChatGPT在幫我debug時回答的前提和實際環境不一樣。


Step1. 詢問Ruby、Bundler版本下載
  我使用Mac M1，我要下載哪個版本的Ruby、Bundler?

Step2. 確認版本相容
  把安裝好的 ruby ＆ bundler (Terminal回覆的)版本列出來，重複詢問是否相容？

Step3. 確認可以安裝的Jekyll版本
  一樣把ruby ＆ bundler 版本寫出來 + "我要如何知道該安裝什麼版本的Jekyll"？


***因為我是重新安裝，之前手賤刪除部分檔案的問題就延伸出以下***
  "嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem"
  "建議嘗試重新安裝 Jekyll 和相關的 gem 來解決兼容性問題"

Step4. 移除之前的檔案
  -> gem uninstall jekyll
  -> gem uninstall google-protobuf
  -> gem uninstall sass-embedded

Step5. 重新安裝Jekyll
  -> gem install jekyll -v '4.3.3'
  "ERROR:  While executing gem ... (Gem::FilePermissionError) You don't have write permissions for the /usr/bin directory."

  詢問權限原因給了幾種解法：
  1. 使用 --user-install 選項安裝
  2. 檢查 Ruby 的安裝目錄 (安裝rbenv or rvm)
  3. 確認系統權限

      我選擇安裝 rbenv 這個 ruby 版本控制的解決辦法根據指示操作：
      -> 須先安裝 Homebrew 才能安裝 rbenv ，一樣要先確認版本 brew -v。
      -> brew install rbenv
      -> 安裝完成後，需要將 rbenv 加入到您的 shell 配置文件中（例如 ~/.zshrc 或 ~/.bash_profile），以便每次啟動終端時都能自動加載 rbenv。
      -> sudo nano ~/.bash_profile 添加 export PATH="$HOME/.rbenv/bin:$PATH" eval "$(rbenv init -)"
  
  安裝 rbenv 完成！

Step6. 重新確認目前版本和兼容性
  在Terminal check版本，這時候我發現我的 ruby 版本被控制成 ruby 3.2.5 (原本是ruby 2.6.10p210) 、 Bundler version 2.5.17 (Bundler version 2.2.3)
  把版本資訊再丟回去GPT是否Ruby、Bundler、Jekyll 是否有兼容


Step7. 執行Jekyll
  創建新資料夾
  -> jekyll new . --force (初始化)
  -> bundle exec jekyll serve
  終於成功！

總結一下，這次的安裝流程就是不停的：
1. 重複敘述目前環境(版本)給ChatGPT
    ChatGPT 可能會給 a - b - c - d - e 五個解題步驟，你會根據 a 指令延伸出的問題詢問解決，回到 b 、 c ...。
    在要解 d 時可能已經又問了10個問題，這時就建議問"我目前a、b、c的狀態是xx"，我現在有d問題，不能當作與真人溝通一樣，省略稍早的對話的前提。
2. 挑選執行方案
3. 把error貼回ChatGPT解決




Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
