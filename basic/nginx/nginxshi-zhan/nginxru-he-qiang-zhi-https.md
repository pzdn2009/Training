# Nginx如何强制https

```nginx
server
{
    listen 80;
    server_name www.aab.com ddss.com;
    rewrite ^(.*)$ https://$host$1 permanent;
}
```