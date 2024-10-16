---
title: "Docker (3) - Connecting to MySQL in Docker"
date: 2024-08-29
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 透過SSH隧道讓本地電腦對 MySQL Container 下指令，嘗試模擬伺服器之間傳遞資訊。<br>*Local -> Container -> Container's MySQL Server


<!-- 正文 -->

<br>

- Terminal 1.『 MySQL 基本設置 』
    - Container 內運行安裝命令 MySQL 
    - 修改 MySQL default setting：port -> 3006、bin -> 0.0.0.0
    - 設置 Root 密碼
    - 開啟 SSH & MySQL

<br>

- Terminal 2.『 建立 SSH 隧道 』

    ```bash
    ssh -L 3307:localhost:3306 user1@localhost -p 5566
    ```

  - 在本地電腦的 `3307` 端口和 Docker 容器內部的 `3306` 端口之間建立一條隧道。  
local port 3307 -> Docker container’s port 5566 (SSH) -> MySQL port 3306

<br>

- Terminal 3.『 本地連接 MySQL 客戶端 』
    ```bash
    mysql -h 127.0.0.1 -P 3307 -u root -p
    ```
    - `-h 127.0.0.1`: 連接到本地的 IP 地址（即本機）。
    - `-P 3307`: local port 3307 通過 SSH 隧道映射到了container's MySQL port:3006。
    - `-u root -p`: 使用 root 用戶進行登錄，並提示輸入密碼。


### 結論

1. 了解基本的資訊傳遞、遠端操作