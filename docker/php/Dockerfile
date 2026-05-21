# =========================
# NODE BUILD STAGE
# =========================
FROM node:24-alpine AS node_builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# =========================
# PHP STAGE
# =========================
FROM php:8.4-fpm-alpine

WORKDIR /var/www

RUN apk add --no-cache \
    bash \
    curl \
    git \
    unzip \
    icu-dev \
    oniguruma-dev \
    libzip-dev \
    zip

RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    intl \
    mbstring \
    zip \
    bcmath \
    opcache

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY . .

COPY --from=node_builder /app/public/build ./public/build

RUN composer install \
    --no-dev \
    --optimize-autoloader

RUN chown -R www-data:www-data storage bootstrap/cache

EXPOSE 9000

CMD ["php-fpm"]
