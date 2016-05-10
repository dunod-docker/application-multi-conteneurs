FROM centos:7

MAINTAINER docker-ecosysteme@dunod.com

RUN yum install -y epel-release && \
    yum update -y && \
    yum install -y php-common php-cli php-fpm php-mysql php-apcu php-curl php-intl php-mbstring && \
    yum clean all
    #php-common php-cli php-mbstring php-fpm php-intl php-mysql php-apcu

 RUN usermod -u 1000 apache

COPY symfony.ini /etc/php.d/
COPY symfony.pool.conf /etc/php-fpm.d/
RUN rm -rf /etc/php-fpm.d/www.conf && \
    mkdir -p /var/www/ && \
    mkdir -p /var/lib/php/session && \
    chown -R apache:apache /var/lib/php

RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony && \
    chmod a+x /usr/local/bin/symfony

EXPOSE 9000

CMD ["/usr/sbin/php-fpm", "-c /etc/php-fpm.d"]
