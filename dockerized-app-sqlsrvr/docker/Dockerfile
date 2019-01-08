FROM php:7.2-apache
# Microsoft SQL Server Prerequisites
RUN apt-get update \
    && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/9/prod.list \
    > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get install -y --no-install-recommends \
    locales \
    apt-transport-https \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen \
    && apt-get update \
    && apt-get -y --no-install-recommends install \
    unixodbc-dev \
    msodbcsql17

    RUN docker-php-ext-install mbstring pdo pdo_mysql \
        && pecl install sqlsrv pdo_sqlsrv xdebug \
        && docker-php-ext-enable sqlsrv pdo_sqlsrv xdebug

    # Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

		# Add cake and composer command to system path
		ENV PATH="${PATH}:/var/www/html/lib/Cake/Console"
		ENV PATH="${PATH}:/var/www/html/app/Vendor/bin"

RUN echo "<?php phpinfo();" > index.php
		# COPY apache site.conf file
		COPY ./docker/apache/site.conf /etc/apache2/sites-available/000-default.conf

		# Copy the source code into /var/www/html/ inside the image
		COPY . .

		# Set default working directory
		WORKDIR ./app
