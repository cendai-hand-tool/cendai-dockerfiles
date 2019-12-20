FROM php:7.2

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

RUN set -eux; \
    apt-get update; \
    apt-get install -y git libmcrypt-dev libpq-dev libcurl4-gnutls-dev libicu-dev libvpx-dev libjpeg-dev libpng-dev libxpm-dev zlib1g-dev libfreetype6-dev libxml2-dev libexpat1-dev libbz2-dev libgmp3-dev libldap2-dev unixodbc-dev libsqlite3-dev libaspell-dev libsnmp-dev libpcre3-dev libtidy-dev \
            freetds-bin freetds-dev libcurl4-gnutls-dev libxslt1-dev
# RUN ln -s /usr/lib/x86_64-linux-gnu/libsybdb.a /usr/lib/

RUN set -eux; \
    ln -s /usr/lib/x86_64-linux-gnu/libsybdb.a /usr/lib/; \
    curl -L -o /tmp/PHP-7.2.tar.gz https://github.com/php/php-src/archive/PHP-7.2.tar.gz

RUN set -eux; \
    cd /tmp; \
    tar -zxf /tmp/PHP-7.2.tar.gz; \
    cd /tmp/php-src-PHP-7.2/ext/pdo_dblib; \
    phpize; \
    ./configure; \
    make; \
    make install; \
    docker-php-ext-enable pdo_dblib; \
    php -m | grep -q 'pdo_dblib'

    # echo "extension=pdo_dblib.so" > /tmp/pdo_dblib.ini; \
    # php -i
    # cp /tmp/pdo_dblib.ini /etc/php/7.2/mods-available/pdo_dblib.ini; \
    # ln -s /etc/php/7.2/mods-available/pdo_dblib.ini /etc/php/7.2/cli/conf.d/20-pdo_dblib.ini; \
    # ln -s /etc/php/7.2/mods-available/pdo_dblib.ini /etc/php/7.2/fpm/conf.d/20-pdo_dblib.ini; \
    # service php7.2-fpm restart
RUN set -eux; \
    docker-php-ext-install mbstring curl json intl gd xml zip bz2 opcache calendar bcmath exif gettext pcntl \
            mysqli pdo_mysql pdo_pgsql pdo_sqlite pgsql shmop sockets sysvmsg sysvsem sysvshm xsl wddx

RUN set -eux; \
    curl -sS https://getcomposer.org/installer | php; \
    mv composer.phar /usr/local/bin/composer; \
    curl -LO https://deployer.org/deployer.phar; \
    mv deployer.phar /usr/local/bin/dep; \
    chmod +x /usr/local/bin/dep

RUN set -eux; \
    cd ~; \
    rm -f /tmp/pdo_dblib.ini; \
    rm -f /tmp/PHP-7.2.tar.gz; \
    rm -rf /tmp/php-src-PHP-7.2; \
    php -m
