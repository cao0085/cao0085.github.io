---
title: "Docker (4) - Docker Compose"
date: 2024-09-02
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 使用 Docker Compose 一次建立多個 Container，並利用不同的容器功能模擬實際的資訊傳遞。



<!-- 正文 -->
### 概念

<br>

Project & Service 是 Docker Compose 的核心概念。  Project 指的是 .yml 內容框架( compose version 、 services 、 networks ... ) Services 則是定義 containers 的環境。 Docker Compose 是用 Python 編寫的，通過調用 Docker API 來管理容器，只要操作平台支持 Docker API，就可以利用 Compose 來進行容器的編排管理。

<br>

- 基本指令
  - 啟動專案： Docker Compose 會發送一個 HTTP request -> Docker 引擎 ->  .yml 定義創建容器。
```bash
docker-compose up -d
```
  - 停止專案：Docker Compose 會發送請求來停止並刪除專案中的所有容器。
```bash
docker-compose down
```
  - 查看專案中的容器狀態
```bash
docker-compose ps
```

<br>

### 練習框架

- 3個需求3個容器
  - web_scraper: 使用 Python 進行網頁爬蟲，抓取所需數據。
  - mysql: 將抓取到的數據匯入 MySQL 伺服器。
  - api_service: 透過外部指令（例如：curl 和查詢語句）來推送和接收 MySQL 資料。

<br>


- 1.建立 .yaml
  - 用可預設執行的檔案名稱：docker-compose.yml
  - 此次只為了驗證流程和功能性，容器無需安裝完整的依賴。
  - port 先設置在 7000 到 7500 範圍內，避免衝突問題。

  ```yaml
  version: '3.9'

    services:
      web_scraper:
        image: python:3.9-slim-buster
        container_name: web_scraper
        volumes:
          - ./scraper:/app
        working_dir: /app
        command: bash -c "pip install mysql-connector-python && python scraper.py && tail -f /dev/null"
        ports:
          - "7002:7002"
        depends_on:
          - mysql
    mysql:
      image: mysql:8.0
      container_name: mysql
      environment:
        MYSQL_ROOT_PASSWORD: "0000"
        MYSQL_DATABASE: scraper_db
      ports:
        - "7001:3306"
      volumes:
        - mysql_data:/var/lib/mysql
    api_service:
      image: python:3.9-slim-buster
      container_name: api_service
      volumes:
        - ./api:/app
      working_dir: /app
      command: bash -c "pip install flask mysql-connector-python && python app.py"
      ports:
        - "7003:80"
      depends_on:
        - mysql
    volumes:
      mysql_data:
  ```

<br>
    
- 2.建立目錄與文件（api_service為例）
  - 創建名為 api 的新資料夾，該資料該會對應到容器內的 volumes。
  - 工作目錄為 working_dir: /app 所以將執行 app.py 放入 api 資料夾。

  ```markdown
  docker-scrap/
  ├── docker-compose.yml
  ├── api/
      └── app.py
  ```

  for local curl to MySQL
  ```python 
  from flask import Flask, request, jsonify
  import mysql.connector

  app = Flask(__name__)

  # 設定 MySQL 連接
  db = mysql.connector.connect(
      host="mysql",
      user="root",
      password="0000",
      database="scraper_db")

  @app.route('/query', methods=['POST'])
  def query_mysql():
      sql_query = request.form.get('query')
      cursor = db.cursor()
      try:
          cursor.execute(sql_query)

          # 如果有結果集，將其讀取
          if cursor.with_rows:
              results = cursor.fetchall()
          else:
              results = []

          db.commit()
          return jsonify(results)
      except Exception as e:
          return str(e), 400
      finally:
          cursor.close()  # 確保 cursor 在完成後被關閉

  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=80)

  ```
  
  scrap to MySQL
  ```python
  import mysql.connector

  # 設定 MySQL 連接
  db = mysql.connector.connect(
      host="mysql",  # 使用 Docker Compose 中服務的名稱 "mysql"
      user="root",
      password="0000"
  )

  cursor = db.cursor()

  # 建立資料庫
  cursor.execute("CREATE DATABASE IF NOT EXISTS db_20240904;")
  cursor.execute("USE db_20240904;")

  # 建立表格
  cursor.execute("""
  CREATE TABLE IF NOT EXISTS test1 (
      id INT,
      value CHAR(1)
  );
  """)

  # 插入資料
  data = [
      (1, 'a'),
      (2, 'b')
  ]

  cursor.executemany("INSERT INTO test1 (id, value) VALUES (%s, %s);", data)
  db.commit()

  print("Database and table created, data inserted successfully.")

  # 關閉 cursor 和資料庫連接
  cursor.close()
  db.close()
  ```

<br>

- 3.啟動 Docker Compose  

  - 在專案資料夾內執行，d 標誌表示以分離模式在後台運行
  - 確保所有容器都正常啟動並運行。
```
docker-compose up -d
```
```
docker-compose ps
```
<br>

### 操作與執行

<br>

我們的需求是在本地端輸入提取 MySQL 資料 ，所以先了解 Docker Compose 和 MySQL 的收送資料方式。

<br>

- Docker Compose
  - Docker Compose 啟動容器時，會自動創建一個內部 DNS 解析系統。
  - 當一個容器需要與另一個容器通信時，Docker 的內部 DNS 會自動解析服務名稱為對應的容器 IP 地址，這樣容器就能通過服務名稱進行通信，而不需要知道具體的 IP 地址。

<br>

- MySQL
  1. MySQL 原生協議：
    - 使用 MySQL 的原生二進制協議進行通信，這是最常用的方式。MySQL 客戶端（如 `mysql` 命令行工具）和各種程式語言的 MySQL 驅動程序（如 Python 的 `mysql-connector-python` 或 PHP 的 `mysqli`）。

  2. HTTP/JSON 接口（需中間層）：
    - MySQL 本身不直接支持 HTTP 協議，但是可以使用中間層服務（如 PHP、Python 的 Flask/Django 等 Web 框架）通過 HTTP 接口接收請求，然後將這些請求轉換為 MySQL 指令。這樣就可以通過 HTTP/JSON 的方式間接與 MySQL 進行交互。

  3. ODBC（Open Database Connectivity）：
    - 通過 ODBC 驅動程序與 MySQL 通信。這種方法主要用於需要與不同類型數據庫進行互操作的應用程序，例如在 Windows 環境中通過 ODBC 驅動器進行資料庫連接。

  4. JDBC（Java Database Connectivity）：
    - Java 應用程序通過 JDBC 驅動程序與 MySQL 進行通信。這種方法是所有基於 Java 的應用程序訪問 MySQL 的主要方式。

<br>
 

- 流程
  - 使用前重啟 Docker Compose
  - 本地request -> Docker Compose 內部處理轉換資訊 -> respones給本地
  

  <br>

  1. Local Send Http Requests：
  ```bash
  curl -X POST http://localhost:7003/query -d "query=SELECT * FROM db_20240904.test1;"
  ```
    - 在本地終端使用 `curl` 發送 HTTP POST 請求給 api container (port：7003:80)。

  2. Api Container Get Requests & Send Requests：
    - service 有設定啟動 api container 時會運行資料夾內的 app.py。
    ```
  bash -c "pip install flask mysql-connector-python && python app.py"
    ```
    - 在 app.py 中 Flask 應用被設計為啟動後立即監聽所有網絡接口上的端口 80 。
    ```
  app.run(host='0.0.0.0', port=80)
  # docker file有設定container expose port80
    ```
    - 所以從 curl request 透過本地 7003 -> 容器 80 -> app.py。 
    - Flask 應用會將收到的 HTTP 請求中的數據作為 SQL 查詢語句，使用 MySQL 驅動程序（如 mysql-connector-python），通過 Docker 內部網絡發送給 MySQL 服務器（mysql 容器）。

  3. MySQL Get Requests & Send Respones：
    - MySQL 服務器接收到來自 Flask 的 SQL 查詢後，執行相應的操作（如創建表、插入數據等），並生成執行結果。
    - 結果（例如操作成功或失敗的消息，查詢的結果集等）返回給 Flask 應用。

  4. Api Container Send Respones ：
    - Flask 接收到 MySQL respones 會將其轉換為 HTTP respones（JSON 格式），然後返回給發送請求的客戶端。

<br>

### 總結
1. 因為 docker compose 是一次啟動所有容器，各別的容器啟動後會各自執行任務。所以容器間有依賴關係時可能會有問題（SQL伺服器還沒有架好，API就傳送請求 ）。
2. Scrap 那邊應該要透過 API 進 SQL 而不是直接引用 Python 設計的 SQL Function 抓進去。