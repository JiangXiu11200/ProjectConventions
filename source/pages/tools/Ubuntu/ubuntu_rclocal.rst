=======================================
uBuntu rc.local 開機自動執行命令
=======================================

1. 建立/etc/rc.local
--------------------------

.. code-block:: bash

    sudo nano /etc/rc.local

原檔可能會有一些預設內容，主要在此把需要執行的指令寫上即可。

.. code-block:: bash

    #!/bin/sh -e
    #
    # rc.local
    #
    # This script is executed at the end of each multiuser runlevel.
    # Make sure that the script will 'exit 0' on success or any other
    # value on error.
    #
    # In order to enable or disable this script just change the execution
    # bits.
    #
    # By default this script does nothing.
    exec 1>/tmp/rc.local.log 2>&1  # send stdout and stderr from rc.local to a log file
    set -x                         # tell sh to display commands before execution

    exit 0

2. 啟動或停止rc.local.service
------------------------------
查看狀態

.. code-block:: bash

    systemctl status rc-local.service

啟動

.. code-block:: bash

    systemctl start rc-local.service

停止

.. code-block:: bash

    systemctl stop rc-local.service