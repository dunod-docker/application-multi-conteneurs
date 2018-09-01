FROM nginx

COPY nginx.conf /etc/nginx/nginx.conf
COPY symfony.conf /etc/nginx/sites-available/
RUN mkdir -p /etc/nginx/sites-enabled/ && \
    ln -s /etc/nginx/sites-available/symfony.conf /etc/nginx/sites-enabled/symfony && \
    mkdir -p /var/www/
