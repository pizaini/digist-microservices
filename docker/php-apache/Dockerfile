FROM php:7.4-apache
LABEL maintainer="Pizaini <instagram.com/pizaini>"

#Copy config files
COPY conf/000-default.conf /etc/apache2/sites-available/
COPY conf/apache2.conf /etc/apache2/

#change doc root
ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

#Enable necessary mods
RUN ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
RUN ln -s /etc/apache2/mods-available/headers.load /etc/apache2/mods-enabled/headers.load

#Install extension
RUN apt-get update && apt-get install -y libpq-dev
RUN docker-php-ext-install pdo_pgsql
#RUN docker-php-ext-install pdo_mysql

#copy php.ini
# Please note the default SESSION SAVE PATH in php.ini
COPY ./conf/php.ini "$PHP_INI_DIR/php.ini"

#Supervisor
RUN apt-get update && apt-get install -y --no-install-recommends --no-install-suggests supervisor
COPY ./conf/supervisor-send-email.conf /etc/supervisor/conf.d/supervisor-send-email.conf
