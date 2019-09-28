FROM richarvey/nginx-php-fpm

#代码
ADD ./src /var/www/html

#镜像仓库
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

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

#NGINX配置
ADD docker/nginx.conf /etc/nginx/nginx.conf
ADD docker/vhost.conf /etc/nginx/sites-available/default.conf
ADD docker/vhost-ssl.conf /etc/nginx/sites-available/default-ssl.conf


#修改 composer 为国内镜像
RUN composer config -g repos.packagist composer https://mirrors.cloud.tencent.com/composer/
#允许composer
ENV COMPOSER_ALLOW_SUPERUSER 1


#工作目录
WORKDIR /var/www/html

COPY docker/entrypoint.sh /scripts/entrypoint.sh

#启动服务
ENTRYPOINT ["entrypoint.sh"]
