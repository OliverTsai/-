# -
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

首先先安裝必要的軟體
