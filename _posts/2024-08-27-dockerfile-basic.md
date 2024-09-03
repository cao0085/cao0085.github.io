---
title: "Docker File Basics"
date: 2024-08-27
categories: 
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 寫一個基本功能的 DockerFile，以 SQL 為例。

<!-- 正文 -->

### 基本設置
- 建立優先順序
    - FROM：指定基礎映像，必須是第一條指令。
    - WORKDIR：設置工作目錄，通常在應用程序目錄之前。
    - RUN（安裝系統包和依賴）：在 WORKDIR 之後，COPY 和 ADD 之前。
    - 目錄結構和權限設置（RUN）：在安裝必要的系統依賴後、應用程序安裝和配置之前。
    - COPY 和 ADD：在安裝必要的系統依賴後執行。
    - RUN（應用程序安裝和配置）：在 COPY 和 ADD 之後執行。
    - USER 和用戶設置：應用程序安裝和配置完成後設置。
    - EXPOSE：通常在應用配置完成後設置，告知暴露端口。
    - CMD 和 ENTRYPOINT：通常是最後一條指令，指定容器啟動時的默認命令或腳本。

<br>

- **選擇Iamge** （`FROM`）
   - 選擇合適的基礎映像（`FROM` 指令），如 `ubuntu:latest`、`alpine:latest`、`node:latest` 或專門的映像（如 `mysql:latest` 或 `python:3.9`）
    ```dockerfile
    FROM mysql:latest
    ```

- **工作目錄設置**  (`WORKDIR`)
   - 設置工作目錄，這將是所有後續命令運行的默認目錄。例如：
   ```dockerfile
   WORKDIR / app
   ```

- **系統包安裝** (`RUN`)
   - 安裝必要的系統工具和依賴包，這可以包括 SSH 服務器、curl、git 等。使用包管理器（如 `apt-get`、`yum`、`apk`）來安裝：
   ```dockerfile
   RUN apt-get update && apt-get install -y openssh-server curl sudo
   ```

- **目錄結構和權限設置** (`RUN`)
   - 創建應用所需的目錄結構，並設置適當的權限。例如：
   ```dockerfile
   RUN mkdir -p /var/log/myapp && chown -R newuser:newuser /var/log/myapp
   ```

- **文件複製** (`COPY` 和 `ADD`)
   - 使用 `COPY` 或 `ADD` 指令將本地文件複製到映像中，這可以包括應用程序源代碼、配置文件、腳本等：
   ```dockerfile
   COPY . /app
   ADD config/myapp.conf /etc/myapp/
   ```

- **應用程序安裝和配置**
   - 如果需要，安裝應用程序和依賴包。這可以是語言運行環境（如 Node.js、Python）、數據庫（如 MySQL、PostgreSQL），或其他服務。
   - 配置應用程序文件和目錄，例如設置 SSH 的配置文件或應用程序的配置文件。

- **用戶設置** (`RUN` + `USER`)
   - 創建新的用戶並設置其權限。這樣做可以提高安全性，避免以 root 身份運行容器內的服務：
   ```dockerfile
   RUN useradd -m -s /bin/bash newuser && echo "newuser:newpassword" | chpasswd
   USER newuser
   ```

- **端口暴露** (`EXPOSE`)
   - 使用 `EXPOSE` 指令告訴 Docker 哪些端口需要暴露，便於容器間通信或與外部網絡連接。例如：
   ```dockerfile
   EXPOSE 80 443
   ```

### 進階設置

- **清理無用文件** (`RUN`)
   - 清理安裝過程中產生的無用文件，以減少映像大小。例如：
   ```dockerfile
   RUN apt-get clean && rm -rf /var/lib/apt/lists/*
   ```

- **環境變數設置** (`ENV`)
   - 使用 `ENV` 指令設置環境變數以配置應用程序。例如，設置數據庫密碼、端口號等：
   ```dockerfile
   ENV MYSQL_ROOT_PASSWORD=root_password
   ENV MYSQL_DATABASE=twitter_data
   ```

- **健康檢查** (`HEALTHCHECK`)
   - 定義容器內服務的健康檢查指令，確保服務正常運行。例如：
   ```dockerfile
   HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost/health || exit 1
   ```
- **多階段構建**
   - 使用多階段構建來減少映像大小和增加安全性，特別是對於需要構建過程的應用程序。

   ```dockerfile
   FROM golang:1.16 AS builder
   WORKDIR /app
   COPY . .
   RUN go build -o myapp

   FROM alpine:latest
   WORKDIR /app
   COPY --from=builder /app/myapp .
   CMD ["./myapp"]
   ```

- **設置時區和本地化** (`RUN` + `ENV`)
   - 設置容器內的時區和本地化設置，以滿足應用程序的地區要求。例如：
   ```dockerfile
   RUN ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
   ENV LANG en_US.UTF-8
   ```

- **安全性設置**
   - 設置安全性相關的配置，如關閉不必要的服務、設置防火牆規則、使用非 root 用戶運行等。

- **優化構建過程**
   - 使用 `.dockerignore` 文件排除不需要的文件和目錄，減少構建上下文大小。

- **持續集成/持續部署（CI/CD）集成**
   - 編寫適合於 CI/CD 流程的 Dockerfile，支持自動化構建和部署。

- **卷和持久化存儲**
   - 配置數據卷來持久化數據，防止數據因容器銷毀而丟失。例如：
   ```dockerfile
   VOLUME /var/lib/mysql
   ```

*** 

