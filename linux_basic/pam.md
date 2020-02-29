# PAM

Pluggable Authentication Module，可插入身份验证模块(PAM).

PAM应用在许多程序与服务上，比如登录程序(login、su)的PAM身份验证（口令认证、限制登录），passwd强制密码，用户进程实时管理，向用户分配系统资源等。

## 配置文件

/etc/pam.d/

配置规则：
```
service　　type 　　control　　module-path　　module-arguments
服务名称　　 类型　　  控制　　　　模块路径　　　　 模块参数　
```
/etc/pam.d/passwd：

```conf
#%PAM-1.0
auth       include      system-auth
account    include      system-auth
password   substack     system-auth
-password   optional    pam_gnome_keyring.so use_authtok
password   substack     postlogin
```

## 验证类别type

验证类别主要分为四种：

* auth：是 authentication (认证) 的缩写，所以这种类别主要用来检验使用者的身份验证，这种类别通常是需要**口令**来检验的， 所以后续接的模块是用来检验用户的身份。
* account：账户模块接口，检查指定账户是否满足当前验证条件，**检查账户是否到期等**。
* session：会话模块接口，用于管理和配置用户会话。会话在用户成功认证之后启动生效
* password：密码模块接口，用于更改用户密码，以及强制使用强密码配置

## 控制标志Control

**简单语法：**
* required：若成功则带有 success (成功) 的标志，若失败则带有 failure 的标志，但不论成功或失败都会继续后续的验证流程。 由于后续的验证流程可以继续进行，因此相当有利于数据的登录 (log) ，这也是 PAM 最常使用 required 的原因。

* requisite：若验证失败则立刻返回 failure 的标志，并终止后续的验证流程。若验证成功则带有 success 的标志并继续后续的验证流程。 这个项目与 required 最大的差异，就在于失败的时候还要不要继续验证下去。由于 requisite 是失败就终止， 因此失败时所产生的 PAM 信息就无法透过后续的模块来记录了。

* sufficient：若验证成功则立刻回传 success 给原程序，并终止后续的验证流程；若验证失败则带有 failure 标志并继续后续的验证流程。 这玩意儿与 requisits 刚好相反！
 
* optional ：该模块返回的通过/失败结果被忽略。当没有其他模块被引用时，标记为optional模块并且成功验证时该模块才是必须的。

* include:将指定配置文件中所有的type类型作为参数包含在此控制语句中

* substack:将指定配置文件中type作为参数包含在此控制语句中，和Include不同的是，在substack中完成任务或者die，只会影响substack内控制命令，不会影响完整的stack桟

**control复杂语法**
`[value1=action1 value2=action2 ...]`

valueN代表对应模块中控制语句的返回值，可以是下面的值：`success,open_err,symbol_err,service_err,system_err,buf_err,perm_denied,deault`等，其中default表示所有未被显式提到的所有值。


##  常用模块

pam_securetty.so：
限制系统管理员 (root) 只能够从安全的 (secure) 终端机登陆；例如 tty1, tty2 等就是传统的终端机装置名称。那么安全的终端机配置呢？ 就写在 /etc/securetty 这个文件中。你可以查阅一下该文件， 就知道为什么 root 可以从 tty1~tty7 登陆，但却无法透过 telnet 登陆 Linux 主机了！
 
pam_nologin.so：
这个模块可以限制一般用户是否能够登陆主机之用。当 /etc/nologin 这个文件存在时，则所有一般使用者均无法再登陆系统了！若 /etc/nologin 存在，则一般使用者在登陆时， 在他们的终端机上会将该文件的内容显示出来！所以，正常的情况下，这个文件应该是不能存在系统中的。 但这个模块对 root 以及已经登陆系统中的一般账号并没有影响。
 
pam_selinux.so：
SELinux 是个针对程序来进行细部管理权限的功能。由于 SELinux 会影响到用户运行程序的权限，因此我们利用 PAM 模块，将 SELinux 暂时关闭，等到验证通过后， 再予以启动！
 
pam_console.so：
当系统出现某些问题，或者是某些时刻你需要使用特殊的终端接口 (例如 RS232 之类的终端联机设备) 登陆主机时， 这个模块可以帮助处理一些文件权限的问题，让使用者可以透过特殊终端接口 (console) 顺利的登陆系统。
 
pam_loginuid.so：
我们知道系统账号与一般账号的 UID 是不同的！一般账号 UID 均大于 500 才合理。 因此，为了验证使用者的 UID 真的是我们所需要的数值，可以使用这个模块来进行规范！
 
pam_env.so：
用来配置环境变量的一个模块，如果你有需要额外的环境变量配置，可以参考 /etc/security/pam_env.conf 这个文件的详细说明。
 
pam_unix.so：
这是个很复杂且重要的模块，这个模块可以用在验证阶段的认证功能，可以用在授权阶段的账号许可证管理， 可以用在会议阶段的登录文件记录等，甚至也可以用在口令升级阶段的检验！非常丰富的功能！ 这个模块在早期使用得相当频繁喔！
 
pam_cracklib.so：
可以用来检验口令的强度！包括口令是否在字典中，口令输入几次都失败就断掉此次联机等功能，都是这模块提供的！ 这玩意儿很重要！
