FROM php:8.4-cli

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    zip \
    libzip-dev \
    libicu-dev \
    nodejs \
    npm \
    && docker-php-ext-install intl zip pdo pdo_mysql bcmath

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]
