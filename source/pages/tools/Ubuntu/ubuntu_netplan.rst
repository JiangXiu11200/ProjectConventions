================================
uBuntu 使用Netplan對ubuntu網路卡做設定
================================

前言
--------------------------
當Ubuntu無法使用GUI設定網路時，可以利用netplan設定檔，從Command進行修改。

1. 進入netplan .yaml的設定檔
-----------------------------

.. code-block:: bash

    sudo nano /etc/netplan/50-cloud-init.yaml

2. 進入設定檔
-------------

原始生成的檔案，通常內部會預設DHCP。

.. code-block:: bash

    # This file is generated from information provided by the datasource.  Changes
    # to it will not persist across an instance reboot.  To disable cloud-init's
    # network configuration capabilities, write a file
    # /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
    # network: {config: disabled}
    network:
        ethernets:
            eth0:
                dhcp4: no
        version: 2

3. 修改設定檔
-------------

.. code-block:: bash

    # This file is generated from information provided by the datasource.  Changes
    # to it will not persist across an instance reboot.  To disable cloud-init's
    # network configuration capabilities, write a file
    # /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
    # network: {config: disabled}
    network:
        ethernets:
            eth0:
                dhcp4: off
                addresses: [192.168.1.189/24]
                gateway4: 192.168.1.1
                nameservers:
                    addresses: [8.8.8.8]
        renderer: networkd
        version: 2

4. 套用設定
-------------
執行以下指令，這時候通常會斷線個幾秒鐘，之後終端機上就會出現一個倒數計時，代表設定正確。這時候再按下 Enter 就會保存設定。
該用途用於，若設定錯誤，timeout後會自動恢復原本設定。

.. code-block:: bash

    sudo netplan try

5. 套用設定
-------------
執行以下指令，會強制設定，建議在確定設定無誤後再執行。

.. code-block:: bash

    sudo netplan apply