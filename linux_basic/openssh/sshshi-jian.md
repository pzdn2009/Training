# SSH 实践

- 安装openssh

```
sudo apt-get install openssh-client
sudo apt-get install openssh-server

yum install openssh-server openssh-clients
```

- 无选项参数运行 SSH

```
ssh 172.17.0.5 #无参数链接

The authenticity of host '172.17.0.5 (172.17.0.5)' can't be established.
ECDSA key fingerprint is 4a:43:df:74:59:ee:d5:37:0a:dd:17:01:66:d4:67:1c.
Are you sure you want to continue connecting (yes/no)?yes
# 第一次连接主机时，需要确定主机的真实性，输入yes
pzdn@172.17.0.5's password:
# 输入密码。
```
解释：
>第一次登陆后，ssh就会把登陆的ssh指纹存放在用户home目录的.ssh目录的know_hosts文件中，如果远程系统重装过系统，ssh指纹已经改变，你需要把 .ssh 目录下的know_hosts中的相应指纹删除，再登陆回答yes，方可登陆。这个目录的权限必须是700,并且用户的home目录也不能给其他用户写权限，否则ssh服务器会拒绝登陆。如果发生不能登陆的问题，请察看服务器上的日志文件/var/log/secure。通常能很快找到不能登陆的原因。

- 指定登录用户

默认的，ssh 会尝试用当前用户作为用户名来连接。

指定用户名的，可以使用 -l 选项参数。

```
ssh -l root 172.17.0.4
```

- 指定端口

```
ssh -l root 172.17.0.4 -p 22
```

- 指定压缩传输

```
ssh -C 192.168.0.103
```

- 指定加密算

 /etc/ssh/ssh_config or ~/.ssh/config文件中加入：
```
Cipher blowfish #默认是3des
```

- 调试模式

```
ssh -l root -v 172.17.0.4
```

连接到docker容器的结果打印：

```
OpenSSH_6.6.1, OpenSSL 1.0.1f 6 Jan 2014
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to 172.17.0.4 [172.17.0.4] port 22.
debug1: Connection established.
debug1: identity file /home/pzdn/.ssh/id_rsa type 1
debug1: identity file /home/pzdn/.ssh/id_rsa-cert type -1
debug1: identity file /home/pzdn/.ssh/id_dsa type 2
debug1: identity file /home/pzdn/.ssh/id_dsa-cert type -1
debug1: identity file /home/pzdn/.ssh/id_ecdsa type -1
debug1: identity file /home/pzdn/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/pzdn/.ssh/id_ed25519 type -1
debug1: identity file /home/pzdn/.ssh/id_ed25519-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.8
debug1: Remote protocol version 2.0, remote software version OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.8
debug1: match: OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.8 pat OpenSSH_6.6.1* compat 0x04000000
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: server->client aes128-ctr hmac-md5-etm@openssh.com none
debug1: kex: client->server aes128-ctr hmac-md5-etm@openssh.com none
debug1: sending SSH2_MSG_KEX_ECDH_INIT
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: ECDSA 4a:43:df:74:59:ee:d5:37:0a:dd:17:01:66:d4:67:1c
debug1: Host '172.17.0.4' is known and matches the ECDSA host key.
debug1: Found key in /home/pzdn/.ssh/known_hosts:7
debug1: ssh_ecdsa_verify: signature correct
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: SSH2_MSG_SERVICE_REQUEST sent
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey,password
debug1: Next authentication method: publickey
debug1: Offering RSA public key: /home/pzdn/.ssh/id_rsa
debug1: Server accepts key: pkalg ssh-rsa blen 279
debug1: key_parse_private2: missing begin marker
debug1: read PEM private key done: type RSA
debug1: Authentication succeeded (publickey).
Authenticated to 172.17.0.4 ([172.17.0.4]:22).
debug1: channel 0: new [client-session]
debug1: Requesting no-more-sessions@openssh.com
debug1: Entering interactive session.
debug1: Sending environment.
debug1: Sending env LANG = en_US.UTF-8
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.16.0-23-generic x86_64)
```

- 绑定源地址

 针对多ip的主机
```
ssh -b 172.17.0.200 -l root 172.17.0.4
```
- 使用配置文件
 
```
ssh -F /home/pzdn/my_ssh_config 172.17.0.4
```
