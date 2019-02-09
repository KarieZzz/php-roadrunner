FROM golang:1.11 as builder

ENV GO111MODULE on
ENV RR_VERSION v1.3.0

RUN apt-get update && apt-get install -y --no-install-recommends git

# Build RoadRunner
WORKDIR /go/src
RUN git clone --depth 1 --branch ${RR_VERSION} https://github.com/spiral/roadrunner \
  && cd /go/src/roadrunner \
  && make \
  && make install

# PHP Image
FROM php:7.2-cli

# Copy RoadRunner binary
COPY --from=builder /go/src/roadrunner/rr /usr/local/bin/rr

# Install PHP Extensions
RUN docker-php-ext-install opcache \
  && docker-php-ext-enable opcache

# Install Composer
RUN curl -o composer-setup.php https://getcomposer.org/installer \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') !== trim(file_get_contents('https://composer.github.io/installer.sig'))) { unlink('composer-setup.php'); echo 'Composer installer corrupt' . PHP_EOL; exit(1); }" \
  && php composer-setup.php \
  && rm composer-setup.php \
  && mv composer.phar /usr/local/bin/composer

WORKDIR /var/www

ENTRYPOINT ["/usr/local/bin/rr", "serve", "-d", "-c", "/etc/roadrunner/.rr.yml"]
