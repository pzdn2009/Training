# OpenSSH-SSH

[http://man.openbsd.org/ssh](http://man.openbsd.org/ssh)

ssh ―― SSH协议的客户端程序，用来登入远程系统或远程执行命令。旨在不同的主机之间提供安全的通信。

```text
ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec] [-D [bind_address:]port] [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file] [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]] [user@]hostname [command]
```

## AUTHENTICATION认证

Open SSH客户端支持SSH协议1和2，默认为2，一般不建议更改为2。

默认认证的方式与顺序：

1. GSSAPI-based authentication（通用安全服务应用程序接口认证）,
2. host-based authentication,
3. public key authentication,
4. challenge-response authentication,
5. password authentication

**PreferredAuthentications** 可以更改认证的顺序。

**公钥加密认证：**

* 每个用户创建一对公钥和私钥
* 服务器保持公钥，用户自己保持私钥
* ssh使用DSA，ECDSA，Ed25519，RSA其中之一，自动实现公钥认证协议
* ~/.ssh/authorized\_keys列出了允许登录的公钥。用户登录时，ssh程序服务器使用哪一对密钥来认证。客户端确保使用私钥，服务器通过公钥来检查。
* 通过ssh-keygen来生成密钥对
* 私钥存储在：

  ~/.ssh/identity \(protocol 1\)，

  ~/.ssh/id\_dsa \(DSA\)，

  ~/.ssh/id\_ecdsa \(ECDSA\)，

  ~/.ssh/id\_ed25519 \(Ed25519\)，

  ~/.ssh/id\_rsa \(RSA\)，

* 公钥存储在：

  ~/.ssh/identity.pub \(protocol1\),

  ~/.ssh/id\_dsa.pub \(DSA\),

  ~/.ssh/id\_ecdsa.pub \(ECDSA\),

  ~/.ssh/id\_ed25519.pub \(Ed25519\),

  ~/.ssh/id\_rsa.pub \(RSA\)

* 用户需要将公钥复制到远程主机的~/.ssh/**authorized\_keys** 文件中。每行一个key。

## 文件

* ~/.ssh/

  所有用户配置和认证信息的默认目录。权限建议要为700.

* ~/.ssh/authorized\_keys

  列出用户可以登录的公钥。建议权限为600。

* ~/.ssh/identity

  ~/.ssh/id\_dsa

  ~/.ssh/id\_ecdsa

  ~/.ssh/id\_ed25519

  ~/.ssh/id\_rsa

  包含认证的私钥。数据极为敏感，建议权限为400。

* ~/.ssh/identity.pub

  ~/.ssh/id\_dsa.pub

  ~/.ssh/id\_ecdsa.pub

  ~/.ssh/id\_ed25519.pub

  ~/.ssh/id\_rsa.pub

  包含认证的公钥，不敏感，但最好也别让别人可读。

* ~/.ssh/known\_hosts

  包含曾经登录过的主机，且不再全局/etc/ssh/ssh\_known\_hosts中记录。

* /etc/ssh/ssh\_config

  全局配置文件

* /etc/ssh/ssh\_host\_key

  /etc/ssh/ssh\_host\_dsa\_key

  /etc/ssh/ssh\_host\_ecdsa\_key

  /etc/ssh/ssh\_host\_ed25519\_key

  /etc/ssh/ssh\_host\_rsa\_key

