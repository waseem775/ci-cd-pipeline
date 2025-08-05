# Stage 1: Build dependencies
FROM composer:latest AS build

WORKDIR /app

# Copy everything
COPY . .

# Install dependencies
RUN composer install --no-dev --no-interaction --prefer-dist --optimize-autoloader

# Stage 2: Runtime
FROM php:8.2-cli

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    zip \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring

# Set working dir
WORKDIR /var/www

# Copy app from build stage
COPY --from=build /app /var/www

# Expose port Laravel will listen on
EXPOSE 8000

# Start Laravel's dev server (production needs real NGINX)
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
