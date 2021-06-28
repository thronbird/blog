---
title: command_daily
date: 2020-06-17 09:39:55
tags:
---

# VIM

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

`di"` - delete inside `"`
`dap` - delete around paragraph



# GIT

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

commit合并之后历史太混乱： 从master重新拉一个新分支，然后 git merge --squash master

设置代理：
git config --global https.proxy http://127.0.0.1:9999
git config --global http.proxy http://127.0.0.1:9999
```



# MAVEN

```xml
查看setting.xml文件位置
mvn help:effective-settings
```



# VS_CODE

-  ctrl+ ` 打开终端

- code . 在当前位置打开vscode（Command Palette (Ctrl+Shift+P) and type 'shell command' ）

- command + p navigate files

- ctrl+t  查找 symbol

- command + k  zen mode

- alt+option+f 格式化代码

  


# Shell

RSYNC_PASSWORD="xxxxx" rsync -az watchdog.prof xxx@47.96.187.82::xxxxxx

 ss -an | grep 192.168.7.8 | grep -i time-wait | wc -l

nginx -s reload

journalctl -u  xxxx  -fn 500

lsof | grep default.log 查询正在读写default.log的进程

ll proc/{pid}/fd

for i in *;do mv $i xxx${i}xxx ;done 批量重命名

sudo passwd root 修改密码

sudo passwd -d root 删除密码

du -h --max-depth=1 查看当前目录占用磁盘大小



# Network

iptables -t nat -nvL

route -n

tcpdump -i ethx -host xxx.xxx.xxx.xxx 

conntrack -L

netstat -tunlp



# JAVA

./jmap -F -dump:format=b,file=monitor.hprof 23893

./jcmd 14091 help VM.native_memory summary

./jstat -gcutil 20971

./jstat -gccapacity 20971



# Kafka

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



# DOCKER

**Docker: remove all Exited containers**

sudo docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm

sudo docker rmi $(sudo docker images -f "dangling=true" -q)

 <https://stackoverflow.com/questions/28377446/how-to-physically-remove-untagged-docker-images>  

docker image prune -a

 <https://stackoverflow.com/questions/54480752/error-response-from-daemon-conflict-unable-to-delete-2602b4852593-cannot-be-f> 



# K8S

- 根据标签清理过期deployment

k get deployment --show-labels |grep s-microfe-first | cut -d ' ' -f 1 |xargs kubectl delete deployment

 k get deployment | grep lll- |cut -d " " -f 1 |xargs kubectl delete deployment



- 查询所有容器内进程的Xmx配置

 kubectl get pods -n local-test |awk '{if (NR> 1) print $1}' |xargs -n 1 -I {} kubectl -n local-test exec {} -it -- ps -ef|grep -i -oP XmX[0-9][0-9][0-9]m

- Node亲和性

kubectl taint node k8s-node21121test node.kubernetes.io/memory-pressure=29491:NoExecute-

- 查看容器Xmx配置

kubectl get pods -n local-test |awk '{if (NR> 1) print $1}' |xargs -n 1 -I {} kubectl -n local-test exec {} -it -- ps -ef|grep -i -oP XmX[0-9][0-9][0-9]m

- kubectl get pod nginx-ingress-controller-548755f5d4-hs7kx  -A -o wide -o json

# Docker

Command to remove all unused images:

- docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")

  

Remove all containers|images:

- docker rm $(docker ps -aq) 
- docker rmi $(docker images -q)



# Prometheus

启动：./prometheus --config.file=prometheus.yml --web.enable-lifecycle

重载： curl -X POST http://localhost:9090/-/reload



指标查询：

- 应用cpu指标

sum(node_namespace_pod_container:container_cpu_usage_seconds_total:sum_rate{namespace="ali-sml"}) by (container) / sum(kube_pod_container_resource_limits_cpu_cores{namespace="ali-sml"}) by (container)



# Brew 

ALL_PROXY=socks5://127.0.0.1:10000 brew upgrade

brew services restart grafana



# Mysql 

服务启动|停止：

- mysql.server start  
- mysql.server stop

To install:
``` brew install mysql@5.6```

To have launchd start mysql@5.6 now and restart at login:
``` brew services start mysql@5.6``` 

Or, if you don't want/need a background service you can just run:
```/usr/local/opt/mysql@5.6/bin/mysql.server start```

登录mysql： mysql -uroot  -po0o0o0

设置|删除root用户密码：

SET PASSWORD FOR root@localhost='';

创建用户并授权：

CREATE USER 'eops'@'localhost' IDENTIFIED BY 'eops2019';
GRANT ALL PRIVILEGES ON *.* TO 'eops'@'localhost';

https://stackoverflow.com/questions/54935512/how-to-install-mysql-5-6-on-osx-mojave
https://flaviocopes.com/mysql-how-to-install/




# Arthas

- curl -O https://arthas.aliyun.com/arthas-boot.jar
  java -jar arthas-boot.jar
- trace xxx  '#cost > 10'




#MAC
- network-proxy : 192.168.0.0/16、10.0.0.0/8、172.16.0.0/12、127.0.0.1、localhost、*.local、*.esign.cn、*.timevale.cn、*.tsign.cn
- 清理dns缓存： sudo dscacheutil -flushcache  chrome://net-internals/#dns






# Terminal 

https://ss64.com/osx/syntax-bashkeyboard.html

## Moving the cursor:

```
  Ctrl + a   Go to the beginning of the line (Home)
  Ctrl + e   Go to the End of the line (End)
  Hold the Option key  and click on the current line = Jump Backwards

  Ctrl + p   Previous command (Up arrow)
  Ctrl + n   Next command (Down arrow)
  Hold the Option key  and click on a previous line = Jump upwards

  Ctrl + f   Forward one character
  Ctrl + b   Backward one character
   Alt + b   Back (left) one word      or use Option+Right-Arrow
   Alt + f   Forward (right) one word  or use Option+Left-Arrow

  Ctrl + xx  Toggle between the start of line and current cursor position
```

## Editing:

```
 Ctrl + L   Clear the Screen, similar to the clear command

  Alt + Del Delete the Word before the cursor.
  Alt + d   Delete the Word after the cursor.
 Ctrl + d   Delete character under the cursor
 Ctrl + h   Delete character before the cursor (backspace)

 Ctrl + w   Cut the Word before the cursor to the clipboard.
 Ctrl + k   Cut the Line after the cursor to the clipboard.
 Ctrl + u   Cut/delete the Line before the cursor position.

  Alt + t   Swap current word with previous
 Ctrl + t   Swap the last two characters before the cursor (typo).
 Esc  + t   Swap the last two words before the cursor.

 ctrl + y   Paste the last thing to be cut (yank)
  Alt + u   UPPER capitalize every character from the cursor to the end of the current word.
  Alt + l   Lower the case of every character from the cursor to the end of the current word.
  Alt + c   Capitalize the character under the cursor and move to the end of the word.
  Alt + r   Cancel the changes and put back the line as it was in the history (revert).
 ctrl + _   Undo
 
  TAB       Tab completion for file/directory names
```

> For example, to move to a directory 'sample1'; Type cd sam ; then press TAB and ENTER.
> type just enough characters to uniquely identify the directory you wish to open.

## Special keys: Tab, Backspace, Enter, Esc

> Text Terminals send characters (bytes), not key strokes.
> Special keys such as Tab, Backspace, Enter and Esc are encoded as control characters.
> Control characters are not printable, they display in the terminal as ^ and are intended to have an effect on applications.
>
> Ctrl+I = Tab
> Ctrl+J = Newline
> Ctrl+M = Enter
> Ctrl+[ = Escape
>
> Many terminals will also send control characters for keys in the digit row:
> Ctrl+2 → ^@
> Ctrl+3 → ^[ Escape
> Ctrl+4 → ^\
> Ctrl+5 → ^]
> Ctrl+6 → ^^
> Ctrl+7 → ^_ Undo
> Ctrl+8 → ^? Backward-delete-char
>
> Ctrl+v tells the terminal to not interpret the following character, so Ctrl+v Ctrl-I will display a tab character,
> similarly Ctrl+v ENTER will display the escape sequence for the Enter key: ^M

## History:

```
  Ctrl + r   Recall the last command including the specified character(s)
             searches the command history as you type.
             Equivalent to : vim ~/.bash_history. 
  Ctrl + p   Previous command in history (i.e. walk back through the command history)
  Ctrl + n   Next command in history (i.e. walk forward through the command history)

  Ctrl + s   Go back to the next most recent command.
             (beware to not execute it from a terminal because this will also launch its XOFF).
  Ctrl + o   Execute the command found via Ctrl+r or Ctrl+s
  Ctrl + g   Escape from history searching mode
        !!   Repeat last command
      !n     Repeat from the last command: args n e.g. !:2 for the second argumant.
      !n:m   Repeat from the last command: args from n to m. e.g. !:2-3 for the second and third.
      !n:$   Repeat from the last command: args n to the last argument.
      !n:p   Print last command starting with n
        !$   Last argument of previous command
   ALT + .   Last argument of previous command
        !*   All arguments of previous command
^abc­^­def   Run previous command, replacing abc with def
```

## Process control:

```
 Ctrl + C   Interrupt/Kill whatever you are running (SIGINT)
 Ctrl + l   Clear the screen
 Ctrl + s   Stop output to the screen (for long running verbose commands)
 Ctrl + q   Allow output to the screen (if previously stopped using command above)
 Ctrl + D   Send an EOF marker, unless disabled by an option, this will close the current shell (EXIT)
 Ctrl + Z   Send the signal SIGTSTP to the current task, which suspends it.
            To return to it later enter fg 'process name' (foreground).
```

To use the **Alt** Key Shortcuts in macOS - Open Terminal Preferences | Settings Tab | Keyboard | Tick "Use option as meta key"

## Custom Terminal Keyboard Shortcuts

> In addition to the standard system-wide and bash keyboard shortcuts, it is possible to add extra key shortcuts for the macOS terminal. These are defined under Terminal > Preferences > Profile > Keyboard
>
> To add the **Home** key ![up/left](command-daily/key_tl.gif) for Start of Line and the **End** Key ![down/right](command-daily/key_br.gif) for End of Line:
>
> - Press + to add a new entry
> - Select the keyboard shortcut
> - Set the Action to Send Text
> - Enter the escape sequence: \033[H (for Start of Line) or \033[F (for End of Line)

## Emacs mode vs Vi Mode

> All the above assume that bash is running in the default Emacs setting, if you prefer this can be switched to [Vi](https://ss64.com/vi.html) shortcuts instead.
>
> Set Vi Mode in bash:
>
> ```
> $ set -o vi
> ```
>
> Set Emacs Mode in bash:
>
> ```
> $ set -o emacs
> ```