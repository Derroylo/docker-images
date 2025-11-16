# Build arguments for flexibility
ARG PHP_VERSION=8.3

FROM php:${PHP_VERSION}-cli-alpine

# Re-declare ARG after FROM to make it available in build steps
ARG XDEBUG_VERSION=3.4.0

# Install xdebug and clean up in a single layer to reduce image size
# $PHPIZE_DEPS includes build tools needed for PECL extensions (autoconf, g++, make, etc.)
# linux-headers is required for newer xdebug versions (3.4+)
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS linux-headers \
	&& pecl install xdebug-${XDEBUG_VERSION} \
	&& docker-php-ext-enable xdebug \
	&& apk del .build-deps \
	&& rm -rf /tmp/pear
