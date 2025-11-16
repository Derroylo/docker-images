# Build arguments for flexibility
ARG PHP_VERSION=8.2
ARG XDEBUG_VERSION=3.2.1

FROM php:${PHP_VERSION}-cli-alpine

# Install xdebug and clean up in a single layer to reduce image size
RUN pecl install xdebug-${XDEBUG_VERSION} \
	&& docker-php-ext-enable xdebug \
	&& rm -rf /tmp/pear
