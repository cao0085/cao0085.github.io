---
title: "Scaping data on twitter"
date: 2024-09-04
categories: code-notes
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 以為目標，擷取數據

<!-- 正文 -->

### Twitter API
失敗

### Twikit

Twikit 是一個用來解析 Twitter 網頁並擷取推文數據的非官方工具。這個工具是基於 Python 寫成的，通過模擬網頁請求來獲取公開推文和用戶資料。由於 Twikit 是非官方工具，它不需要 Twitter API 的權限，因此適合你目前沒有足夠 API 權限的情況。

### Twikit 的功能

1. **擷取特定用戶的推文**：你可以獲取特定用戶的最新推文，包括推文內容、發佈時間、轉推和喜歡數等。
2. **擷取推文的詳細資訊**：包括推文的所有回覆、引用推文、轉推和喜歡數。
3. **追蹤趨勢**：可以透過特定關鍵詞或主題，追蹤其相關的最新推文。

### Twikit 的使用方式

1. **安裝 Twikit**：
   你需要先安裝 Twikit。假設它是作為一個 Python 套件分發的，你可以使用 pip 進行安裝：

   ```bash
   pip install twikit
   ```

2. **基本用法**：
   使用 Twikit 通常只需要指定目標用戶或關鍵字，然後進行爬取。例如，擷取特定用戶的推文可以這樣做：

   ```python
   from twikit import Twikit

   # 初始化 Twikit
   twikit = Twikit()

   # 擷取某個用戶的最新推文
   tweets = twikit.get_user_tweets('twitter_username')

   # 顯示推文
   for tweet in tweets:
       print(tweet.text)
   ```

3. **擷取趨勢資料**：
   如果你要擷取趨勢資料，可以針對特定關鍵字或話題進行爬取。Twikit 可能也支援搜尋功能，你可以這樣進行：

   ```python
   # 擷取關鍵字的最新推文
   trend_tweets = twikit.search_tweets('Taiwan trending')

   # 顯示趨勢推文
   for tweet in trend_tweets:
       print(tweet.text)
   ```

### 優點和限制

- **優點**：
  - 不需要 API 權限，適合權限受限的使用情況。
  - 可以擷取公開推文和資料。

- **限制**：
  - Twikit 是非官方工具，使用時需要注意 Twitter 的服務條款。
  - 解析網頁的方式可能會隨 Twitter 網頁結構的改變而失效。
  - 效能可能比不上官方 API。

***

pip install twikit

檢查模組的內容：你可以使用以下命令來查看 twikit 模組中有哪些可用的屬性或類：
import twikit
print(dir(twikit))


`ConfigParser` 是 Python 標準庫中的一個模組，屬於 `configparser`。它主要用於讀取和寫入配置文件（通常是 `.ini` 格式的文件）。這些配置文件通常用來儲存應用程序的設定，比如資料庫連接資訊、應用程序選項等。

`ConfigParser` 提供的方法允許你方便地操作這些設定文件，例如讀取、修改、添加和刪除設定。這些 `.ini` 文件的格式通常如下：

```ini
[SectionName]
option1 = value1
option2 = value2
```

以下是一個簡單的示例，說明如何使用 `ConfigParser` 來讀取和寫入 `.ini` 配置文件：

```python
from configparser import ConfigParser

# 創建 ConfigParser 物件
config = ConfigParser()

# 讀取配置文件
config.read('example.ini')

# 獲取某個 section 下的選項值
value = config.get('SectionName', 'option1')
print(value)

# 修改配置文件
config.set('SectionName', 'option1', 'new_value')

# 添加新的 section 和選項
config.add_section('NewSection')
config.set('NewSection', 'option3', 'value3')

# 寫回到文件
with open('example.ini', 'w') as configfile:
    config.write(configfile)
```

這段程式碼展示了如何讀取、修改和寫入 `.ini` 配置文件。使用 `ConfigParser` 可以讓你以結構化的方式管理配置文件，非常適合於需要靈活配置的應用程式。

