---
title: "Jekyll basics layout"
date: 2024-08-25
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 生成第一個Jekyll專案後，添加一些功能讓網頁更完整，然後再部署到Github.pages當作Blogs！

<!-- 正文 -->

### Jekyll 專案結構

```
.
├── _config.yml       # 配置檔案
├── Gemfile           # Ruby 依賴管理檔案
├── Gemfile.lock      # 鎖定依賴版本檔案
├── index.md          # 網站首頁
├── _posts/           # 發布文章目錄
├── _drafts/          # 草稿文章目錄
├── _layouts/         # 佈局模板目錄
├── _includes/        # 可重用的 HTML 片段
├── _site/            # Jekyll 生成的靜態網站目錄
├── assets/           # 靜態資源目錄
├── categories/       # 文章分類目錄
└── ...               # 其他資料夾和檔案
```

<br>

### 網頁生成

- 生成規則
  - 根目錄中的檔案。 
  - _post資料夾中內符合命名規範的檔案。
  - 自定義有前後端（YAML Front Matter）的 html or md。

- 生成內容
  - 若檔案為完整的 html 格式則直接生成。
  - 若檔案為 md ，則需在前面指定一個 layout 模板，協助完整的網頁生成。

- 網域網址
  - _config.yml 可自定義一些設定。
  - 根目錄 / index 會默認為主網域頁面。
  - _post 內文章會根據檔案名稱。
  - 創建指定路徑或是在文件內前面指定 permalink。

<br>

### 基本架設

- 大框架設計
  - 畫 default.html 作為大框架：Jekyll 項目中的 default.html 是主要的佈局文件，定義了網站的基本結構（如 <header>, <main>, <footer> 等）。
  - 延續大框架：如果需要使用相同的大框架，頁面或文章的 YAML 前後端應設定 layout: default。目前的blog就只使用一個 default.html 當作主要 layout，可以看到主頁、目錄頁面、文章只有中間內容<main>不同。 
  - 內容生成與動態顯示：使用 Liquid 語法中的 content 當作變數放入<main>中。

  ```markdown
  {% raw %}
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>{{ page.title }}</title>
      <link rel="stylesheet" href="{{ site.baseurl }}/assets/css/medium-style.css">
  </head>

  <body>
      <!-- 導覽列 -->
      <header>
          <nav>
              <ul>
                  <li><a href="{{ site.baseurl }}/">Recent Posts</a></li>
                  <li><a href="{{ site.baseurl }}/categories/setup-basics/">Setup & Basics</a></li>
                  <li><a href="{{ site.baseurl }}/categories/code-notes/">Code Notes</a></li>
                  <li><a href="{{ site.baseurl }}/categories/algorithm-leetcode/">Algorithm & LeetCode</a></li>
              </ul>
          </nav>
      </header>
      <!-- 中間 -->
      <main>
          {{ content }}
      </main>
      <!-- 底部 -->
      <footer>
          <p>&copy; 2024 Your Name</p>
      </footer>
  </body>

  </html>
  {% endraw %}
  ```

#### 網頁內容
下面就是把上面的 <main>content<main> 變數，替換成<ul>...<ul>，以default.html為基底生成網頁。

<br>

index.md：抓取__post檔案夾內的文章當作<main>內容

```markdown
---
layout: default
---
<ul>
  <br>
  {% raw %}
  {% for post in site.posts %}
    <li style="margin-bottom: 20px;">
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
      <span>
        {{ post.date | date: "%Y-%m-%d" }}
      </span>
    </li>
  {% endfor %}
  {% endraw %}
</ul>
```
<br>

category.html：抓取__post檔案夾內的"某categories"文章當作<main>內容。 
這邊比較特別，因為有目錄格式是重複的，所以用default.html再製作一個模板 category.html。
```markdown
---
layout: default
---

<ul>
  <br>
  {% raw %}
  {% for post in site.categories[page.category] %}
    <li style="margin-bottom: 20px;">
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
      <span>
        {{ post.date | date: "%Y-%m-%d" }}
      </span>
    </li>
  {% endfor %}
  {% endraw %}
  </ul>
```

<br>


#### 執行

- 製作流程
  - 製作一個default當作主架構放入_layout資料夾。
  - 把index、文章_layout設置default。 
  - 生成你要的目錄分頁（參考GPT有很多做法）。
  - 把目錄連結複製放回default的header裡面。
  - 創建assets/css/樣式.css，一樣導入default。

- 注意
  - 本地測試時，可能會使用默認的 url 設定。上傳GitHub Pages上可能需要重新配置 _config.yml 檔案中的 baseurl 和 url 參數。
  - GitHub Pages 若url配置有問題，可以在上面的 Actions 裡找該次生成出的網頁。
  - Jekyll會自動解析文件名和關鍵字，需要注意不要有多餘的空白或大小寫不一致的情況。
  - 使用IDE建議關閉自動補齊空白等格式化功能。

<br>

### 總結
1. 命名規則和 URL 設定最麻煩，不能像是單獨寫 code 一樣隨便。
2. 若最終是要部署到其他地方，一開始就要注意。