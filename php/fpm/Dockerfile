FROM php:7.2-fpm-alpine3.9


#代码
ADD . /var/www/html

#镜像仓库
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

#LDAP
COPY docker/ldap.conf /etc/openldap/ldap.conf
COPY docker/cron  /var/spool/cron/crontabs/root
COPY docker/entrypoint /scripts/docker-entrypoint

#PHP扩展
RUN apk add --no-cache  \
        openldap \
        openldap-dev \
        cyrus-sasl-dev \
        libsasl \
        cyrus-sasl \
    && ln -s /usr/include/sasl/sasl.h /usr/lib/sasl.h \
    && docker-php-ext-configure ldap --with-libdir=lib/ --with-ldap-sasl \
    && docker-php-ext-install ldap pdo_mysql mysqli \
    && curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

#修改 composer 为国内镜像
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

#允许composer
ENV COMPOSER_ALLOW_SUPERUSER 1

#工作目录
WORKDIR /var/www/html

#安装依赖
RUN composer install --no-plugins --no-scripts

RUN chmod -R 777 /var/www/html/storage

#配置cron文件
RUN chown root:root /var/spool/cron/crontabs/root && chmod 600 /var/spool/cron/crontabs/root

ENTRYPOINT ["/scripts/docker-entrypoint"]

