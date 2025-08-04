=======================================================
K8s Credentials expired (:6443 connection refused)
=======================================================

前言
------

Kubernetes master 本身具有憑證機制，其用於保護 API Server 的通訊安全，且預設情況下，憑證的有效期限為一年。當憑證過期後，API Server 將無法接受來自 kubelet、kube-proxy 等元件的請求，導致出現 `:6443 connection refused` 的錯誤。
一旦出現該錯誤，會使得 API Server 無法正常運作，進而導致無法使用 kubectl 等工具進行操作，但 Kubernetes 集群本身仍然可以正常運作。

解決方法
----------------

1. 登入 Kubernetes master 節點。

2.  查看憑證時間：

.. code-block:: bash

    sudo kubeadm certs check-expiration

3. 重新生成憑證：

.. code-block:: bash

    sudo kubeadm certs renew all

指令

.. code-block:: bash

    ai02@ai02:~$ sudo kubeadm certs renew all
    [renew] Reading configuration from the cluster...
    [renew] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
    [renew] Error reading configuration from the Cluster. Falling back to default configuration

    certificate embedded in the kubeconfig file for the admin to use and for kubeadm itself renewed
    certificate for serving the Kubernetes API renewed
    certificate the apiserver uses to access etcd renewed
    certificate for the API server to connect to kubelet renewed
    certificate embedded in the kubeconfig file for the controller manager to use renewed
    certificate for liveness probes to healthcheck etcd renewed
    certificate for etcd nodes to communicate with each other renewed
    certificate for serving etcd renewed
    certificate for the front proxy client renewed
    certificate embedded in the kubeconfig file for the scheduler manager to use renewed

    Done renewing certificates. You must restart the kube-apiserver, kube-controller-manager, kube-scheduler and etcd, so that they can use the new certificates.

4. 生成新的配置文建 (更新配置文建內憑證)

.. code-block:: bash

    sudo kubeadm init phase kubeconfig admin

此時k8s目錄下會生成一個新的admin.conf

.. code-block:: bash

    ai02@ai02:~$ sudo mv /etc/kubernetes/admin.conf /etc/kubernetes/_admin.conf
    ai02@ai02:~$ sudo kubeadm init phase kubeconfig admin
    W1108 11:30:12.402785  402480 version.go:104] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get "https://dl.k8s.io/release/stable-1.txt": dial tcp: lookup dl.k8s.io on 127.0.0.53:53: server misbehaving
    W1108 11:30:12.402865  402480 version.go:105] falling back to the local client version: v1.26.3
    [kubeconfig] Writing "admin.conf" kubeconfig file
    ai02@ai02:~$ ls /etc/kubernetes/
    _admin.conf  admin.conf  controller-manager.conf  kubelet.conf  manifests  pki  scheduler.conf  secrets-store-csi-providers

複製到根目錄

.. code-block:: bash

    ai02@ai02:~$ sudo cp /etc/kubernetes/admin.conf ~/.kube/config 