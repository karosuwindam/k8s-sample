wget https://raw.githubusercontent.com/kubernetes-csi/csi-driver-smb/refs/heads/master/deploy/v1.17.0/rbac-csi-smb.yaml
wget https://raw.githubusercontent.com/kubernetes-csi/csi-driver-smb/refs/heads/master/deploy/v1.17.0/csi-smb-driver.yaml
wget https://raw.githubusercontent.com/kubernetes-csi/csi-driver-smb/refs/heads/master/deploy/v1.17.0/csi-smb-controller.yaml
wget https://raw.githubusercontent.com/kubernetes-csi/csi-driver-smb/refs/heads/master/deploy/v1.17.0/csi-smb-node.yaml



kubectl apply -f ./rbac-csi-smb.yaml
kubectl apply -f ./csi-smb-driver.yaml
kubectl apply -f ./csi-smb-controller.yaml
kubectl apply -f ./csi-smb-node.yaml

## 使い方
以下のコマンドで、パスワードを登録する
```
kubectl -n default create secret generic smbcreds --from-literal username=USERNAME --from-literal password="PASSWORD"
kubectl -n default create secret generic k8s-1-smb --from-literal username=USERNAME --from-literal password="PASSWORD"
```
