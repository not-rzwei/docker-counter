# Build image
ARG PHP_IMAGE
FROM composer:2.0.11 as build

COPY composer.json composer.lock ./
RUN composer install -vvv --no-dev --ignore-platform-reqs --no-scripts 

COPY . .
RUN composer dump-autoload \
  && php artisan route:cache \
  && php artisan view:cache \
  && chown -R www-data:www-data bootstrap/cache storage \
  && mv infrastructure/php/release/entrypoint.sh /usr/local/bin \
  && rm -rf infrastructure

# Final image
ARG PHP_IMAGE
FROM $PHP_IMAGE
FROM jakmall/recruitment/cloud-engineer/counter/php-fpm

COPY --from=build /usr/local/bin/entrypoint.sh /usr/local/bin
COPY --from=build /app/ /var/www/html

ENTRYPOINT ["entrypoint.sh"]