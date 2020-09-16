FROM php:7.4.10-fpm-alpine3.12

ENV \
  PECL_EXTENSIONS="apcu pcov psr redis" \
  PHP_EXTENSIONS="bcmath bz2 calendar intl exif mysqli pdo_mysql soap sockets zip"

# Packages
RUN rm -rf /var/cache/apk/* && \
    rm -rf /tmp/* && \
    apk update && apk upgrade --available && \
    apk --no-cache add \
        autoconf \ 
    	g++ \
		file \
	    icu-dev \
        curl \
    	libzip-dev \
        libmcrypt-dev \
        libtool \
        libxslt-dev \
        bzip2-dev \
        make \
	&& docker-php-source extract \
    && pecl channel-update pecl.php.net \
    && pecl install $PECL_EXTENSIONS \
    && cd /usr/src/php/ext/ \
    && docker-php-ext-enable $(echo $PECL_EXTENSIONS | sed -E 's/\-[^ ]+//g') opcache \
    && docker-php-ext-install -j "$(nproc)" $PHP_EXTENSIONS \
    && pecl clear-cache \
# cleanup
	&& rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/*
CMD php-fpm