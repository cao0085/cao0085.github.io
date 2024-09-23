---
title: "Linux Commands"
date: 2024-09-09
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### Dockerfile 經常會需要使用到 Linux 指令。記錄一下經常用到的重要 Linux 指令。



### **文件與目錄操作**

<br>

#### 1. echo  
`echo` 用來在終端輸出文字或變數值。在 Dockerfile 中，`echo` 常被用來創建文件或添加簡單的訊息到腳本或日誌中。

```bash
echo "Hello, Docker!" > /app/greeting.txt
```

- 寫入規則
  - 創建文件：如果文件不存在，echo "內容" > 文件名 會創建一個新的文件，並將 “內容” 寫入其中。
  - 覆蓋文件：如果文件已經存在，使用 > 會覆蓋文件中的所有內容。
  - 追加到文件：如果你不想覆蓋已有的文件內容，而是希望將新內容追加到文件的末尾，可以使用 >>，這樣文件內容會被保留，新內容會被追加到最後一行。

#### 2. touch  
`touch` 用來創建空文件或更新已存在文件的修改時間。在 Dockerfile 中，`touch` 可以確保某些日誌文件存在，這樣應用或 `cron` 任務就可以正常寫入。

```bash
touch /var/log/cron.log
```

- 特性
  - 創建文件：如果文件不存在，touch 會創建一個新的空文件，不包含任何內容。
  - 更新修改時間：如果文件已經存在，touch 不會改變文件內容，只會更新文件的修改時間（timestamp），這對於某些情境下非常有用，比如用來標記文件已被「訪問」或「更新」過。

#### 3. mkdir  
`mkdir` 用來創建目錄。在 Dockerfile 或命令行中，經常需要創建新目錄以存放應用程序或日誌文件。

```bash
mkdir -p /app/logs
```
- 創建規則
  - 不帶 -p 參數時：使用 mkdir 創建目錄時，在路徑完全正確下才會成功創建目標目錄。
  - 帶 -p 參數時：-p 參數會讓 mkdir 會在父目錄不存在的情況，自動創建所需的父級目錄完成目標目錄。


#### 4. cp  
`cp` 用來複製文件或目錄，且要求路徑完全正確才能成功複製文件或目錄。

```bash
cp /source/config.json /app/(自定義名稱)
```
- 覆蓋規則
  - 目標是文件：會覆蓋該文件，可使用 -i 在覆蓋前提示確認。
  - 目標是目錄：且目錄已經存在，cp 會將文件複製到該目錄中。
  - 目標是目錄：-r 複製一個目錄及其所有內容。

#### 5. mv  
`mv` 用來移動或重命名文件。在 Dockerfile 中，有時需要將臨時文件移動到指定位置，或更改文件名稱。

```bash
mv /tmp/file.txt /app/file.txt
```
這行指令會將 `file.txt` 從 `/tmp` 移動到 `/app` 目錄中。

#### 6. rm  
`rm` 用來刪除文件或目錄。在 Dockerfile 中，經常需要刪除臨時文件來減少映像大小。

```bash
rm -rf /tmp/*
```
這行指令會遞歸刪除 `/tmp` 目錄中的所有文件和子目錄。

---

### **文件權限與環境管理**

<br>

#### 7. chmod  
`chmod` 用來更改文件或目錄的權限，權限設置通常分為三類：文件所有者、組和其他用戶，每類用戶可以擁有讀（r）、寫（w）、和執行（x）權限。

```bash
chmod +x /app/script.sh
```
- 權限設置
  - 4：讀取權限（r）
  - 2：寫入權限（w）
  - 1：執行權限（x）
  - 7：讀、寫、執行權限（rwx = 4 + 2 + 1）
  - 6：讀、寫權限（rw- = 4 + 2）
  - 5：讀、執行權限（r-x = 4 + 1）
  - 4：只有讀取權限（r--）

#### 8. env  
`env` 用來查看或設置環境變量。在 Dockerfile 中，環境變量對於配置應用非常重要，可以通過 `ENV` 指令來設置變量。

```bash
ENV APP_ENV=production
```
這行指令設置了一個名為 `APP_ENV` 的環境變量，並將其值設為 `production`。

#### 9. export  
`export` 用來導出環境變量，使其在當前 shell 的子進程中可用。在腳本或 Dockerfile 中，這個命令可以用來設置變量供後續命令使用。

```bash
export PATH=$PATH:/usr/local/bin
```
這行指令會將 `/usr/local/bin` 添加到當前會話的 `PATH` 中。

---

### **系統狀態與進程管理**

#### 10. ps aux  
`ps aux` 是一個系統監控指令，用來列出系統中當前正在運行的所有進程。當你想要檢查 `cron` 或其他進程是否正確運行時，這個指令很有用。

```bash
ps aux | grep cron
```
這行指令會列出所有正在運行的 `cron` 進程，幫助你確保 `cron` 正在正常運行。

#### 11. top  
`top` 用來實時顯示系統進程和資源使用情況。在 Docker 容器中運行這個指令可以幫助你監控應用的資源消耗情況。

```bash
top
```
這行指令會顯示當前運行的進程列表，並動態更新。

#### 12. kill  
`kill` 用來終止運行中的進程。當某個進程無法正常停止時，這個命令可以強制終止它。

```bash
kill -9 1234
```
這行指令會強制終止進程 ID 為 1234 的進程。

#### 13. df -h  
`df -h` 用來查看文件系統的磁碟空間使用情況。這個命令對於檢查容器內的磁碟空間是否充足非常有用。

```bash
df -h
```
這行指令會以人類可讀的格式（以 GB、MB 為單位）顯示每個磁碟分區的使用情況。

---

### **排程與日誌管理**

<br>

#### 14. crontab  
`crontab` 是用來管理 `cron` 任務的工具。當你需要定期執行腳本時，通常會在 Dockerfile 中設置一個 `cronjob`，並使用 `crontab` 將任務加入排程中。

```bash
echo "* * * * * root /app/script.sh >> /var/log/cron.log 2>&1" 
> /etc/cron.d/my-cron-job && chmod 0644 /etc/cron.d/my-cron-job
```
這行指令會每分鐘執行一次 `/app/script.sh`，並將其輸出寫入到 `/var/log/cron.log`。

#### 15. cat  
`cat` 用來顯示文件的內容。在調試或日誌查看時，`cat` 是快速查看文件內容的好工具。

```bash
cat /var/log/cron.log
```
這行指令會顯示 `cron.log` 文件的全部內容。

#### 16. tail -f  
`tail -f` 用來實時查看文件的最後幾行，並持續更新。這個指令常用於查看日誌文件的動態輸出。在 Docker 容器中，這個指令經常被用來保持容器不退出，並允許你查看日誌輸出。

```bash
cron && tail -f /var/log/cron.log
```
這行指令會啟動 `cron` 服務，並實時顯示 `cron` 日誌內容。

#### 17. grep  
`grep` 用來在文本中搜尋特定的模式。這個指令經常與其他命令一起使用，用於過濾出需要的信息。

```bash
grep "ERROR" /var/log/app.log
```
這行指令會在 `app.log` 文件中搜尋所有包含 "ERROR" 的行。

---

### **網絡與下載工具**

<br>

#### 18. wget  
`wget` 用來從網絡下載文件。在 Dockerfile 中，經常使用 `wget` 來下載軟體包或配置文件。

```bash
wget https://example.com/file.tar.gz
```
這行指令會從指定的 URL 下載 `file.tar.gz` 文件。

#### 19. curl  
`curl` 是另一個從網絡傳輸數據的工具，比 `wget` 功能更豐富，並且可以用來發送 HTTP 請求。在 Dockerfile 中經常用來檢查 API 或下載文件。

```bash
curl -O https://example.com/file.zip
```
這行指令會下載 `file.zip` 並保存到當前目錄。

---

日後隨時補充