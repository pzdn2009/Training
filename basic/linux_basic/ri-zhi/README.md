# 日志

在Centos 7.x / RHEL 7.x 的版本，系统日志是由一个名为 rsyslog的服务管理的，默认的日志守护进程为 rsyslog , **rsyslog** 是 syslog 的升级版本，默认安装，随机启动。

```bash
systemctl status rsyslog # 查看服务状态
rpm -ql rsyslog # 列出所有的文件与目录
 /etc/rsyslog.conf # 主要配置文件
 /etc/rsyslog.d # 主要配置文件
```

/etc/rsyslog.conf：

```text
# Log anything (except mail) of level info or higher.
# Don't log private authentication messages!
*.info;mail.none;authpriv.none;cron.none                /var/log/messages

# The authpriv file has restricted access.
authpriv.*                                              /var/log/secure

# Log all the mail messages in one place.
mail.*                                                  -/var/log/maillog


# Log cron stuff
cron.*                                                  /var/log/cron

# Everybody gets emergency messages
*.emerg                                                 :omusrmsg:*

# Save news errors of level crit and higher in a special file.
uucp,news.crit                                          /var/log/spooler

# Save boot messages also to boot.log
local7.*                                                /var/log/boot.log
```

实践：

```text
egrep -v "^#|^$" /etc/rsyslog.conf 

$ModLoad imuxsock # provides support for local system logging (e.g. via logger command)
$ModLoad imjournal # provides access to the systemd journal
$WorkDirectory /var/lib/rsyslog
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
$IncludeConfig /etc/rsyslog.d/*.conf
$OmitLocalLogging on
$IMJournalStateFile imjournal.state
*.info;mail.none;authpriv.none;cron.none                /var/log/messages
authpriv.*                                              /var/log/secure
mail.*                                                  -/var/log/maillog
cron.*                                                  /var/log/cron
*.emerg                                                 :omusrmsg:*
uucp,news.crit                                          /var/log/spooler
local7.*                                                /var/log/boot.log
```

