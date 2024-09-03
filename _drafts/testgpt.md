想要問一下我這樣設置算成功用本地電腦輸入SQL Command嗎?

執行時總共有 3 個 terminal:

1. Ubantu SSH container  ( PORTS ：.0.0.0:5566->22/tcp)  我在裡面下載 mysql 把內部 ports 設定成 3006 並開啟Mysql
2. SSH隧道用：第二個從本地執行``ssh -L 3307:localhost:3306 user1@localhost -p 5566`` -> 變成隧道後保持開啟
  3. 本地執行：mysql -h 127.0.0.1 -P 3307 -u root -p -> 成功進入也可以下指令
/Users/flea0085/Desktop/Programming/docker file/my-docker-app


while true; do 
  echo -e "HTTP/1.1 200 OK\n\n $(mysql -u root -p0000 -h 127.0.0.1 -P 3307 -e "SHOW DATABASES;")" | nc -l 3308; 
done


while true; do 
  response=$(mysql -u root -p -h 127.0.0.1 -P 3307 -e "SHOW DATABASES;")
  echo -e "HTTP/1.1 200 OK\n\n$response" | nc -l -p 3308; 
done


while true; do 
  response=$(mysql -u root -p0000 -h 127.0.0.1 -P 3307 -e "SHOW DATABASES;")
  echo -e "HTTP/1.1 200 OK\n\n$response" | nc -l -p 3308; 
done



nc -l 3308 | while read line; do
    if [[ $line == *"key1="* ]]; then
        key1=$(echo $line | grep -oP 'key1=\K[^&]*')
        key2=$(echo $line | grep -oP 'key2=\K[^&]*')

        # 使用 mysql 命令行工具將數據插入 MySQL
        mysql -h 127.0.0.1 -P 3307 -u root -pYourPassword -e "INSERT INTO your_table (key1, key2) VALUES ('$key1', '$key2');"
    fi
done


ssh -i ~/.ssh/id_rsa_github -L 3307:localhost:3306 user1@localhost -p 5566



本地測試 my sql -> ok