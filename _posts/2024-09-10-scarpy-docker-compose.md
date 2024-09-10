---
title: "Basic Scrapy Setup with Docker Compose (2)"
date: 2024-09-10
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
######  添加一個定時執行爬蟲的功能，並嘗試用 dockerfile 做 docker-compose

<!-- 正文 -->

### 前言

這次的目標是添加一個定時執行爬蟲的功能，需要釐清哪些 Cron 指令應該寫在 Dockerfile 裡，哪些應該放在 `docker-compose.yml` 裡，實際操作起來比預想的困難。我的流程是先建立並測試一個 Dockerfile，然後讓 Docker Compose 使用這個 Dockerfile 作為基礎來運行容器。

<br>

#### scraper-dockerfile

```dockerfile
# 使用 python:3.9-slim-buster 作為基礎映像
FROM python:3.9-slim-buster

# 設定時區為台灣
ENV TZ=Asia/Taipei

# 更新系統並安裝必要的套件
RUN apt-get update && apt-get install -y \
    cron \
    tzdata \
    procps \
    && rm -rf /var/lib/apt/lists/*

# 安裝 mysql-connector-python
RUN pip install mysql-connector-python

# 確保 /app 目錄存在
RUN mkdir -p /app

# 創建 cronjob 文件，添加兩個任務
# 創建 cronjob 文件，添加兩個任務並設置時區
RUN echo "TZ=Asia/Taipei" > /etc/cron.d/my-cron-job && \
    echo "* * * * * root date >> /app/cronrecord.log 2>&1" >> /etc/cron.d/my-cron-job && \
    echo "* * * * * root /app/command.sh >> /app/cronrecord.log 2>&1" >> /etc/cron.d/my-cron-job
    
# 設定 cronjob 文件的權限
RUN chmod 0755 /etc/cron.d/my-cron-job

# 確保 cron 的日誌檔案存在
RUN touch /app/cronrecord.log

# 啟動 cron 並保持容器運行（使用 JSON 格式來避免 OS 信號問題）
CMD ["cron", "-f"]
```

- Cron Setting
  1. 使用 dockerfile cron 指令要下 `<user>`，和開啟權限這樣才可以正確執行。
  2. 確認自動生成的 cronrecord.log 路徑
  3. 主任務：執行 `command.sh` 的腳本（未創建），若需要添加任務只需在這個腳本裡進行修改。
  4. 監控任務：用 date 紀錄有沒有正常運行。

<br>

#### zone.env
```
TZ=Asia/Taipei
```
#### scraper-entry.sh
```
#!/bin/bash

# 啟動 cron 服務
service cron start

# 確保 command.sh 存在並具有執行權限
touch /app/command.sh
chmod +x /app/command.sh

# 執行 Python 腳本
python /app/scraper.py

# 保持容器前台運行
tail -f /dev/null

```

### 完成後專案目錄結構

```
/ (root directory)
│
├── docker-compose.yml          # Defines the services for Docker Compose
│
├── scraper-dockerfile           # Dockerfile for web_scraper service
│
├── zone.env                     # Environment variables file (e.g., TZ=Asia/Taipei)
│
├── scraper/                     # Directory for the scraper-related files
│   ├── scraper.py               # Python script for web scraping
│   └── command.sh               # for cronjob
│   └── scraper-entry.sh         # for dockerfile command
│
├── api/                         # Directory for the API service
│   └── app.py                   # Python Flask API file
│
└── mysql_data/                  # Volume used by MySQL for data persistence (managed by Docker, not a local folder)
```

#### Docker Compose
```yml
version: '3.9'

services:
  web_scraper:
    image: python-cron
    container_name: web_scraper
    build:
      context: .
      dockerfile: scraper-dockerfile
    volumes:
      - ./scraper:/app
    working_dir: /app
    command: /app/scraper-entry.sh
    ports:
      - "7002:7002"
    depends_on:
      - mysql
    env_file:
      - ./zone.env  # 引用全局環境變數文件

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
    env_file:
      - ./zone.env  # 引用全局環境變數文件
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h localhost"]
      interval: 10s
      retries: 5
      start_period: 30s
      timeout: 5s

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
    env_file:
      - ./zone.env  # 引用全局環境變數文件

volumes:
  mysql_data:
```

<br>

### 結語

這次算是我第一次寫所需的環境參數，過程中熟悉 Linux command 、哪些指令放在 entrypoint.sh 哪些寫在 Dockerfile or docker-compose.yml。  

<br>  

另外還有" Docker Compose 初次啟動時，MySQL 服務尚未完全啟動，API 和 Python 程序就已經開始運行並嘗試連接數據庫導致連接失敗" 這個問題未解決。