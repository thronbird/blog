---
title: K8S
date: 2020-09-01 11:11:32
tags:
---

###		IP

流量走向：Node ip(物理网卡ip) -> Service IP -> Pod IP（service通过selector动态负载pod流量）

如下： 10.100.8.249  是正常服务的pod 那么service对应的endpoints端口只会负载10.100.8.249的ip地址

```
➜  ~ k get service | grep lll
lll-testvpc-svc                             ClusterIP   10.101.168.128   <none>        8090/TCP,8888/TCP,7788/TCP                     90d

➜  ~ k get pod -owide| grep lll
lll-testvpc-deployment-54db5958d7-d695s                           2/2     Running   0          7d18h   10.100.8.249   k8s-node21122test   <none>           <none>
lll-testvpc-deployment-6595789c67-6k52d                           1/2     Running   3          18m     10.100.3.197   k8s-node21114test   <none>

➜  ~ k get endpoints | grep lll
lll-testvpc-svc                             10.100.8.249:8090,10.100.8.249:7788,10.100.8.249:8888    90d
```

