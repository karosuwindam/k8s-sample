# k8s-sample

本リポジトリはRaspberry Pi用のkubernetesを動かすためのyamlファイル集になります。


control planの初期

```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket /var/run/containerd/containerd.sock --token-ttl 0
```

## flunnelの導入

```
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```


## Caricoの導入

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.29.2/manifests/tigera-operator.yaml
curl https://raw.githubusercontent.com/projectcalico/calico/v3.29.2/manifests/custom-resources.yaml -O

# custom-resources.yamlのcidr: 10.244.0.0/16 に修正
kubectl create -f custom-resources.yaml
kubectl get pods -n calico-system
```


## metaldbによるサービスを導入
以下のコマンドでmetallbを導入する
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml
```

## woerkerセットアップ

コントロールプレインの構築後は、その構築完了時に表示される登録コマンドを各ワーカー対象のノードで実行することで、登録することが可能です。

また、そのコマンドを再度調べる際には以下のコマンドを実行してください。

```
echo sudo kubeadm join $(hostname -I|awk '{print $1}'):6443 \
 --token $(kubeadm token list |sed -n 2P|awk '{print $1}') \
 --discovery-token-ca-cert-hash sha256:$(openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | \
  openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //')
```

デフォルトだとRoleの部分がnone表示のため以下のコマンドでworkerに変更する
```
kubectl label node/k8s-master-01 node-role.kubernetes.io/master=k8s-master-01
kubectl label node/k8s-worker-01 node-role.kubernetes.io/worker=k8s-worker-01
kubectl label node/k8s-worker-02 node-role.kubernetes.io/worker=k8s-worker-02
kubectl label node/k8s-worker-03 node-role.kubernetes.io/worker=k8s-worker-03
```
kubectl label nodes --all node.kubernetes.io/exclude-from-external-load-balancers-

ロードバランサーでIPの外部発信をゆうこうにする場合
kubectl label nodes --all node.kubernetes.io/exclude-from-external-load-balancers=true

kubectl get configmap kube-proxy -n kube-system -o yaml | \
sed -e "s/strictARP: false/strictARP: true/" | \
kubectl apply -f - -n kube-system

参考
https://github.com/metallb/metallb/issues/1765
https://github.com/metallb/metallb/issues/2676
https://sigridjin.medium.com/kubernetes-deep-dive-loadbalancer-services-and-metallb-6a09d58fcdf0

### configmap
kubectl create -n kube-system configmap metricbeat-cm --from-file=metricbeat.yml

