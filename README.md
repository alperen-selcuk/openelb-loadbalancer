# openelb-loadbalancer

with this repo you can install openElb on kubernetes cluster.

first install with one command 

```
kubectl apply -f https://raw.githubusercontent.com/openelb/openelb/master/deploy/openelb.yaml
```

easist method to get loadbalancer ip Layer2 mode.

first edit kube-proxy

```
kubectl edit configmap kube-proxy -n kube-system
```
do stric arp true

```
ipvs:
  strictARP: true
```

afther that restart kube-proxy **kubectl rollout restart daemonset kube-proxy -n kube-system**

specify interface for openElb masters nic IP

```
kubectl annotate nodes <master NAME> layer2.openelb.kubesphere.io/v1alpha1="<master nic IP>"
```

create eip object and specift IP range, for example 192.168.0.x network

```
apiVersion: network.kubesphere.io/v1alpha2
kind: Eip
metadata:
  name: layer2-eip
spec:
  address: 192.168.0.91-192.168.0.100
  interface: eth0
  protocol: layer2
```


