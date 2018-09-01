FROM centos:7

RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm &&  \
    yum install yum-utils && \
    yum-config-manager --enable remi-php72 && \
    yum update -y && \
    yum install -y php php-xml php-pecl-zip php-cli php-fpm php-mbstring php-mysqlnd php-opcache \
                   git unzip && \
    yum clean all

COPY symfony.pool.conf /etc/php-fpm.d/
RUN rm -rf /etc/php-fpm.d/www.conf

RUN mkdir /run/php-fpm && chown -R apache:apache /run/php-fpm

EXPOSE 9000

CMD ["/usr/sbin/php-fpm", "--nodaemonize", "-c /etc/php-fpm.d"]
