# iptables

## iptables

## 1. 命令解析

**iptables/ip6tables** — administration tool for IPv4/IPv6 packet filtering and NAT.

**格式：**

```text
   iptables [-t 表] 命令选项 链名 匹配条件 [-j 动作]
   iptables [-t table] {-A|-C|-D} chain rule-specification
   ip6tables [-t table] {-A|-C|-D} chain rule-specification
   iptables [-t table] -I chain [rulenum] rule-specification
   iptables [-t table] -R chain rulenum rule-specification
   iptables [-t table] -D chain rulenum
   iptables [-t table] -S [chain [rulenum]]
   iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]
   iptables [-t table] -N chain
   iptables [-t table] -X [chain]
   iptables [-t table] -P chain target
   iptables [-t table] -E old-chain-name new-chain-name
   rule-specification = [matches...] [target]
   match = -m matchname [per-match-options]
   target = -j targetname [per-target-options]
```

**命令选项**

* -A【append】在指定的链的结尾添加规则
* -D【delete】删除指定链中的规则，可以按规则号或者规则内容来删除
* -I【insert】插入一条规则，默认是在最前面
* -R【replace】替换某一条规则
* -L【list】列出所有规则
* -F【flush】清空所有规则
* -N【new】自定义一条规则链
* -X【--delete-chain】删除用户自定义规则链
* -P【policy】设置默认策略
* -n【numberic】以数字方式显示
* -v【verbose】显示详细信息
* -V【version】查看iptables的版本信息

**链名**

* INPUT
* OUTPUT
* FORWARD

**-m matchname**

* -p udp
* -p icmp
* -s ipadress
* -d ipaddress
* --sport port
* --dport port
* -i eth0 \#接口匹配

**-j targetname**

* ACCEPT
* REJECT
* DROP

## 2. 应用

协议匹配，地址匹配，端口匹配，接口匹配，SNAT转换，DNAT转换，MAC地址匹配，数据包和速率

```text
iptables -I INPUT -p icmp -j REJECT #icmp协议的数据，拒绝进入
iptables -A FORWARD -p udp -j ACCEPT #udp协议的数据，允许转发

iptables -A FORWARD -s 10.0.0/8 -j DROP #拒绝转发来自10.0.0/8的数据
iptables -A FORWARD -d 10.0.0/8 -j DROP #拒绝转发目的地是10.0.0/8的数据

iptables -A FORWARD -s 10.0.0/8 -p tcp --dport 80 -j ACCEPT #允许转发来自10.0.0/8网段，目的端口是80的数据包
iptables -I FORWARD -s 10.0.0/8 -p tcp --sport 21 -j ACCEPT #允许转发来自10.0.0/8网段，源端口是21的数据包

iptables -A FORWARD -i eth0 -s 10.0.0.0/8 -p tcp --dport 80 -j ACCEPT #允许转发从eth0进入，来自10.0.0.0/8，使用tcp协议，目的端口是80的数据包
iptables -A INPUT -i eth0 -s 80.0.0.0/8 -j DROP #拒绝从eth0进入，来自80.0.0.0/8的数据包
```

eg:

```text
pzdn@ubuntu:~$ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
DROP       all  --  172.18.0.0/16        172.17.0.0/16       
DROP       all  --  172.17.0.0/16        172.18.0.0/16       
DROP       all  --  172.19.0.0/16        172.17.0.0/16       
DROP       all  --  172.17.0.0/16        172.19.0.0/16       
DOCKER     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            
DROP       all  --  172.19.0.0/16        172.18.0.0/16       
DROP       all  --  172.18.0.0/16        172.19.0.0/16       
DOCKER     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            
DOCKER     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         

Chain DOCKER (3 references)
target     prot opt source               destination
```

