==========================================
PVE 建立內部NAT網路
==========================================

準備工作
---------

* 本教學為: 如何利用路由規則的設定，建構ProxmoxVE環境中的NAT網路。
* Proxmox PVE Server  (須設定好網路，使其可以連接到Internet)
* 測試用PC (用以連接至PVE)

建立NAT介面卡
----------------

.. image:: /assets/ProxmoxVE/nat/nat-1.png

1. 登入PVE
2. 選擇你的PVE Server
3. Network
4. Create
5. 選Linux Bridge

.. image:: /assets/ProxmoxVE/nat/nat-2.png

6. 輸入NAT IP
7. 建立完畢後點選Create
8. Apply Configuration

.. image:: /assets/ProxmoxVE/nat/nat-3.png

9. Shell
10. cd /etc/network
11. nano interfaces

網路配置與路由規則設定
------------------------

.. image:: /assets/ProxmoxVE/nat/nat-4.png

12. 修改剛剛建立的介面卡並新增路由規則
13. 存檔離開

*備註: 設定細節供參考*

.. code-block:: bash

    auto vmbr5
    iface vmbr5 inet static
            address 192.168.100.1
            netmask 255.255.255.0
            bridge-ports none
            bridge-stp off
            bridge-fd 0

    # NAT configuration
    post-up echo 1 > /proc/sys/net/ipv4/ip_forward
    post-up iptables -t nat -A POSTROUTING -s '192.168.100.0/24' -o vmbr0 -j MASQUERADE
    post-down iptables -t nat -D POSTROUTING -s '192.168.100.0/24' -o vmbr0 -j MASQUERADE

連接到NAT網路
----------------

1. Create一台NAT-VM
2. 初期，先將NAT-VM的網路橋接到可以連接到實體網路的介面卡 (vmbr0)
3. 進行update & upgrade，並安裝所需工具

.. code-block:: bash

    sudo apt update
    sudo apt upgrade
    sudo apt install net-tools

4. 更新完畢後，**將原先的vmbr0 Remove，並Add上一個章節設定好的NAT 介面卡**

.. image:: /assets/ProxmoxVE/nat/nat-5.png

5. 網路設定完畢後，設定NAT-VM的netplan。

.. image:: /assets/ProxmoxVE/nat/nat-6.png

6. 設定完畢後，將netplan apply

.. code-block:: bash

    sudo netplan apply

7. reboot NAT-VM，設定完成

.. image:: /assets/ProxmoxVE/nat/nat-7.png

9. 檢查網路設定，此時介面卡是可以連接到Internet的

.. image:: /assets/ProxmoxVE/nat/nat-8.png

