FROM centos:7

RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm &&  \
    yum install yum-utils && \
    yum-config-manager --enable remi-php72 && \
    yum update -y && \
    yum install -y mariadb-server mariadb-client\
                   nginx \
                   php php-xml php-pecl-zip php-cli php-fpm php-mbstring php-mysqlnd php-opcache \
                   supervisor \
                   git unzip && \
    yum clean all


#Copie de la configuration de Nginx
COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY nginx/symfony.conf /etc/nginx/sites-available/
RUN mkdir -p /etc/nginx/sites-enabled/ && \
    ln -s /etc/nginx/sites-available/symfony.conf /etc/nginx/sites-enabled/symfony && \
    mkdir -p /var/www/

#Adaptation de l'UID du user apache
RUN usermod -u 1000 apache

#Copie de la configuration php pour Symfony
COPY php-fpm/symfony.pool.conf /etc/php-fpm.d/
RUN rm -rf /etc/php-fpm.d/www.conf

#C'est ici que le fichier PID de PHP-FPM sera créé
RUN mkdir /run/php-fpm && chown -R apache:apache /run/php-fpm

#Installation de Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
ENV COMPOSER_CACHE_DIR /tmp/composer


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
