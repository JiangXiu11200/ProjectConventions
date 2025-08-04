==================================================================
pause 版本錯誤，container.service 報錯 (:6443 connection refused)
==================================================================

前言
------

:6443 Connnection refused 是常見且惱人的錯誤，其問題常見於 kubelet 啟動失敗、pause 容器異常、或 CA 證書過期、設備時間不一致等情況。
這些問題會導致 Kubernetes 集群無法正常運作，特別是 kubelet 無法連接到 API Server。


解決方法
----------------

1. 查看 container.service logs

.. code-block:: bash

    journalctl -u containerd.service --since now -f

2. 確認是否有image相關錯誤

.. image:: /assets/Kubernetes/pause_version_error/pause-1.png

.. code-block:: bash

    十二 24 10:40:30 ai02 containerd[958]: time="2024-12-24T10:40:30.877610112+08:00" level=error msg="RunPodSandbox for &PodSandboxMetadata{Name:kube-controller-manager-ai02,Uid:4b006fa3e864a372ea31fd0d928332f5,Namespace:kube-system,Attempt:50,} failed, error" error="failed to get sandbox image \"registry.k8s.io/pause:3.6\": failed to pull image \"registry.k8s.io/pause:3.6\": failed to pull and unpack image \"registry.k8s.io/pause:3.6\": failed to resolve reference \"registry.k8s.io/pause:3.6\": failed to do request: Head \"https://registry.k8s.io/v2/pause/manifests/3.6\": dial tcp: lookup registry.k8s.io: Temporary failure in name resolution"

4. 上傳所缺少的pause:3.6

.. code-block:: bash

    ai02@ai02:~/Downloads$ sudo ctr -n k8s.io i import pause_v3.6.tar.gz 

5. 查看是否上傳成功

.. code-block:: bash

    ai02@ai02:~/Downloads$ sudo ctr -n k8s.io image ls | grep pause

等待containerd.service 與kubelet 自動重新載入即可

6. 延伸探討

也許可以直接修改container.service 預設pause的版本，因為ctr -n k8s.io中本來就有pause:3.9，以下是chat GPT的參考


.. image:: /assets/Kubernetes/pause_version_error/pause-2.png

.. image:: /assets/Kubernetes/pause_version_error/pause-3.png