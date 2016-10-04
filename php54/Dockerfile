FROM php:5.4-apache

ENV DB_PASSWD admin

# Avoid MySQL questions during installation
#ENV DEBIAN_FRONTEND noninteractive
#RUN echo mysql-server-5.5 mysql-server/root_password password $DB_PASSWD | debconf-set-selections
#RUN echo mysql-server-5.5 mysql-server/root_password_again password $DB_PASSWD | debconf-set-selections

RUN apt-get update \
    && apt-get install -y libmcrypt-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libfreetype6-dev \
        libxml2-dev \
        mysql-client \
        wget \
        unzip \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install iconv mcrypt pdo mysql pdo_mysql mbstring soap gd zip

# Install Ioncube loader
RUN mkdir /ioncube && \
    cd /ioncube && \
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz && \
    tar xvf ioncube_loaders_lin_x86-64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /ioncube/ioncube/ioncube_loader_lin_5.6.so . && \
    rm -rf /ioncube/

# Apache configuration
RUN a2enmod rewrite
RUN chown www-data:www-data -R /var/www/html/