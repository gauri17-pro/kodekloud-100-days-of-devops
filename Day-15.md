# Day 15: Setup SSL for Nginx

nautilus.key and nautilus.crt files are given

## Create the directory
`sudo mkdir -p /etc/nginx/ssl`

## Move the files to appropriate location
```
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/nautilus.crt
sudo mv /tmp/nautilus.key /etc/nginx/ssl/nautilus.key
```

## Give appropriate permissions to the files
```
sudo chmod 644 /etc/nginx/ssl/nautilus.crt
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```

## Create the configuration file
```
sudo vi /etc/nginx/conf.d/ssl.conf
```

## Paste the following content
```
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Test
```
sudo nginx -t
```

## Restart Nginx
```
sudo systemctl restart nginx
```

Note: In curl -Ik https://stapp01/, the options mean:
- -I = send a HEAD request. It fetches only the response headers, not the page body.
- -k = insecure mode. It tells curl to ignore SSL/TLS certificate validation errors, which is useful here because the certificate is self-signed.
