# mail服务

## 安装配置


```
yum install mailx -y

cat>>/etc/mail.rc<<EOF
set bsdcompat
set from=p18576670179@163.com               #发送邮件后显示的邮件发送方
set smtp=smtp.163.com                      #网易邮箱smtp邮件服务器地址
set smtp-auth-user=*****@163.com       #发件人邮箱
set smtp-auth-password=****            #发件人邮箱密码
set smtp-auth=login                         #动作为登录
EOF
```

## 发送邮件

```
mail -s "subject" address
mail -s "subject" address < bodyfile
echo '你好'| mail -s '主题' xxxxxxxx@qq.com
```
