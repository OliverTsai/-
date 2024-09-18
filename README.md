一個完整的網站
應該有以下幾種功能

1.整套的web服務：指的是網站或應用的整體架構，包括前端和後端服務。

2.分布式資料庫：資料庫系統分布在多個伺服器上，以提高效能和可靠性。

3.快取：用來加速數據存取的技術，通常儲存最近使用的資料。

4.虛擬化：通過虛擬機技術來分割伺服器資源，提高資源利用率和靈活性。

5.自動化部署：自動完成應用程式和服務的安裝與更新，簡化管理工作。

安裝虛擬機的方式
--------------------------------------------------------------------------------------------------------------------------------------------------

1.挑選一個 Linux ubuntu 映像檔
從 Ubuntu 官網下載一個映像檔，有很多版本可以做選擇，像是 22.04 LTS， LTS 的意思是 Long term support，其中16.04、18.04、20.04 都是比較穩定的版本。

2. 安裝 VirtualBox
從 VirtualBox 下載 .exe 檔並安裝，依照自己電腦的作業系統選擇下載檔

3. 啟動 VirtualBox 建立一台 Ubuntu 虛擬機
新增一台虛擬機
ISO image 填入剛剛下載的 Ubuntu iso path

![image](https://github.com/OliverTsai/memorandum/blob/main/img/1.png)


如果想要自己進行手動安裝就勾選 “Skip Unattended Installation”
設定 Username 和 Password

![image](https://github.com/OliverTsai/memorandum/blob/main/img/2.png)

配置硬體資源(記憶體和CPU)，記憶體是系統的短期資料儲存區，存放電腦正在使用中的資訊，以便快速地存取；那 CPU 是電腦的中央處理器，這裡可以選擇要幾顆 CPU ，CPU 越多顆電腦的計算效能越好。

![image](https://github.com/OliverTsai/memorandum/blob/main/img/3.png)

配置儲存空間容量，檔案存放的容量大小，像是。 Windows C槽或是D槽的 125GB 或是多少 GB ，這裡就是要設定可以存放的空間大小。

![image](https://github.com/OliverTsai/memorandum/blob/main/img/4.png)

完成圖如下

![image](https://github.com/OliverTsai/memorandum/blob/main/img/5.png)


--------------------------------------------------------------------------------------------------------------------------------------------------



佈置Linux ubuntu-22.04的方法
--------------------------------------------------------------------------------------------------------------------------------------------------

首先要先將你的名稱加入權限
當我們首次佈署一台基於Linux Ubuntu的伺服器時，經常需要安裝一些常用的套件，例如vim和net-tools等。然而，在這個過程中，你可能會遇到一個常見的問題，就是缺乏sudo權限，這可能導致你無法順利完成安裝。解決其問題，需將當前的使用者帳號添加至sudoers名單中，以獲得必要權限。
sudo是Linux系統中一個非常強大且重要的指令，它允許授予特定使用者或使用者組執行特定指令的權限，並且在操作上具有安全性。本文將帶領讀者，詳細了解如何將帳號添加至sudoers名單中。

![image](https://github.com/OliverTsai/memorandum/blob/main/img/6.png)

# 切換至root權限。

$ su

# 查看當前權限級別

$ whoami

# 安裝或將sudo更新至最新版本

$ apt install sudo

# 添加帳號至sudoers名單中

$usermod -aG sudo oliver

# 離開root模式

$ exit

# 重新啟動伺服器（Ubuntu）

$ reboot


有權限後就能安裝必要的軟體

# 以管理員身份更新你系統的軟體包列表

$ sudo apt update

# 安裝 Nginx

$ sudo apt install nginx

# 安裝完成後，可以使用以下命令啟動 Nginx

$ sudo systemctl start nginx

# 並且設置 Nginx 開機自啟：

$ sudo systemctl enable nginx

# 檢查 Nginx 是否已成功啟動

$ sudo systemctl status nginx

# 安裝Python

$ sudo apt install python3

# 安裝 pip

$ sudo apt install python3-pip


幫助回憶，總結一下接下來要做的事情：

不管是後端或是爬蟲都會設定一個進入點啟動的python檔
不同的專案需要建立不同的虛擬環境來安裝各個python專案所需的各種依賴
佈置好好不同的軟件後就要設定重開機的啟動檔
需要賦予腳本權限跟創建systemd的服務來完成重啟之後要啟動的腳本或軟件


# 安裝 venv（如果尚未安裝）

$ sudo apt install python3-venv

# 創建虛擬環境

$ python3 -m venv myenv

# 激活虛擬環境(可以測試看看能不能機活)

$ source myenv/bin/activate

![image](https://github.com/OliverTsai/memorandum/blob/main/img/7.png)

# 在對應的虛擬環境中安裝 Flask 等依賴

$ pip install Flask

# 創建腳本文件

$ nano start_script.sh

編寫腳本內容

#!/bin/bash
source myenv/bin/activate  # 替換為你的虛擬環境路徑
cd /path/to/your/project  # 替換為你的專案路徑
python your_script.py  # 替換為要執行的 Python 檔案

# 賦予執行權限（Linux）

$ chmod +x ~/start_script.sh

# 創建systemd服務文件

在 /etc/systemd/system/ 目錄下創建一個新的服務文件，例如 my_service.service

$ sudo nano /etc/systemd/system/my_service.service

# 編寫服務配置

[Unit]
Description=My Startup Script

[Service]
ExecStart=/home/user/start_script.sh

[Install]
WantedBy=multi-user.target

# 重新加載 systemd 配置

$ sudo systemctl daemon-reload

# 啟用服務

使用以下命令使服務在啟動時自動運行

$ sudo systemctl enable my_service.service

重啟系統測試
