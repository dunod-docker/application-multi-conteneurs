FROM centos:7

MAINTAINER docker-ecosysteme@dunod.com

RUN yum install -y epel-release && \
    yum update -y && \
    yum install -y mariadb-server mariadb-client\
                   nginx \
                   php-common php-cli php-fpm php-mysql php-apcu php-curl php-intl php-mbstring \
                   supervisor && \
    yum clean all

#Copie de la configuration de Nginx
COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY nginx/symfony.conf /etc/nginx/sites-available/
RUN mkdir -p /etc/nginx/sites-enabled/ && \
    ln -s /etc/nginx/sites-available/symfony.conf /etc/nginx/sites-enabled/symfony

#Adaptation de l'UID du user apache
RUN usermod -u 1000 apache

#Copie de la configuration php pour Symfony
COPY php-fpm/symfony.ini /etc/php.d/
COPY php-fpm/symfony.pool.conf /etc/php-fpm.d/
RUN rm -rf /etc/php-fpm.d/www.conf && \
    mkdir -p /var/www/ && \
    mkdir -p /var/lib/php/session && \
    chown -R apache:apache /var/lib/php

#Copie des ficher de l'application Symfony
RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony && \
    chmod a+x /usr/local/bin/symfony

# Execute le script d'installation de MariaDB et donne les droits à l'utilisateur mysql
RUN /usr/bin/mysql_install_db > /dev/null && \
    chown -R mysql:mysql /var/lib/mysql/
#Requis par MariaDB pour pouvoir utiliser le client MariaDB depuis l'intérieur du container
RUN echo -e "\nexport TERM=xterm" >> ~/.bashrc

# Exposition du port 80 servi par nginx
EXPOSE 80

#Définitions d'un volume pour les fichiers de MariaDB
VOLUME ["/var/lib/mysql/", "/var/www/"]

# Copie de la configuration de supervisor
COPY supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Démarrage de supervisor en mode bloquant
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
