FROM php:8.1-fpm

# Copy existing application directory contents
COPY . /var/www

# Set working directory
WORKDIR /var/www

# Get nodejs
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    mariadb-client \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    nodejs

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo pdo_mysql

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Get file install project
COPY install.sh /usr/local/bin/entrypoint.sh

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www
RUN chmod +x /usr/local/bin/entrypoint.sh

# Change current user to www
USER www

# Expose port 9000 and start php server
EXPOSE 9000
CMD ["/usr/local/bin/entrypoint.sh"]
