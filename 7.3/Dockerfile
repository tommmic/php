FROM php:7.3-apache

RUN a2enmod rewrite

RUN apt-get update -y \
	&& apt-get install -y --no-install-recommends \
	libxml2-dev \
	git \
	zlib1g-dev \
	libjpeg-dev \
	libpng-dev \
	libmagickwand-dev \
	libmcrypt-dev \
	libssh2-1 \
	libssh2-1-dev \
	openssh-client \
	unzip \
	libzip-dev \
	webp \
	&& apt-get clean -y \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pecl install redis && docker-php-ext-enable redis

RUN cd /tmp && git clone https://git.php.net/repository/pecl/networking/ssh2.git && cd /tmp/ssh2 \
&& phpize && ./configure && make && make install \
&& echo "extension=ssh2.so" > /usr/local/etc/php/conf.d/ext-ssh2.ini \
&& rm -rf /tmp/ssh2

RUN docker-php-ext-configure gd --with-jpeg-dir=/usr/lib/x86_64-linux-gnu/
RUN docker-php-ext-install \
	pdo \
	pdo_mysql \
	mysqli \
	opcache \
	bcmath \
	calendar \
	soap \
	zip \
	gd

RUN yes | pecl install imagick
RUN docker-php-ext-enable \
	imagick

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
ENV COMPOSER_ALLOW_SUPERUSER 1
