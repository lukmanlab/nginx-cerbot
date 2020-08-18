# Simple Guide

## RUN Nginx First:
```
docker-compose up -d nginx
````

## Request Certificate:

```
docker-compose run --rm cerbot \
certonly --webroot --email admin@wormroot.tech \
--agree-tos --no-eff-email --webroot-path=/data/letsencrypt \
-d wormroot.tech
```

## Edit config Nginx

```
vi conf.d/wormroot.tech.conf
````

### Enable Protocol `443` and Pointing SSL path:

* Before:
```
server {
    listen       80;
    #listen       443 ssl;
    server_name  wormroot.tech;
    root   /usr/share/nginx/html;
    
    location / {
        index index.php index.html;
    }
    
    location ~ /.well-known/acme-challenge {
        allow all;
        try_files $uri $uri/ /index.php;
    }

    #ssl_certificate /etc/letsencrypt/live/wormroot.tech/fullchain.pem; 
    #ssl_certificate_key /etc/letsencrypt/live/wormroot.tech/privkey.pem; 
}
```

* After:
```
server {
    listen       80;
    listen       443 ssl;
    server_name  wormroot.tech;
    root   /usr/share/nginx/html;
    
    location / {
        index index.php index.html;
    }
    
    location ~ /.well-known/acme-challenge {
        allow all;
        try_files $uri $uri/ /index.php;
    }

    ssl_certificate /etc/letsencrypt/live/wormroot.tech/fullchain.pem; 
    ssl_certificate_key /etc/letsencrypt/live/wormroot.tech/privkey.pem; 
}
```

Restart Nginx:
```
docker-compose restart nginx
```