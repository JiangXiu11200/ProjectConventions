==========================================
uBuntu Create service
==========================================

前言
------

透過建立ubuntu系統service，可以方便開機後始系統自動於背景中運行，系統中的各項service通常建立於: /etc/systemd/system/ 底下。


建立service file
--------------------------

1. 至目錄下建立service file

.. code-block:: bash

    cd /etc/systemd/system/
    sudo nano /etc/systemd/system/my_service.service

2. 編輯service file

內容可以參考參本文的參考資料撰寫。

.. code-block:: bash

    [Unit]
    Description=my_service_name service     # service 描述
    StartLimitIntervalSec=0                 # 取消對服務重啟的限制
    [Service]
    Type=simple                             # 定義啟動時的進程行為: simple/forking/oneshot/...
    User=user                               # 指定運行服務的用戶
    Group=user                              # 指定運行服務的用戶組
    WorkingDirectory=/home/user/motorVehiclesOffice  # 執行程式目錄
    ExecStart=/usr/bin/python3 manage.py runserver 0.0.0.0:8000 --insecure  # 運行命令，這邊的範例是啟動django web server
    Restart=always                          # 定義重啟狀況: 總是重啟
    KillSignal=SIGINT                       # 默認關閉process方式
    RestartSec=1                            # 重啟服務間隔
    [Install]
    WantedBy=multi-user.target              # 表示該服務所在的Target, Target表示一組服務

3. 啟動service

.. code-block:: bash

    sudo systemctl start my_service.service

4. 設定service開機啟動

如果要開機自動啟動，必須執行daemon-reload重新載入新建立的服務，或是如果有修改服務內容，也需執行。

.. code-block:: bash

    sudo systemctl daemon-reload
    sudo systemctl enable my_service.service

5. 查看service狀態

.. code-block:: bash

    sudo systemctl status my_service.service

6. 重新啟動service

.. code-block:: bash

    sudo systemctl restart my_service.service

7. 停止service

.. code-block:: bash

    sudo systemctl stop my_service.service

8. 刪除service

.. code-block:: bash

    sudo systemctl disable my_service.service
    sudo rm /etc/systemd/system/my_service.service
    sudo systemctl daemon-reload

參考資料
--------------------------

* `Linux Systemd 詳細介紹: Unit、Unit File、Systemctl、Target <https://www.3chy2.com.tw/3c%E8%B3%87%E8%A8%8A/linux-systemd-%E8%A9%B3%E7%B4%B0%E4%BB%8B%E7%B4%B9-unit%E3%80%81unit-file%E3%80%81systemctl%E3%80%81target/>`_