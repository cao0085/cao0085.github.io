---
layout: post
title:  "How to Install Jekyll Step by Step with ChatGPT"
date:   2024-08-07 23:56:28 +0800
categories: jekyll update
---

第一次安裝時無腦複製貼上ChatGPT的指令安裝成功，之後想練習重新安裝就遇到很多障礙，多弄了二個小時後冷靜下來才又成功...

分享重新安裝的流程

想使用 Jekyll 須安裝 Ruby、Bundler

***先確認！！ 先確認！！目前電腦內的 Ruby、Bundler的版本***
ruby -v
bundler -v

Q1：先從ruby開始問 -> "我使用(Mac M1)，我要如何確認下載哪個版本符合需求" or "我目前的版本是 ruby.xx 我需要下載哪個版本的bundler" ?
    -> 根據回答下載 ruby ＆ bundler。

Q2：把安裝好的 ruby ＆ bundler 版本列出來再重複詢問一次雙重確認是否相容

Q3：一樣把ruby ＆ bundler 版本寫出來 + "我要如何知道我要安裝什麼版本" 根據回答安裝

問題開始...
    因為我這次是重新安裝，在之前的無腦照GPT回答複製貼上，版本問題就開始浮現
    -> ""嘗試加載 Jekyll 相關的 Ruby gem 時出現了問題。具體來說，錯誤涉及到 google-protobuf 和 sass-embedded gem
    建議嘗試重新安裝 Jekyll 和相關的 gem 來解決兼容性問題""
        gem uninstall jekyll
        gem uninstall google-protobuf
        gem uninstall sass-embedded
        舊版本移除

    成功執行上述步驟後執行-> gem install jekyll -v '4.3.3'
    ->ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /usr/bin directory.

    然後詢問原因給了幾種解法
    1.	使用 --user-install 選項安裝
    2.	檢查 Ruby 的安裝目錄 (安裝rbenv or rvm)
    3.	確認系統權限

    那我想要執行這個 安裝 rbenv：
    先安裝 Homebrew 才能安裝 rbenv ，一樣要先確認版本 brew -v。
    brew install rbenv
    安裝完成後，需要將 rbenv 加入到您的 shell 配置文件中（例如 ~/.zshrc 或 ~/.bash_profile），以便每次啟動終端時都能自動加載 rbenv。
    添加以下內容到 ~/.bash_profile：

    nano ~/.bash_profile ->

    export PATH="$HOME/.rbenv/bin:$PATH"
    eval "$(rbenv init -)"

    這時候又碰到一些問題 Terminal ： [ Cannot open file for writing: Permission denied ] 
    解決方法：sudo nano ~/.bash_profile

    成功改完之後我一樣先check版本
    ruby -v 、 bundler -v 、jekyll -v
    這時候我發現我的 ruby 版本被控制成 ruby 3.2.5 (原本是ruby 2.6.10p210) 、 Bundler version 2.5.17 (Bundler version 2.2.3)

    在 Terminal 確認完後把版本資訊再丟回去GPT是否Ruby、Bundler、Jekyll 有兼容
    -> 沒問題就直接執行
    -> 創建新資料夾 -> jekyll new . --force (初始化)
                  -> bundle exec jekyll serve
    成功！！





Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
