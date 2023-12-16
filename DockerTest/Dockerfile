# Use a PHP image with version 8.1 or newer
FROM php:8.1-fpm

# Set the working directory inside the container
WORKDIR /var/www/html

# Install necessary dependencies
RUN apt-get update && \
    apt-get install -y \
        git \
        unzip \
        libzip-dev \
    && docker-php-ext-install pdo_mysql zip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Output Composer diagnostic information
RUN composer diagnose

# Copy the local application files to the container
COPY . .

# Set permissions
RUN chown -R www-data:www-data /var/www/html
RUN chmod -R 755 /var/www/html/storage /var/www/html/bootstrap/cache

# Update Composer
RUN composer self-update

# Run specific Composer commands for more information
RUN composer validate
RUN composer check-platform-reqs

# Install Composer dependencies
RUN composer install --no-interaction --no-progress --no-suggest

# Set the default command to run the Laravel development server
CMD php artisan serve --host=0.0.0.0 --port=8000
