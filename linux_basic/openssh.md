# 1.SSH简介

SSH全称Secure SHell。

SSH的主要目的是用来取代传统的telnet和R系列命令（rlogin,rsh,rexec等）远程登陆和远程执行命令的工具，实现对远程登陆和远程执行命令加密。防止由于网络监听而出现的密码泄漏，对系统构成威胁。

ssh协议目前有SSH1和SSH2，SSH2协议兼容SSH1。目前实现SSH1和SSH2协议的主要软件有OpenSSH和SSH Communications Security Corporation公司的SSH Communications 软件。前者是OpenBSD组织开发的一款免费的SSH软件，后者是商业软件，因此在linux、FreeBSD、OpenBSD、NetBSD等免费类UNIX系统种，通畅都使用OpenSSH作为SSH协议的实现软件。

_note_：需要注意的是OpenSSH和SSH Communications的登陆公钥/私钥的格式是不同的，如果想用SSH Communications产生的私钥/公钥对来登入到使用OpenSSH的linux系统需要对公钥/私钥进行格式转换。

在出现SSH之前，系统管理员需要登入远程服务器执行系统管理任务时，都是用telnet来实现的，telnet协议采用明文密码传送，在传送过程中对数据也不加密，很容易被不怀好意的人在网络上监听到密码。同样，在SSH工具出现之前R系列命令也很流行（由于这些命令都以字母r开头，故把这些命令合称为R系列命令R是remote的意思），比如rexec是用来执行远程服务器上的命令的，和telnet的区别是telnet需要先登陆远程服务器再实行相关的命令，而R系列命令可以把登陆和执行命令并登出系统的操作整合在一起。这样就不需要为在远程服务器上执行一个命令而特地登陆服务器了。

SSH是一种加密协议，不仅在登陆过程中对密码进行加密传送，而且对登陆后执行的命令的数据也进行加密，这样即使别人在网络上监听并截获了你的数据包，他也看不到其中的内容。

**OpenSSH软件包**包含以下命令：

http://www.openssh.com/manual.html

- ssh(1) - The basic rlogin/rsh-like client program
- sshd(8) - The daemon that permits you to log in
- ssh_config(5) - The client configuration file
- sshd_config(5) - The daemon configuration file
- ssh-agent(1) - An authentication agent that can store private keys
- ssh-add(1) - Tool which adds keys to in the above agent
- sftp(1) - FTP-like program that works over SSH1 and SSH2 protocol
- scp(1) - File copy program that acts like rcp
- ssh-keygen(1) - Key generation tool
- sftp-server(8) - SFTP server subsystem (started automatically by sshd)
- ssh-keyscan(1) - Utility for gathering public host keys from a number of hosts
- ssh-keysign(8) - Helper program for host-based authentication

# 2.ssh

http://man.openbsd.org/ssh

ssh ―― SSH协议的客户端程序，用来登入远程系统或远程执行命令。旨在不同的主机之间提供安全的通信。

```
ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec] [-D [bind_address:]port] [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file] [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]] [user@]hostname [command]
```

## 2.1 AUTHENTICATION认证

Open SSH客户端支持SSH协议1和2，默认为2，一般不建议更改为2。

默认认证的方式与顺序：

1. GSSAPI-based authentication（通用安全服务应用程序接口认证）,
2. host-based authentication,
3. public key authentication,
4. challenge-response authentication,
5. password authentication

**PreferredAuthentications** 可以更改认证的顺序。

**公钥加密认证：**

- 每个用户创建一对公钥和私钥
- 服务器保持公钥，用户自己保持私钥
- ssh使用DSA，ECDSA，Ed25519，RSA其中之一，自动实现公钥认证协议

- ~/.ssh/authorized_keys列出了允许登录的公钥。用户登录时，ssh程序服务器使用哪一对密钥来认证。客户端确保使用私钥，服务器通过公钥来检查。
- 通过ssh-keygen来生成密钥对
- 私钥存储在：

 ~/.ssh/identity (protocol 1)，

 ~/.ssh/id_dsa (DSA)，

 ~/.ssh/id_ecdsa (ECDSA)，

 ~/.ssh/id_ed25519 (Ed25519)，

 ~/.ssh/id_rsa (RSA)，

- 公钥存储在：

 ~/.ssh/identity.pub (protocol1),

 ~/.ssh/id_dsa.pub (DSA),

 ~/.ssh/id_ecdsa.pub (ECDSA),

 ~/.ssh/id_ed25519.pub (Ed25519),

 ~/.ssh/id_rsa.pub (RSA)

- 用户需要将公钥复制到远程主机的~/.ssh/authorized_keys 文件中。每行一个key。


## 2.2 文件

- ~/.ssh/

  所有用户配置和认证信息的默认目录。权限建议要为700.

- ~/.ssh/authorized_keys

  列出用户可以登录的公钥。建议权限为600。

- ~/.ssh/identity

  ~/.ssh/id_dsa

  ~/.ssh/id_ecdsa

  ~/.ssh/id_ed25519

  ~/.ssh/id_rsa

 包含认证的私钥。数据极为敏感，建议权限为400。

- ~/.ssh/identity.pub

 ~/.ssh/id_dsa.pub

 ~/.ssh/id_ecdsa.pub

 ~/.ssh/id_ed25519.pub

 ~/.ssh/id_rsa.pub

 包含认证的公钥，不敏感，但最好也别让别人可读。

- ~/.ssh/known_hosts

 包含曾经登录过的主机，且不再全局/etc/ssh/ssh_known_hosts中记录。

- /etc/ssh/ssh_config

 全局配置文件

- /etc/ssh/ssh_host_key

 /etc/ssh/ssh_host_dsa_key

 /etc/ssh/ssh_host_ecdsa_key

 /etc/ssh/ssh_host_ed25519_key

 /etc/ssh/ssh_host_rsa_key

# 3. 实战

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
