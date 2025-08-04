==========================================
Kubernetes 運行環境中的四大核心元件
==========================================


元件說明
------

1. API Server

    - 這是 Kubernetes 的核心元件，負責處理 REST API 請求並與 etcd 資料庫進行溝通。
    - 它的設定檔通常位於：/etc/kubernetes/manifests/kube-apiserver.yaml （在 Kubernetes 管理節點上）

2. etcd

    - Kubernetes 使用 etcd 來存儲所有集群數據。這是一個一致性和高可用的鍵值存儲，主要用於存儲集群狀態數據。
    - etcd 的設定檔通常放置在：/etc/kubernetes/manifests/kube-apiserver.yaml（在 Kubernetes 管理節點上）

3. Controller Manager (kube-controller-manager)

    - 此元件負責處理 K8s 中的控制 (如複製、節點控制等)，以確保集群所需的狀態。
    - 設定檔路徑通常為：/etc/kubernetes/manifests/kube-controller-manager.yaml

4. Scheduler (kube-scheduler)

    - 負責將 Pod 指派到適當的節點上。
    - Scheduler 根據資源需求和策略決定最佳節點。
    - 它的配置檔案通常位於：/etc/kubernetes/manifests/kube-scheduler.yaml


基礎指令參考
------

1. 查看 Service logs

.. code-block:: bash

    journalctl -xeu kubelet.service -f

2. 檢查容器狀態

.. code-block:: bash

    sudo crictl ps -a

3. 查看容器日誌

查看所有容器的狀態

.. code-block:: bash

    sudo crictl ps -a

查看某個container的log

.. code-block:: bash

    sudo crictl logs `CONTAINER_ID`

