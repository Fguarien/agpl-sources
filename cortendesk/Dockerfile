# cortendesk 1.14.0 + PHP 8.4.27 (CVE-2026-109618 / CVE-2026-109619, ticket 2130).
# L'amont (ghcr.io/marcpope/cortendesk) n'a pas republié ; on garde SON image, épinglée, et on n'y
# remplace que PHP : binaires php/php-fpm d'un 8.4.27 construit depuis la recette docker-library
# (php-8.4.27/), et les 6 extensions partagées RECOMPILÉES contre ce 8.4.27 (même ABI 20240924,
# mais on ne parie pas dessus). À retirer dès que l'amont publie une image en >= 8.4.27.
# Build (vm-compute-01) :
#   docker build -t php-local:8.4.27-fpm-alpine3.24 php-8.4.27/
#   docker build -t cortendesk-local:1.14.0-php8.4.27 .
FROM php-local:8.4.27-fpm-alpine3.24 AS php
RUN apk add --no-cache --virtual .deps $PHPIZE_DEPS icu-dev libsodium-dev \
 && docker-php-ext-install -j"$(nproc)" bcmath ftp intl opcache pdo_mysql sodium \
 && php -v | head -n 1

FROM ghcr.io/marcpope/cortendesk@sha256:3305ea4bca8814e71f6c0dcf49df449cf44f82be4023a0c98e15fdc2ef919545
COPY --from=php /usr/local/bin/php /usr/local/bin/php
COPY --from=php /usr/local/sbin/php-fpm /usr/local/sbin/php-fpm
COPY --from=php /usr/local/lib/php/extensions/no-debug-non-zts-20240924/ /usr/local/lib/php/extensions/no-debug-non-zts-20240924/
RUN php -v | grep -q '^PHP 8\.4\.27 ' && php-fpm -t
