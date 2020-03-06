# RStudio

安裝：

```text
$ wget https://s3.amazonaws.com/rstudio-server/rstudio-server-rhel5-1.0.136-i686.rpm
$ sudo yum install --nogpgcheck rstudio-server-rhel5-1.0.136-i686.rpm

$ wget https://s3.amazonaws.com/rstudio-server/rstudio-server-rhel5-1.0.136-x86_64.rpm
$ sudo yum install --nogpgcheck rstudio-server-rhel5-1.0.136-x86_64.rpm
```

執行bin/rstudio

```text
rstudio-server start
```

ngxin配置:

```text
server {
        listen       18787 default_server;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
           proxy_pass http://localhost:8787;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

訪問：

[http://youripaddress:18787/](http://youripaddress:18787/) ，通过Linux用户名登陆 即可。

