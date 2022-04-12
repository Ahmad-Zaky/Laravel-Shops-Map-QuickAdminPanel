FROM php:8.0-apache
WORKDIR /var/www/html

RUN apt-get update -y && apt-get install -y sendmail libpng-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libonig-dev \
        && docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg \
        && docker-php-ext-install gd
 
RUN docker-php-ext-install mbstring
RUN apt-get install -y zip libzip-dev \
  && docker-php-ext-configure zip \
  && docker-php-ext-install zip

RUN apt-get -yqq update
RUN apt-get -yqq install exiftool
RUN docker-php-ext-configure exif
RUN docker-php-ext-install exif
RUN docker-php-ext-enable exif

RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN docker-php-ext-enable mysqli pdo pdo_mysql
RUN apt-get update && apt-get install -y curl zip unzip
RUN mkdir /app
COPY .docker/vhost.conf /etc/apache2/sites-available/000-default.conf
RUN touch /usr/local/etc/php/conf.d/uploads.ini \
    && echo "upload_max_filesize = 2024M" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "post_max_size = 2024M" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "memory_limit = 2024M" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "max_execution_time = 30000" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "max_input_time = 30000" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "max_file_uploads = 1000" >> /usr/local/etc/php/conf.d/uploads.ini
RUN chown -R www-data:www-data /app && a2enmod rewrite
# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer




