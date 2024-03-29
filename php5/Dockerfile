FROM php:5.6-fpm-alpine

ARG LANGUAGE
ARG TIMEZONE

EXPOSE 80
EXPOSE 9000

VOLUME [ "/var/www/html" ]
WORKDIR /var/www/html

# Install base packages ================================================================================================
RUN apk --no-cache --update add \
    linux-headers  curl-dev freetype-dev git graphicsmagick htop icu-dev imagemagick libjpeg-turbo-dev libmcrypt-dev \
    libpng-dev libxml2-dev lynx mc mysql-client nano sqlite-dev tree tzdata unzip vim wget freetype-dev autoconf g++ \
    imagemagick imagemagick-dev libtool make libmemcached-dev tzdata ncurses pcre-dev \
    ${PHPIZE_DEPS}

# Set locale ===========================================================================================================
ENV LANG=${LANGUAGE}
ENV LANGUAGE=${LANGUAGE}

RUN cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime

# Apply pecl certificate patches (ignoring erros) ======================================================================
COPY patches/ca-certificates.crt.patch /tmp
COPY patches/cert.pem.patch /tmp

RUN patch /etc/ssl/certs/ca-certificates.crt /tmp/ca-certificates.crt.patch; exit 0
RUN patch /etc/ssl/cert.pem /tmp/cert.pem.patch; exit 0

RUN rm /etc/ssl/certs/2e5ac55d.0 && \
    rm /etc/ssl/certs/ca-cert-DST_Root_CA_X3.pem

# Install pecl packages ================================================================================================
RUN  pecl channel-update pecl.php.net \
    && yes '' | pecl install -o -f redis-3.1.0 imagick-3 xdebug-2.5.5 memcached-2.2.0 ssh2-0.13

# Install php packages =================================================================================================
RUN apk --no-cache --update add \
    php5 php5-common php5-dev php5-curl php5-dom php5-exif php5-ftp php5-gd php5-iconv php5-openssl php5-posix \
    php5-soap php5-zip php5-ldap php5-bcmath php5-calendar php5-gettext php5-json php5-pcntl php5-apcu php5-phar \
    php5-sockets php5-xmlrpc php5-pdo php5-mysql php5-mysqli php5-wddx php5-mcrypt php5-xmlreader php5-zip php5-zlib \
    php5-xsl php5-opcache php5-ctype php5-pdo_mysql php5-pdo_sqlite php5-sqlite3 php5-intl php5-fpm

# Install php extensions ===============================================================================================
RUN docker-php-ext-install \
    zip mcrypt curl tokenizer session ctype simplexml opcache pdo pdo_mysql iconv xml dom mbstring gd intl soap bcmath \
    pdo_mysql mysqli

RUN docker-php-ext-enable imagick
RUN docker-php-ext-enable redis
RUN docker-php-ext-enable ssh2
RUN docker-php-ext-enable mysqli
RUN docker-php-ext-enable xml
RUN docker-php-ext-enable soap

# Remove temp files ====================================================================================================
RUN rm -rf /tmp/* \
    rm -rf /var/cache/apk/*

# Install composer =====================================================================================================
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Log files ============================================================================================================
RUN ln -sf /dev/stdin /var/log/fpm5-access.log
RUN ln -sf /dev/stderr /var/log/fpm5-error.log

# Execute command ======================================================================================================
CMD [ "php-fpm" ]
