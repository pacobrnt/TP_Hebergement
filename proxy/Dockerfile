FROM alpine:3.20
RUN apk add --no-cache nginx openssl \
 && mkdir -p /run/nginx /etc/nginx/ssl \
 && openssl req -x509 -nodes -days 365 -newkey rsa:2048 -subj "/CN=localhost" \
      -keyout /etc/nginx/ssl/server.key -out /etc/nginx/ssl/server.crt \
 && sed -i 's|root   /var/lib/nginx/html;|proxy_pass http://web:80;\n        proxy_set_header X-Real-IP $remote_addr;\n        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;|' /etc/nginx/http.d/default.conf \
 && sed -i '$a server {\n    listen 443 ssl;\n    ssl_certificate /etc/nginx/ssl/server.crt;\n    ssl_certificate_key /etc/nginx/ssl/server.key;\n    location / {\n        proxy_pass http://web:80;\n        proxy_set_header X-Real-IP $remote_addr;\n        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;\n    }\n}' /etc/nginx/http.d/default.conf \
 && sed -i '/http {/a\    server_tokens off;\n    gzip on;' /etc/nginx/nginx.conf
EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]
