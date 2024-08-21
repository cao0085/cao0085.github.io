---
title: How to Install Jekyll Step by Step with ChatGPT
layout: post
date: '2024-08-07 23:56:28 +0800'
categories: setup-basics
---

<br>
> **Jekyll 是使用 Ruby 語言編寫的網站生成器，因此需先安裝 Ruby、Bundler：**  
> 
> 1. Ruby：Jekyll 需要 Ruby 的支持。可以使用版本管理工具如 rbenv 來管理 Ruby 版本。  
> 
> 2. Bundler：Bundler 是 Ruby 的一個套件管理工具，用於安裝和管理 Jekyll 及其依賴的 gems。   

<br>
### **前言**

首次安裝時順順下指令給ChatGPT就成功，後來刪除到Jeykll生成出的某些檔案無法正常開啟。
重新安裝就遇到很多障礙，多弄了三個小時後冷靜下來才又成功。在操作安裝前想先模擬一個情境，分享目前對於使用ChatGPT的理解，看能不能讓和我一樣的初學者有共感。
<br>
<br>
#### *情境：今天你是一個B2B的Sales，要去拿下一個新案子*
* 首先詢問客戶木有什麼需求（解決Jekyll的安裝問題）、目前有上有什麼資源（Ruby、Bundler）
* 所以我要把客戶的資訊("解決Jekyll的安裝問題"、有什麼版本的Ruby、Bundler)回報給公司技術團隊(ChatGPT)
* 挑選團隊(ChatGPT)提供的可執行方案給客戶(Terminal)，執行上若出現問題(Terminal Error)就需要當作中間的橋樑(把Error貼給ChatGPT)
* 若還是無法解決就找外部資源(Youtube、Stack Overflow)

按照這個概念來解決 Jekyll 安裝的問題吧！

<br>

### **安裝**


*先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本*  
*先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本* 

我犯下最大的錯誤就是沒有意識到 Ruby、Bundler、Jekyll 有版本相容性的問題，導致後來ChatGPT在幫我debug時回答的情境錯亂。


Step1. 詢問Ruby、Bundler版本下載
  我使用Mac M1，我要下載哪個版本的Ruby、Bundler?

Step2. 確認版本相容
  把安裝好的 ruby ＆ bundler (Terminal回覆的)版本列出來，重複詢問是否相容？
```
 ->	ruby -v
 ->	bundler -v
```

Step3. 安裝Jekyll
  一樣把ruby ＆ bundler 版本寫出來 + "我要該安裝什麼版本的Jekyll"？  
	`gem install jekyll xxx`
  
	
順利的話到這就安裝完成就可以按照Step7執行了，但是...
<br>
<br>
-----
*但是因為我是重新安裝，之前手賤刪除部分檔案的問題就延伸出以下error*  

  "嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem"  
	
  "建議嘗試重新安裝 Jekyll 和相關的 gem 來解決兼容性問題"  
<br>
<br>
Step4. 移除之前的檔案
```
  -> gem uninstall jekyll
  -> gem uninstall google-protobuf
  -> gem uninstall sass-embedded
```
<br>
Step5. 重新安裝Jekyll  
```
  -> gem install jekyll -v '4.3.3'
  "ERROR: While executing gem ... (Gem::FilePermissionError) You don't have write permissions for the /usr/bin directory."
```
<br>
 詢問此ERROR(權限)原因給了幾種解法：
* 使用 --user-install 選項安裝
* 檢查 Ruby 的安裝目錄 (安裝rbenv or rvm)
* 確認系統權限

我選擇安裝 rbenv 這個 ruby 版本控制的解決辦法：
須先安裝 Homebrew 才能安裝 rbenv ，安裝完成一樣要先確認版本
```
 -> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
 -> brew -v。
 -> brew install rbenv
```
安裝完成後，還需要將 rbenv 加入到您的 shell 配置文件中（例如 ~/.zshrc 或 ~/.bash_profile），以便每次啟動終端時都能自動加載 rbenv。
```
 ->sudo nano ~/.bash_profile
添加 export PATH="$HOME/.rbenv/bin:$PATH" eval "$(rbenv init -)"
```

安裝 rbenv 完成！
<br>
<br>

Step6. 重新確認目前版本和兼容性  

在Terminal check版本，這時候我發現我的 ruby 版本被控制成 ruby 3.2.5 (原為ruby 2.6.10p210) 、 Bundler version 2.5.17 (Bundler version 2.2.3)
把版本資訊再丟回去GPT是否Ruby、Bundler、Jekyll 是否有兼容
<br>
<br>
Step7. 執行Jekyll
* 創建新資料夾在**資料夾內執行**
```
jekyll new . --force
bundle exec jekyll serve
```
成功！
<br>
<br>

### **總結**
這次的安裝流程就是不停的：
* 重複敘述目前環境給ChatGPT(不能當作與真人溝通，省略某些對話前提)
* 挑選執行方案
* 把error貼回ChatGPT解決

<br>



Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
