## Đánh label cho máy cần được tích hợp tính năng containerfreeze

**For containerd(hay dùng)**:

> kubectl label nodes minikube knative.dev/container-runtime=containerd

- Để check xem máy có dùng containerd hay không?

Ubuntu:

> sudo service containerd status

-> nếu có tồn tại thì máy đang dùng containerd

**For cri-o(version>=1.24.1) (chỉ dành cho OpenShift)**:

> kubectl label nodes minikube knative.dev/container-runtime=crio


## Cài đặt container-freezer

```
RUNTIME=containerd # RUNTIME=crio nếu máy dùng crio
RELEASE=v0.1.0
kubectl apply -f "https://github.com/knative-sandbox/container-freezer/releases/download/${RELEASE}/freezer-common.yaml"
kubectl apply -f "https://github.com/knative-sandbox/container-freezer/releases/download/${RELEASE}/freezer-${RUNTIME}.yaml"
```

## Bật conatiner-freezer

**Hiện tại phiên bản v.0.1.0 chỉ cho phép 1 máy bật được tính năng này. Để bật cho máy khác thì thay địa chỉ máy tương ứng**

> HOST_IP=192.168.1.x

> kubectl patch configmap/config-deployment -n knative-serving --type merge -p '{"data":{"concurrencyStateEndpoint":"http://$HOST_IP:9696"}}'