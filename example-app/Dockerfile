# Use PHP base image with CLI and extensions
# Testing cicd
FROM php:8.2-cli
# Install system dependencies
RUN apt-get update && apt-get install -y \
    git unzip curl libpng-dev libonig-dev libxml2-dev zip \
    && docker-php-ext-install pdo pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install Node.js + npm (for Vite/Tailwind etc)
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs

# Set working directory
WORKDIR /var/www/html
ENV APP_URL=https://laravel.magicsheet.store
ENV VITE_APP_URL=https://laravel.magicsheet.store
# Copy everything
COPY . .

# Install PHP dependencies
RUN composer install --no-interaction --optimize-autoloader --no-dev

# Install Node dependencies & build assets
RUN npm install && npm run build

# Expose port 8080
EXPOSE 8080

# Run Laravel server
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8080"]
