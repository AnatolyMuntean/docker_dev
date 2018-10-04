FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install -y software-properties-common

RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get -y install apache2 libapache2-mod-php7.1 \
    php7.1-curl \
    php7.1-gd \
    php7.1-mbstring \
    php7.1-mcrypt \
    php7.1-pdo \
    php7.1-mysql \
    php7.1-mongo \
    php7.1-dom \
    php7.1-soap \
    php7.1-xdebug \
    php7.1-zip

COPY ./www/phpinfo.php /var/www/html/
COPY ./www/adminer-4.6.3-mysql-en.php /var/www/html/adminer.php

COPY ./conf/20-xdebug.ini /etc/php/7.1/apache2/conf.d/

EXPOSE 80


