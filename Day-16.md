# Day 16: Install and Configure Nginx as an LBR

## Install nginx if not installed
```
sudo yum install nginx -y
```

## Update the file /etc/nginx/nginx.conf 
```
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    upstream app_servers {
        server stapp01:3000;
        server stapp02:3000;
        server stapp03:3000;
    }

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;

        location / {
            proxy_pass http://app_servers;
        }

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
}
```

## Check the configuration 
```
sudo nginx -t
```
## Restart the nginx server
```
sudo systemctl restart nginx 
```
