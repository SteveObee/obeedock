FROM php:apache

COPY vhost.conf /etc/apache2/sites-enabled/vhost.conf

RUN usermod -u 1000 www-data && \
    groupmod -g 1000 www-data


