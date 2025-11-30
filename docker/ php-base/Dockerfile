# Base PHP-FPM image for MSA services
FROM php:8.2-fpm

# Install system dependencies and PHP extensions
RUN apt-get update && apt-get install -y \
    git unzip vim iputils-ping netcat-traditional \
    libzip-dev zlib1g-dev librdkafka-dev libssl-dev \
 && docker-php-ext-install pdo_mysql zip pcntl \
 && pecl install redis rdkafka \
 && docker-php-ext-enable redis rdkafka \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Composer from the official image
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Create non-root user (for running PHP-FPM safely)
ARG UID=1000
RUN adduser --disabled-password --gecos "" --uid ${UID} appuser \
 && mkdir -p /var/www/app \
 && chown -R appuser:appuser /var/www/app

# Switch to non-root user
USER appuser
WORKDIR /var/www/app

# Default command (FPM server)
CMD ["php-fpm"]
