================================
uBuntu 使用nmcli對Wi-Fi做控制 
================================

1. 安裝NetworkManager
--------------------------
NetworkManager可用於以下連接類型：以太網、VLAN、網橋、綁定、成組、Wi-Fi、移動寬帶（比如移動網絡3G）及IP-over-InfiniBand。任何網路控制都會用到NetworkManager。

.. code-block:: bash

    sudo apt-get install network-manager

2. nmcli各種使用與法
--------------------------
nmcli 是一個允許用戶及腳本與NetworkManager互動的命令行工具。

2.1 顯示已知的網路設備狀態
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli d

2.2 顯示每個網卡(NetworkManager)連線狀態與UUID等資訊
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
命令"c"用於連接，是"connections"的縮寫形式。

.. code-block:: bash

    nmcli c

2.3 確保WiFi已開啟
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli radio wifi on

2.4 顯示所有已連接設備名稱與UUID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli connection show

2.5 顯示當前連接的Wi-Fi設備名稱、UUID等資訊
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli connection show --active

2.6 啟動/停止/連線/斷線
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli connection down connection-name
    nmcli connection up connection-name
    nmcli device disconnect interface-name
    nmcli device connect interface-name

2.7 連線至指定的Wi-Fi網路
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo nmcli d wifi connect 'Wi-Fi名稱' password '密碼'

2.8 指定網卡中斷斷線
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo nmcli dev disconnect iface 'interface-name'

2.9 顯示出當前已連線過的Wi-Fi網路UUID

.. code-block:: bash

    nmcli connection | cut -c 20-55

2.10 清除已連線過的Wi-Fi網路
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    nmcli connection delete uuid 'Wi-Fi-UUID'

環境設定：nmcli指令呼叫，無須輸入密碼
-----------------------------------------
若透過一些腳本或是程式，需要使用nmcli指令，但又不想每次都要輸入密碼，可以透過以下方式設定。

1. 將您的用戶添加到netdev組中：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo usermod -a -G netdev $USER

2. 設置sudoers文件以允許netdev組的成員使用nmcli命令而無需輸入密碼：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo visudo

在文件中添加以下行：

.. code-block:: bash

    %netdev ALL=(root) NOPASSWD: /usr/bin/nmcli


參考資料
--------------------------
* `網絡管理之命令行工具nmcli <https://www.cnblogs.com/hokori/p/14265509.html>`_
