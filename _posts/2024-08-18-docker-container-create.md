---
title: "Docker & Container"
date: 2024-08-18
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### Docker 是一個容器化平台，允許開發者將應用程式及其依賴項打包到可移植的container中，確保在不同環境下的運行一致性。它提供了輕量級的虛擬化，讓應用程式在同一台機器上彼此隔離地運行。<br>Container 是一種虛擬化技術，提供應用程式隔離的運行環境，包含所有運行所需的資源，確保應用程式能在任何環境中一致運行。

<!-- 正文 -->


### 初次創建 Docker container
**記得port接口設置超級重要，不然要全部重來!!!**

        #開啟docker
        open -a Docker

        docker info

        #下載image&創建docker
        docker pull "image"
        docker run -it "image"
        docker run -it --name "mycontainer" -p "8080:80" "image"
        (設定名稱、port接口、選擇image)

        docker ps

        #開始建立container環境(User&SSH)

        docker start "mycontainer"

        #進入容器的terminal
        docker exec -it "mycontainer" /bin/bash

        #設定新使用者
        adduser newuser
        usermod -aG sudo newuser

        #設定SSH連線
        apt update
        apt install openssh-server

        修改預設的文件資訊
        vim/etc/ssh/sshd_config
        確保以下設置已啟用：
        •PermitRootLogin yes
        •PasswordAuthentication yes

        service ssh restart

<br>

### 腳本創建 Docker container

1. 
  - [code by Mage 點這](https://lattice.posetmage.com/Tools/OS/Docker/RemoteUbuntu/)
  - 需準備三個檔案放入資料夾

#### 1. docker file
```bash
# Use the official Ubuntu image as the base
FROM ubuntu:latest

# Install SSH server and sudo
RUN apt-get update && apt-get install -y openssh-server sudo

# Set up the user and add to sudo group
ARG SSH_USERNAME
ARG SSH_PASSWORD
RUN useradd -m -s /bin/bash ${SSH_USERNAME} && \
    echo "${SSH_USERNAME}:${SSH_PASSWORD}" | chpasswd && \
    adduser ${SSH_USERNAME} sudo

# Set up SSH to accept connections
RUN mkdir /var/run/sshd
RUN echo 'PermitRootLogin no' >> /etc/ssh/sshd_config
RUN echo 'PasswordAuthentication no' >> /etc/ssh/sshd_config
RUN echo "AllowUsers ${SSH_USERNAME}" >> /etc/ssh/sshd_config
RUN mkdir -p /home/${SSH_USERNAME}/.ssh
RUN chown ${SSH_USERNAME}:${SSH_USERNAME} /home/${SSH_USERNAME}/.ssh
RUN chmod 700 /home/${SSH_USERNAME}/.ssh

COPY ./authorized_keys /home/${SSH_USERNAME}/.ssh/authorized_keys
RUN chown ${SSH_USERNAME}:${SSH_USERNAME} /home/${SSH_USERNAME}/.ssh/authorized_keys && \
    chmod 600 /home/${SSH_USERNAME}/.ssh/authorized_keys

# Copy the entry point script into the Docker image
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

# Make sure the script is executable
RUN chmod +x /usr/local/bin/entrypoint.sh

# Use the entry point script to start the SSH service and any other services
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Expose the SSH port
EXPOSE 22

# Default command (you can override this in `docker run`)
CMD ["/usr/sbin/sshd", "-D"]
```

#### 2. entrypoint.sh
```bash
#!/bin/bash

# Start SSH service
service ssh start

# Execute any additional commands or services
# e.g., start other services or run applications

# Keep the container running
exec "$@"
```

#### 3. authorized_keys(pub)
```bash
ssh key.pub
```

### 資料夾內執行

```bash
#依照dockerFile文本內的環境建立image，創建(自定義)名稱為"ubuntu-ssh"的image。  
docker build 
-f "dockerfile名稱" 
-t "image要設定的名稱" 
--build-arg "SSH_USERNAME=使用者名稱" 
--build-arg "SSH_PASSWORD=使用者密碼"

#用剛剛建立的image創建容器（不會自動執行）  
docker create -it 
-p 5566:22
-v "$(pwd)":/mnt/app #同步本地目錄與容器目錄 
-w /home/user1/app #進入容器內的預設值(user1)
--name "容器名稱" "ubuntu-ssh" /bin/bash

#運行容器  
docker start ubuntu-container

#SSH+私鑰key進入  
ssh -i ~/.ssh/"私鑰名稱" -p 5566 user1@127.0.0.1
```
