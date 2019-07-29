
## 通信の流れ
192.168.33.10 -> 10.0.2.15   -> 10.42.0.5   -> 10.42.0.7 -> 10.42.0.6
(client)        (IPTABLES)   (svclb-traefik)   (traefik)    (nginx)
                               IPTABLES-NAT

## refs
```
svclb-traefik (L3 なので sourceIP が失われる、、)
https://github.com/rancher/klipper-lb
```
```
traefik
https://docs.traefik.io/configuration/logs/
```
```
traefik ingress
https://docs.traefik.io/configuration/backends/kubernetes/
```

```
debug network
https://kubernetes.io/docs/tutorials/services/source-ip/
```

https://qiita.com/omatsu32/items/20b337b96595b6320be3


## nginx ingress を入れてみる

https://kubernetes.github.io/ingress-nginx/deploy/

```
#1
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
```
```
#2
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/baremetal/service-nodeport.yaml
```