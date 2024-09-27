# 完整的網站應該有以下幾種功能

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

--------------------------------------------------------------------------------------------------------------------------------------------------


有權限後就能安裝必要的軟體

# 以管理員身份更新你系統的軟體包列表

$ sudo apt update

# 如果要用putty遠端操作(要自行安裝SSH)

$ sudo apt-get install openssh-server

# 設定ssh不會被防火牆阻擋(不用puyyt會被阻擋)

$ sudo ufw allow OpenSSH

允許 NGINX 處理所有流量（包括 HTTP 和 HTTPS）(還沒安裝先看下面)

$ sudo ufw allow 'Nginx Full'

允許某個端口(比如27017)

# 啟用防火牆

$ sudo ufw enable

# 檢查防火牆狀態

$ sudo ufw status

$ sudo ufw allow 27017/tcp

資安正式營運後的設置
------------------------------------------------------------

更改預設 SSH 連接埠(22=>你喜歡的port)

$ sudo nano /etc/ssh/sshd_config

找到以#Port 22開頭的行，並將其變更為所需的連接埠號碼。(記得把#拿掉)
port例如22689 22222 33366 12345 隨便你開心的號碼

設定防火牆(指定特定ip區段或單一ip可以連入到你設定的port取得ssh連線服務)

$ sudo ufw allow from 192.168.1.0/24 proto tcp to any port 22222

檢查一下規則有沒有寫進去

$ sudo ufw status (numbered 選項)

重新啟動ssh服務

$ sudo systemctl restart ssh

$ sudo systemctl status ssh 

------------------------------------------------------------

# 圖形介面可以安裝中文輸入法

安裝

$ sudo apt-get install ibus ibus-chewing

配置

$ im-config

之後重啟即可

------------------------------------------------------------

# 安裝 Nginx

$ sudo apt install nginx

# 安裝好後需要配置(每一個.com的檔都是一個網站或後端的配置)

$ sudo nano /etc/nginx/sites-available/yourdomain.com

配置靜態網頁
-----------------------------------------------------------------

server{
        listen 80;
        listen [::]:80;

        root /var/www/yourdomain.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name yourdomain.com www.yourdomain.com;

        location / {
                        try_files $uri $uri/ =404;
        }
}

這是標準靜態網頁配置，將yourdomain換成購買的網域即可

listen 80; 這行告訴NGINX在IPv4的端口80（HTTP的默認端口）上監聽請求。

listen [::]:80; 這行告訴NGINX在IPv6的端口80上監聽請求。[::] 是IPv6中表示所有地址的符號。

root /var/www/yourdomain.com/html; 這行指定了網站的根目錄，當用戶訪問這個伺服器的時候，NGINX會從這個目錄提供靜態文件。它表示該站點的HTML、CSS、JS等文件存儲位置。

index index.html index.htm index.nginx-debian.html; 這行定義了當用戶訪問網站根目錄（如 http://yourdomain.com/）時，NGINX應該加載的默認首頁文件。index.html 是優先加載的文件，如果找不到，則會嘗試 index.htm 和 index.nginx-debian.html。

server_name yourdomain.com www.yourdomain.com; 這行定義了這個虛擬主機所服務的域名。當用戶訪問 yourdomain.com 或 www.yourdomain.com 時，NGINX將使用這個 server 區塊內的配置來處理請求。

location / { ... } 這行定義了一個 location 區塊，用來匹配請求的路徑。在這裡，/ 代表網站的根路徑（即所有請求）。它定義了當請求匹配這個路徑時如何處理。

try_files $uri $uri/ =404; 這行是處理靜態文件請求的邏輯，當找不到文件時則返回404錯誤


配置後端的反向代理
-----------------------------------------------------------------

server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

這是後端的配置，將yourdomain換成購買的網域即可

proxy_pass http://127.0.0.1:5000; 這行指示NGINX將匹配的請求（轉發）到位於本地的Flask應用程序，即 http://127.0.0.1:5000。

proxy_set_header Host $host; 這行用來設置轉發的HTTP請求頭中的 Host 字段。$host 是NGINX變量，代表原始請求的主機名。

proxy_set_header X-Real-IP $remote_addr; 這行設定 X-Real-IP 請求頭，將原始用戶的IP地址傳遞給後端服務器。$remote_addr 是用戶的真實IP

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 這行設定 X-Forwarded-For 頭，它用來追蹤代理伺服器的請求鏈。

proxy_set_header X-Forwarded-Proto $scheme; 這行設定 X-Forwarded-Proto 頭，將原始請求使用的協議（http 或 https）傳遞給後端。


配置好後需要啟用站點設定檔

雖然站點設定檔在 /etc/nginx/sites-available/ 中，但要啟用它，你需要在 /etc/nginx/sites-enabled/ 中建立一個符號連結（symlink）

$ sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/

測試配置

每次修改NGINX設定檔後，都應該測試配置是否正確

$ sudo nginx -t


# 安裝佈置完成後，可以使用以下命令啟動 Nginx

$ sudo systemctl start nginx

# 並且設置 Nginx 開機自啟：

$ sudo systemctl enable nginx

# 檢查 Nginx 是否已成功啟動

$ sudo systemctl status nginx

--------------------------------------------------------------------------------------------------------------------------------------------------

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

# 編寫腳本內容
-------------
這邊有個要注意的地方，因為本篇是要透過systemd服務來執行sh啟動檔，所以要將PATH都打上絕對位置或設定一個工作目錄
-------------

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


# 設置連接虛擬機的方法
---------------------------------------------------------------------------------------------------
# 安裝vsftpd

$ sudo apt-get update
$ sudo apt-get install vsftpd

# 配置 vsftpd

$ sudo nano /etc/vsftpd.conf

# 確保以下行未被註解（前面沒有 #）

![image](https://github.com/OliverTsai/memorandum/blob/main/img/8.png)

listen=YES

anonymous_enable=NO

local_enable=YES

write_enable=YES


![image](https://github.com/OliverTsai/memorandum/blob/main/img/9.png)

# 重啟 vsftpd 服務
$ sudo systemctl restart vsftpd

# 將虛擬機的網絡模式設置為橋接模式

打開 VirtualBox，選擇您的虛擬機，然後點擊「設定」。
在「網絡」選項卡中，將「附加到」設置為「橋接適配器」。這樣可以讓虛擬機獲得與主機相同的網絡。

![image](https://github.com/OliverTsai/memorandum/blob/main/img/10.png)

# 查找虛擬機的 IP 地址

在 Linux 虛擬機中，使用以下命令查找其 IP 地址，之後將這個IP地址輸入到FileZilla 就可以

$ hostname -I

![image](https://github.com/OliverTsai/memorandum/blob/main/img/11.png)

在「主機」欄位中輸入剛才查找到的虛擬機 IP 地址。
在「用戶名」和「密碼」欄位中輸入您的 Linux 用戶名和密碼。
默認端口為 21（FTP），如果您有特別設置，可以根據需要修改。
點擊「快速連接」。

![image](https://github.com/OliverTsai/memorandum/blob/main/img/12.png)


連接成功後就可以把你寫的後端程式等等丟到測試機了


![image](https://github.com/OliverTsai/memorandum/blob/main/img/13.png)

# 啟動防火牆後的更新

要先設定防火牆能通過的

對於 FTP，執行以下命令

$ sudo ufw allow 21/tcp

對於 SFTP（如果使用 SSH）

$ sudo ufw allow 22/tcp

允許被動模式的 FTP（可選）： 如果您使用的是被動模式的 FTP，您可能需要允許額外的端口（例如 50000-51000）

$ sudo ufw allow 50000:51000/tcp

使用以下命令檢查防火牆規則是否已正確設置

$ sudo ufw status

![image](https://github.com/OliverTsai/memorandum/blob/main/img/26.png)

重啟服務

$ sudo systemctl daemon-reload

最後改成SFTP連線就可以了


設定FTTPS
---------------------------------------------------------------------------------------------------

生成 SSL 憑證(最後加上-nodes就是生成不用密碼的協議)

openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

![image](https://github.com/OliverTsai/memorandum/blob/main/img/27.png)

依序填入國家公司等等...

![image](https://github.com/OliverTsai/memorandum/blob/main/img/28.png)

就可以在程式中改成fttps連線了

另外這是看到其他大師分享的專業版

[點我](https://blog.miniasp.com/post/2019/02/25/Creating-Self-signed-Certificate-using-OpenSSL)

但用以上設置的自簽憑證會是不安全的連結，會被封鎖，可以使用Let's Encrypt取得免費的SSL證書，Let's Encrypt是免費且自動化的證書頒發機構，且支援Linux的Nginx

首先要安裝Certbot

$ sudo apt update
$ sudo apt install certbot python3-certbot-nginx

然後配置NGINX(資料在上面)

配置好後使用Certbot自動化申請SSL證書

$ sudo certbot --nginx -d yourdomain.com

會要你輸入聯絡信箱並且同意他們的請求

![image](https://github.com/OliverTsai/memorandum/blob/main/img/29.png)

然後會給你證書與私鑰的保存位置，記得到NGINX設定檔裡面添加(範例如下)

server {
    listen 443 ssl;
    server_name yourdomain.com www.yourdomain.com;
	
    # SSL 證書與私鑰的路徑
    ssl_certificate /path/to/your/certificate.crt;
    ssl_certificate_key /path/to/your/private.key;
	
    # SSL設定 (可根據需求調整)
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

成功運行你的網頁或後端後，測試NGINX配置並重新啟動NGINX，連線設定好的網址就會轉過去

---------------------------------------------------------------------------------------------------

# 設置資料庫

# 首先搜尋mongoDB的官網下載

![image](https://github.com/OliverTsai/memorandum/blob/main/img/14.png)

# 然後根據需求(此為Ubuntu22.04版本)下載

![image](https://github.com/OliverTsai/memorandum/blob/main/img/15.png)


# 然後我是透過FileZilla上傳

![image](https://github.com/OliverTsai/memorandum/blob/main/img/16.png)


# 輸入命令執行這個安裝檔案(此為mongodb-org-server_8.0.0_amd64.deb)

$ sudo dpkg -i mongodb-org-server_8.0.0_amd64.deb

![image](https://github.com/OliverTsai/memorandum/blob/main/img/17.png)

# 安裝完成後查看狀態

$ sudo systemctl status mongod


# 安裝正常後要啟動

$ sudo systemctl start mongod

![image](https://github.com/OliverTsai/memorandum/blob/main/img/18.png)


# 設定重開後啟動

$ sudo systemctl enable mongod


# 安裝指令驅動Mongo sh(也可以不安裝)

![image](https://github.com/OliverTsai/memorandum/blob/main/img/19.png)

![image](https://github.com/OliverTsai/memorandum/blob/main/img/20.png)

![image](https://github.com/OliverTsai/memorandum/blob/main/img/21.png)

$ sudo dpkg -i mongodb-mongosh_2.3.1_amd64.deb

# 進入mongoDB sh的指令

$ mongosh

![image](https://github.com/OliverTsai/memorandum/blob/main/img/22.png)

然後就可以在這邊輸入操作指令了

# 安裝圖示資料庫

![image](https://github.com/OliverTsai/memorandum/blob/main/img/23.png)

$ sudo dpkg -i mongodb-compass_1.44.4_amd64.deb

然後就可以用圖示來看資料庫

![image](https://github.com/OliverTsai/memorandum/blob/main/img/24.png)

![image](https://github.com/OliverTsai/memorandum/blob/main/img/25.png)

---------------------------------------------------------------------------------------------------

