==========================================
uBuntu 掛載smb共享資料夾
==========================================

1.安裝所需工具
--------------------------

.. code-block:: bash

    sudo apt update
    sudo apt install samba
    sudo apt install smbclient

2.使用 smbclient 檢查可用的共享
-------------------------------

.. code-block:: bash

    smbclient -L //server -U username

3.掛載 SMB 共享
--------------------------

這裡的server和sharename 分別是你的SMB伺服器的地址和共享名稱。
username和password則是你的SMB帳戶的使用者名稱和密碼。

.. code-block:: bash

    sudo mkdir /mnt/myshare
    sudo mount -t cifs //server/sharename /mnt/myshare -o username=username,password=password

4. 設定smb server
--------------------------

1. 進入smb.conf

.. code-block:: bash

    sudo nano /etc/samba/smb.conf

2. 新增欄位

.. code-block:: bash

    [SMB_NAME]
    path = SMB_HOST_PATH
    valid user = USER_NAME
    read only = no

3. 將系統用戶新增為 Samba 用戶，並設定密碼：

.. code-block:: bash

    sudo smbpasswd -a user

4. restart smb.service

.. code-block:: bash

    sudo systemctl restart smbd.service

5. 自動掛載smb directory
-----------------------------

1. 進入/etc/fstab

.. code-block:: bash

    sudo nano /etc/fstab

2. 設定掛載目錄

.. code-block:: bash

    //SMB_SERVER_IP/DIRECTORY_NAME /HOST_MOUNT_DIRECTORY cifs username=USER_NAME,password=PASSWORD,uid=1000,gid=1000 0 0

//SMB_SERVER_IP/DIRECTORY_NAME：這是你要掛載的遠端 CIFS 檔案系統。
/HOST_MOUNT_DIRECTORY：這是你本地的掛載點。
cifs：檔案系統類型。
sername=USER_NAME,password=PASSWORD：掛載所需的使用者名稱和密碼。
uid=1000,gid=1000：設定檔案的擁有者和群組（可根據實際使用者 ID 和群組 ID 進行調整）。
0 0：
* 第一個 0 (dump 選項)：

    * 如果值為 0，則表示不需要備份檔案系統。
    * 如果值為 1，則表示需要備份檔案系統。一般來說，大多數現代 Linux 系統都不使用 dump，因此通常設定為 0。

* 第二個 0 (fsck 選項)：

    * 如果值為 0，則表示在系統啟動時不需要檢查這個檔案系統。
    * 如果值為 1，則表示需要在系統啟動時檢查這個檔案系統，且檢查的優先順序最高（通常用於根檔案系統）。
    * 如果值為 2，則表示需要在系統啟動時檢查這個檔案系統，但優先順序較低（通常用於非根檔案系統）。