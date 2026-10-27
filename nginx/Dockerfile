FROM alpine:3.20
RUN apk add --no-cache nginx \
 && mkdir -p /run/nginx /var/www/html \
 && printf '<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><title>Bonjour</title></head><body><h1>Bonjour</h1></body></html>' > /var/www/html/index.html \
 && sed -i 's|/var/lib/nginx/html|/var/www/html|g' /etc/nginx/http.d/default.conf \
 && sed -i '/http {/a\    server_tokens off;\n    gzip on;\n    gzip_types text/html text/css application/javascript;' /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
