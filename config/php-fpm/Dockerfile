FROM php:8-fpm-buster

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    iputils-ping \
    # sen email configuration
    zlib1g-dev \
    libzip-dev \
    redis-tools \
    sendmail \
    vim \
    libpq-dev
RUN echo "sendmail_path=/usr/sbin/sendmail -t -i" >> /usr/local/etc/php/conf.d/sendmail.ini 
RUN pecl install redis
RUN pecl install openswoole

# Install PHP extensions
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsq
RUN docker-php-ext-install  mbstring exif pcntl bcmath gd mysqli pdo_mysql zip pdo_pgsql pgsql intl
RUN docker-php-ext-enable redis pdo_pgsql openswoole






RUN sed -i '/#!\/bin\/sh/aservice sendmail restart' /usr/local/bin/docker-php-entrypoint

RUN sed -i '/#!\/bin\/sh/aecho "$(hostname -i)\t$(hostname) $(hostname).localhost" >> /etc/hosts' /usr/local/bin/docker-php-entrypoint



# And clean up the image

RUN rm -rf /var/lib/apt/lists/*


# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*


# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

USER $user
RUN whoami
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
ENV NVM_DIR=/home/$user/.nvm
RUN . $HOME/.nvm/nvm.sh && nvm install --lts
# Set working directory
WORKDIR /var/www
RUN source /home/$user/nvm.sh