FROM webdevops/php-nginx:alpine-php7

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN composer global require "hirak/prestissimo:^0.3" --prefer-dist --no-progress --no-suggest --optimize-autoloader --classmap-authoritative \
	&& composer clear-cache

COPY  ./docker/nginx/ /opt/docker/etc/nginx/

WORKDIR /var/www/html/src

COPY composer.json ./
COPY composer.lock ./

COPY config config/
COPY bin bin/
COPY src src/
COPY templates templates/
COPY public public/

#COPY src/app/templates templates
#COPY src/app/translations translations


RUN mkdir -p \
		var/cache \
		var/logs \
		var/sessions \
	&& composer install --prefer-dist --no-dev --no-autoloader --no-scripts --no-progress --no-suggest \
	&& composer clear-cache \
	&& chown -R application var

RUN composer dump-autoload --optimize --classmap-authoritative --no-dev