# PHP镜像源
ARG PHP_IMAGE_REPO

# PHP镜像标签
ARG PHP_IMAGE_TAG

FROM ${PHP_IMAGE_REPO}:${PHP_IMAGE_TAG}

# 国内镜像
ARG USE_CHINESE_MIRRORS=false
RUN if [ ${USE_CHINESE_MIRRORS} = true ]; then \
    sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list \
;fi

RUN pecl channel-update pecl.php.net

# 更新软件源
RUN set -xe; \
    apt-get update -yqq && \
    pecl channel-update pecl.php.net && \
    apt-get install -yqq \
    apt-utils \
    htop

COPY ./ini/php.ini /usr/local/etc/php/php.ini

# gd
ARG PHP_INSTALL_GD=false
RUN if [ ${PHP_INSTALL_GD} = true ]; then \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
;fi

# gettext
ARG PHP_INSTALL_GETTEXT=false
RUN if [ ${PHP_INSTALL_GETTEXT} = true ]; then \
    docker-php-ext-install gettext \
;fi

# sockets
ARG PHP_INSTALL_SOCKETS=false
RUN if [ ${PHP_INSTALL_SOCKETS} = true ]; then \
    docker-php-ext-install sockets \
;fi

# bcmath
ARG PHP_INSTALL_BCMATH=false
RUN if [ ${PHP_INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
;fi

# zip
ARG PHP_INSTALL_ZIP=false
RUN if [ ${PHP_INSTALL_ZIP} = true ]; then \
    apt-get install -yqq \
    libzip-dev zip unzip && \
    docker-php-ext-configure zip && \
    docker-php-ext-install zip && \
    php -m | grep -q 'zip' \
;fi

# swoole
ARG PHP_INSTALL_SWOOLE=false
RUN if [ ${PHP_INSTALL_SWOOLE} = true ]; then \
    pecl install -o -f swoole \
    && docker-php-ext-enable swoole \
;fi

# pdo_mysql
ARG PHP_INSTSLL_PDO_MYSQL=false
RUN if [ ${PHP_INSTSLL_PDO_MYSQL} = true ]; then \
    docker-php-ext-install pdo pdo_mysql \
;fi

# mongodb
ARG PHP_INSTALL_MONGODB=false
RUN if [ ${PHP_INSTALL_MONGODB} = true ]; then \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi

# opcache
ARG PHP_INSTALL_OPCACHE=false
RUN if [ ${PHP_INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi
COPY ./ini/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# mysqli
ARG PHP_INSTALL_MYSQLI=false
RUN if [ ${PHP_INSTALL_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi
COPY ./ini/mysql.ini /usr/local/etc/php/conf.d/mysql.ini

# mysql-client
USER root
ARG PHP_INSTALL_MYSQL_CLIENT=false
RUN if [ ${PHP_INSTALL_MYSQL_CLIENT} = true ]; then \
      apt-get -y install default-mysql-client \
;fi

# redis
ARG PHP_INSTALL_REDIS=false
RUN if [ ${PHP_INSTALL_REDIS} = true ]; then \
    pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
;fi

# Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

ARG INSTALL_OCI8=false

ENV LD_LIBRARY_PATH="/opt/oracle/instantclient_12_1"
ENV OCI_HOME="/opt/oracle/instantclient_12_1"
ENV OCI_LIB_DIR="/opt/oracle/instantclient_12_1"
ENV OCI_INCLUDE_DIR="/opt/oracle/instantclient_12_1/sdk/include"
ENV OCI_VERSION=12
