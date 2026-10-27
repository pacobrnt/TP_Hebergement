FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get install -y --no-install-recommends apache2 openssl \
 && a2enmod ssl headers deflate \
 && mkdir -p /etc/apache2/ssl \
 && openssl req -x509 -nodes -days 365 -newkey rsa:2048 -subj "/CN=localhost" \
      -keyout /etc/apache2/ssl/server.key -out /etc/apache2/ssl/server.crt \
 && printf '<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><title>Bonjour</title></head><body><h1>Bonjour</h1></body></html>' > /var/www/html/index.html \
 && sed -i 's|/etc/ssl/certs/ssl-cert-snakeoil.pem|/etc/apache2/ssl/server.crt|; s|/etc/ssl/private/ssl-cert-snakeoil.key|/etc/apache2/ssl/server.key|' /etc/apache2/sites-available/default-ssl.conf \
 && a2ensite default-ssl \
 && sed -i 's/ServerTokens OS/ServerTokens Prod/; s/ServerSignature On/ServerSignature Off/' /etc/apache2/conf-available/security.conf \
 && rm -rf /var/lib/apt/lists/*
EXPOSE 80 443
CMD ["apache2ctl", "-D", "FOREGROUND"]
