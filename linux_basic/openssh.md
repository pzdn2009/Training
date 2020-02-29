# OpenSSH简介

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




