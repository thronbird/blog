---
title: command_daily
date: 2020-06-17 09:39:55
tags:
---

### VIM

https://www.worldtimzone.com/res/vi.html

0w移动到行首第一个单词

zz当前行居中显示

. 复制上一个修改操作 ct}xxxxxxx

~大小写转换 5~转换5个字符

10x 删除10个字符

d^删除到行首 

C 删除到行尾

I在行首插入

A在行尾插入

`diw` to delete in the word (doesn't include spaces)
`daw` to delete around the word (includes spaces before the next word).

### GIT

```
回滚版本：
git reflog
git reset --hard HEAD@{54}
git checkout HEAD@{57}

回退：
git reset --hard ORIG_HEAD

保存修改快照：
  |git stash push -m "my_stash" 
  |git stash save my_stash
  
->git stash list

->|  git stash pop stash@{index}
  |  git stash apply stash@{index}
  |  git stash apply stash^{/my_stash}

git diff > some.patch -> git apply some.patch

设置代理：
git config --global https.proxy http://127.0.0.1:9999
git config --global http.proxy http://127.0.0.1:9999
```



### MAVEN

```xml
查看setting.xml文件位置
mvn help:effective-settings
```



### VS_CODE

-  ctrl+ ` 打开终端

- code . 在当前位置打开vscode（Command Palette (Ctrl+Shift+P) and type 'shell command' ）

- command + p navigate files

- ctrl+t  查找 symbol

- command + k  zen mode

- alt+option+f 格式化代码

  


### Linux

RSYNC_PASSWORD="xxxxx" rsync -az watchdog.prof xxx@47.96.187.82::xxxxxx

 ss -an | grep 192.168.7.8 | grep -i time-wait | wc -l

nginx -s reload



 journalctl -u  xxxx  -fn 500



lsof | grep default.log 查询正在读写default.log的进程

ll proc/{pid}/fd



for i in *;do mv $i xxx${i}xxx ;done 批量重命名



sudo passwd root 修改密码

sudo passwd -d root 删除密码



### Network

iptables -t nat -nvL

route -n

tcpdump -i ethx -host xxx.xxx.xxx.xxx 

conntrack -L



### JAVA

./jmap -F -dump:format=b,file=monitor.hprof 23893

./jcmd 14091 help VM.native_memory summary

./jstat -gcutil 20971

./jstat -gccapacity 20971



### Kafka

测试环境 - vpc-test-bigdata-3    | 172.20.61.203

生产环境 -  vpc-prod-datanode01(172.28.57.4)

```
 sudo su - kafka 

 cd /usr/hdp/2.6.5.0-292/kafka/

// 消费
./bin/kafka-console-consumer.sh --bootstrap-server 172.20.61.203:6667 --topic esign-monitor-metric-source  | grep lll

./bin/kafka-console-consumer.sh --bootstrap-server 172.20.61.203:6667 --topic gateway-api-topic

./kafka-console-consumer.sh --bootstrap-server 172.28.57.0:6667,172.28.57.3:6667,172.28.57.4:6667 --topic billing.bs_stock_stats --from-beginning

//查看正在运行的消费组
./bin/kafka-consumer-groups.sh --bootstrap-server 172.20.61.203:6667 --list --new-consumer  

//计算消息的消息堆积情况
./bin/kafka-consumer-groups.sh --bootstrap-server 172.20.61.203:6667 --describe --group  gateway-traffic-consumer-group

---????
 ./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 16 --topic cat-agent-trace
 ./kafka-topics.sh --zookeeper h5:2181,h6:2181,h7:2181 --alter --partitions 300 --topic cat-agent-trace
  ./kafka-topics.sh --zookeeper h5:2181,h6:2181,h7:2181 --alter --partitions 16 --topic cat-agent-jvm


```



### DOCKER

**Docker: remove all Exited containers**

sudo docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm

sudo docker rmi $(sudo docker images -f "dangling=true" -q)

 <https://stackoverflow.com/questions/28377446/how-to-physically-remove-untagged-docker-images>  

docker image prune -a

 <https://stackoverflow.com/questions/54480752/error-response-from-daemon-conflict-unable-to-delete-2602b4852593-cannot-be-f> 



### K8S

- 根据标签清理过期deployment

k get deployment --show-labels |grep s-microfe-first | cut -d ' ' -f 1 |xargs kubectl delete deployment

 k get deployment | grep lll- |cut -d " " -f 1 |xargs kubectl delete deployment



- 查询所有容器内进程的Xmx配置

 kubectl get pods -n local-test |awk '{if (NR> 1) print $1}' |xargs -n 1 -I {} kubectl -n local-test exec {} -it -- ps -ef|grep -i -oP XmX[0-9][0-9][0-9]m

- Node亲和性

kubectl taint node k8s-node21121test node.kubernetes.io/memory-pressure=29491:NoExecute-

- 查看容器Xmx配置

kubectl get pods -n local-test |awk '{if (NR> 1) print $1}' |xargs -n 1 -I {} kubectl -n local-test exec {} -it -- ps -ef|grep -i -oP XmX[0-9][0-9][0-9]m



### Docker

Command to remove all unused images:

- docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")

  

Remove all containers|images:

- docker rm $(docker ps -aq) 
- docker rmi $(docker images -q)



### Prometheus

启动：./prometheus --config.file=prometheus.yml --web.enable-lifecycle

重载： curl -X POST http://localhost:9090/-/reload



指标查询：

- 应用cpu指标

sum(node_namespace_pod_container:container_cpu_usage_seconds_total:sum_rate{namespace="ali-sml"}) by (container) / sum(kube_pod_container_resource_limits_cpu_cores{namespace="ali-sml"}) by (container)



### brew 

 brew services restart grafana



### Arthas

curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
trace xxx xxx



### Arthas

curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar