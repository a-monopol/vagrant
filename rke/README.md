# RancherOS Kubernetes 環境

## 前提

vagrant、rke、 kubectl がインストール済みな事。

## quick start.

シングルノードのクラスタを構成します。

### 1. rancherOS の起動
```
[macos]
$ vagrant up
```

### 2. rke にて kubenetes クラスタを構成
```
[macos]
$ rke up
```

### 3. 生成された kube_config_cluster.yml を コピー
kubectl の利用に必要です。
```
[macos]
$ mv ~/.kube/config ~/.kube/config.bk.$(date +%s)
$ cp kube_config_cluster.yml ~/.kube/config
```

### 4. node の動作状況を確認
STATUS が Ready になっている事を確認。
```
[macos]
$ kubectl get nodes
  NAME            STATUS    ROLES                      AGE       VERSION
  192.168.33.10   Ready     controlplane,etcd,worker   10m       v1.13.5
```

### 5. サンプルの Deployment で動作を確認
以下のように deployment と service が生成されている事を確認する。
```
[macos]
$ kubectl apply -f k8s/sample.yml
  deployment.apps "nginx-deployment" created
  service "nginx" created

$ kubectl get deploy
  NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
  nginx-deployment   1         1         1            1           24s

$ kubectl get svc
  NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
  kubernetes   ClusterIP   10.43.0.1      <none>        443/TCP        16m
  nginx        NodePort    10.43.145.80   <none>        80:30080/TCP   51s
```

### 6. ブラウザでも確認
ブラウザで nginx のスタートページが表示されれば OK。
```
$ open http://192.168.33.10:30080
```

### 7. 後片付け
```
$ vagrant destory
$ mv ~/.kube/{3で生成されたバックアップ} ~/.kube/config 
```

## 備考
- rancherOS の手頃な（公式？は古い） vagrant box ファイルがない。
	- とりあえず version の新しい chrisurwin/RancherOS を利用。
  - ( https://app.vagrantup.com/chrisurwin/boxes/RancherOS/versions/1.5.1 )

## refs
> rke Example Cluster.ymls.  
> https://rancher.com/docs/rke/latest/en/example-yamls/

> rancherOS timezone.
> http://oshiire.to/archives/4998

> rancherOS config export
> https://rancher.com/docs/os/v1.2/en/configuration/

