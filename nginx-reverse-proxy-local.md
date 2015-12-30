Forward port 80 to a different port on a local box with nginx reverse proxy
==============

brew install nginx

edit `/usr/local/etc/nginx/nginx.conf`

```
vim /usr/local/etc/nginx/nginx.conf
```


```
events { worker_connections 1024; }
http {

    server {
        listen 80 default;
        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_redirect http:// https://;

            add_header Pragma "no-cache";

            proxy_pass http://localhost:3449;
            proxy_redirect http://localhost:3449;
        }
    }

}
```

Start nginx as root :

```
sudo nginx
```
