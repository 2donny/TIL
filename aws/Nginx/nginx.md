## Nginx

```shell
user www-data;
worker_processes auto;
error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}

http {
    ##****
    # Basic Settings
    ##

    server {
        server_name xircle-server;
        listen 80 default_server;
        listen [::]:80 default_server;

        access_logo /home/ec2-user/client/server_logs/host.access.log main;

        location / {
                root /home/ubuntu/client/deploy;
                index index.html;
                try_files $uri /index.html;
                add_header X-Frame-Options SAMEORIGIN;
                add_header X-Content-Type-Options nosniff;
                add_header X-XSS-Protection "1; mode=block";
                add_header Strict-Transport-Security "max-age=31536000; includeSubdomains;";

                proxy_set_header HOST $host;
                proxy_pass http://127.0.0.1:8080;
                proxy_redirect off;
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
                root /usr/share/nginx/html;
        }

        server_tokens off;

        location ~ /\.ht {
                deny all;
        }
    }

    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;
    client_body_buffer_size 100k;
    client_body_buffer_size 100k;
    client_header_buffer_size 1k;
    client_max_body_size 100k;
    large_client_header_buffers 2 1k;
    client_body_timeout 10;
    client_header_timeout 10;
    keepalive_timeout 5 5;
    send_timeout 10;
    server_tokens off;
    gzip  on;

    include /etc/nginx/conf.d/*.conf;
```

## 트러블 슈팅
1. ```sudo service nginx start``` 실행시 80 port already in use 에러 발생.
   1. ```sudo fuser -k 80/tcp```로 80포트 프로세스 제거해서 해결.
2. nginx가 active한 상태지만, 80 포트로 접근하면 502 Gateway error가 발생.
   1. http 블럭내 버퍼와 타임아웃을 증가시켜도 해결 안됨.
   2. http로 요청이 들어왔을 때 리버스 프록싱해서(proxy_pass) 8080 포트로 넘겨줬었는데, 생각해보니 8080 포트에서 running하고 있는 프로세스가 없었다. 제대로된 respond가 없으니 프록시 서버에서 502 Gateway 에러를 낸 것.
   3. 당연히 proxy_pass를 제거하니까 문제는 해결되었다. 추후에 SSL 인증서를 발급받아서 8080 포트로 리버스 프록싱해야겠다.

