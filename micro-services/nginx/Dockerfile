FROM nginx

MAINTAINER docker-ecosysteme@dunod.com

COPY nginx.conf /etc/nginx/nginx.conf
COPY symfony.conf /etc/nginx/sites-available/
RUN mkdir -p /etc/nginx/sites-enabled/ && \
    mkdir -p /var/www/ && \
    ln -s /etc/nginx/sites-available/symfony.conf /etc/nginx/sites-enabled/symfony
