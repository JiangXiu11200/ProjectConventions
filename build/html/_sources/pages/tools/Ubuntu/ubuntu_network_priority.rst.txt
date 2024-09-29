==========================================
uBuntu 網路連線優先級
==========================================

前言
--------------------------
當設備同時有兩張網卡，對內連接至攝影機或POE switch，對外連接到有線網路或4G/5G Router時，可以設定其預設網卡順序，使其不會抓錯或抓不到。
今天若設備有兩個RJ45，分別為eno1與enp6s0，前者接有線網路對外，後者接上攝影機。

1. 安裝ifmetric
--------------------------

.. code-block:: bash

    sudo apt-get install ifmetric

2. 查看Routing table
--------------------------

使用route -n命令查看指標

.. code-block:: bash

    route -n

輸出如下，攝影機IP設定為192.168.226.X，對外網路則為192.168.1.X。

.. code-block:: bash

    user@ubuntu1804desktop:~$ route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         192.168.1.1     0.0.0.0         UG    600    0        0 eno1
    0.0.0.0         192.168.226.254 0.0.0.0         UG    100    0        0 enp6s0
    169.254.0.0     0.0.0.0         255.255.0.0     U     600    0        0 eno1
    192.168.1.0     0.0.0.0         255.255.255.0   U     600    0        0 eno1
    192.168.226.0   0.0.0.0         255.255.255.0   U     100    0        0 enp6s0

3. 設定路由順序
--------------------------

因為對外網路使用的網卡為eno1，故需要把eno1的指標降低，使其大於另一張網卡。

.. code-block:: bash

    sudo ifmetric eno1 100
    sudo ifmetric enp6s0 103

設定完畢如下

.. code-block:: bash

    user@ubuntu1804desktop:~$ route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0        0 eno1
    0.0.0.0         192.168.226.254 0.0.0.0         UG    103    0        0 enp6s0
    169.254.0.0     0.0.0.0         255.255.0.0     U     100    0        0 eno1
    192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 eno1
    192.168.226.0   0.0.0.0         255.255.255.0   U     103    0        0 enp6s0

現在，ubuntu會使用eno1進行連線，設定完畢即馬上生效。

4. 斷線自動重設權限
--------------------------

透過建立 check_network.sh 腳本，設定網卡順序後，始對外的網卡定期ping 8.8.8.8，確認連線，若斷線重插或關閉重啟，則自動設定網卡順序。

4.1 建立check_network 腳本
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo nano check_network.sh

.. code-block:: bash

    #!/bin/bash
    reconnectTimes=0
    while [ $reconnectTimes -lt 10 ]
        do
            if ping -q -c 1 -W 1 8.8.8.8;then
                echo "Network online"
                reconnectTimes=$((0))
                echo $reconnectTimes
            else
                sudo ifmetric enp2s0 103
                sudo ifmetric eno1 100
                echo "Network try reconnect"
                reconnectTimes=$(($reconnectTimes+1))
                echo $reconnectTimes
            fi
            sleep 1m
        done
    reboot
    exit 0

4.2 寫入rc.local，開機自動執行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo nano /etc/rc.local

.. code-block:: bash

    #!/bin/sh -e
    echo "hello" > /var/log/rclocal.log

    sudo bash /home/user/checkNetwork.sh & # 啟動指令

    exit 0

4.3 重啟rc-local.service
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo systemctl restart rc-local.service