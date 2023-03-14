## Sử dụng Flannel làm CNI cho Cluster

Để cài đặt thì theo đường dẫn sau:
[Flannel](https://gist.github.com/rkaramandi/44c7cea91501e735ea99e356e9ae7883)

## Bật RAM swap cho Kubernetes

**Sửa file conf**:

> sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

```
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
Environment="FEATURE_GATE=--feature-gates=NodeSwap=true"
# This is a file that "kubeadm init" and "kubeadm join" generates at runtime, populating the KUBELET_KUBEADM_ARGS variable dynamically
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
# This is a file that the user can use for overrides of the kubelet args as a last resort. Preferably, the user should use
# the .NodeRegistration.KubeletExtraArgs object in the configuration files instead. KUBELET_EXTRA_ARGS should be sourced from this file.
EnvironmentFile=-/etc/default/kubelet
ExecStart=
ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS $FEATURE_GATE
```

**Thêm vào file config.yaml(nếu tồn tại memorySwap thì sửa lại)**:

> sudo nano /var/lib/kubelet/config.yaml

```
memorySwap:
  swapBehavior: UnlimitedSwap
```
