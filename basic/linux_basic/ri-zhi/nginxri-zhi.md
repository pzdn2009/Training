# Nginx日志

目录：/var/log/nginx

* access
* error

## access

格式：

```text
#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
```

示例：

```text
113.67.11.244 - - [27/Feb/2020:12:02:56 +0800] "GET /static/css/chunk-1da2ac62.d7a2e0af.css HTTP/1.1" 200 938 "https://shuren.dysong.com/shop/cate" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:02:56 +0800] "GET /static/js/chunk-68e1a4f1.3b72575f.js HTTP/1.1" 200 15883 "https://shuren.dysong.com/shop/cate" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:02:56 +0800] "GET /static/js/chunk-745fd5c1.c9d39376.js HTTP/1.1" 200 66496 "https://shuren.dysong.com/shop/cate" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:02:56 +0800] "GET /static/js/chunk-1da2ac62.f52b8fd8.js HTTP/1.1" 200 69925 "https://shuren.dysong.com/shop/cate" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:02:59 +0800] "GET /shop/goods HTTP/1.1" 404 555 "https://shuren.dysong.com/shop/goods" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:03:06 +0800] "GET /static/css/chunk-1acbe9df.0fe9533e.css HTTP/1.1" 200 739 "https://shuren.dysong.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:03:06 +0800] "GET /static/js/chunk-1acbe9df.edde1038.js HTTP/1.1" 200 5046 "https://shuren.dysong.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
113.67.11.244 - - [27/Feb/2020:12:03:06 +0800] "GET /static/js/chunk-2d0d6345.ccdcf491.js HTTP/1.1" 200 56975 "https://shuren.dysong.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" "-"
```

## error

示例：

```text
2020/02/28 05:51:16 [error] 13489#0: *11801 open() "/usr/share/nginx/html/owa/auth/logon.aspx" failed (2: No such file or directory), client: 128.14.209.234, server: shuren-api.dysong.com, request: "GET /owa/auth/logon.aspx HTTP/1.1", host: "120.27.233.228"
2020/02/28 11:03:36 [error] 13489#0: *11805 open() "/usr/share/nginx/html/owa/auth/logon.aspx" failed (2: No such file or directory), client: 35.160.104.209, server: shuren-api.dysong.com, request: "GET /owa/auth/logon.aspx HTTP/1.1", host: "120.27.233.228:443"
2020/02/28 19:27:10 [error] 13489#0: *11811 open() "/usr/share/nginx/html/otsmobile/app/mgs/mgw.htm" failed (2: No such file or directory), client: 110.106.101.62, server: shuren-api.dysong.com, request: "GET /otsmobile/app/mgs/mgw.htm?operationType=com.cars.otsmobile.queryLeftTicketO&requestData=%5B%7B%22train_date%22%3A%2220200309%22%2C%22purpose_codes%22%3A%2200%22%2C%22from_station%22%3A%22PIJ%22%2C%22to_station%22%3A%22POJ%22%2C%22station_train_code%22%3A%22%22%2C%22start_time_begin%22%3A%220000%22%2C%22start_time_end%22%3A%222400%22%2C%22train_headers%22%3A%22QB%23%22%2C%22train_flag%22%3A%22%22%2C%22seat_type%22%3A%22%22%2C%22seatBack_Type%22%3A%22%22%2C%22ticket_num%22%3A%22%22%2C%22dfpStr%22%3A%22%22%2C%22baseDTO%22%3A%7B%22check_code%22%3A%22f0de4c7c282e96f9d48b01c7674c5510%22%2C%22device_no%22%3A%22460006889635310%7C860878030554164%7CFIf5Sx0Pl5%22%2C%22mobile_no%22%3A%22%22%2C%22os_type%22%3A%22a%22%2C%22time_str%22%3A%2220200228192710%22%2C%22user_name%22%3A%22%22%2C%22version_no%22%3A%224.3.11%22%7D%7D%5D&ts=1582889230369&sign= HTTP/1.1", host: "mobile.12306.cn"
```

## PS

1. 使用zcat查看gz文件；
2. 查看当天Nginx日志：`cat /usr/local/nginx/logs/error.log | grep "$(date +"%Y/%m/%d")"`

