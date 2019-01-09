FROM php:7.2-apache

# apt-get and system utilities
RUN apt-get update && apt-get install -y \
    curl apt-utils apt-transport-https debconf-utils gcc build-essential gnupg \
        && rm -rf /var/lib/apt/lists/*

        # adding custom MS repository
        RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
        RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

        # install SQL Server drivers
        RUN apt-get update && ACCEPT_EULA=Y apt-get install -y unixodbc-dev msodbcsql 

        # install SQL Server tools
        RUN apt-get update && ACCEPT_EULA=Y apt-get install -y mssql-tools
        RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
        RUN /bin/bash -c "source ~/.bashrc"

        # install necessary locales
        RUN apt-get install -y locales \
                        && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
                            && locale-gen

        # install SQL Server PHP connector module 
        RUN pecl install sqlsrv pdo_sqlsrv
        RUN docker-php-ext-enable \
            sqlsrv \
            pdo_sqlsrv

                            # initial configuration of SQL Server PHP connector
        #RUN echo "extension=/usr/lib/php/20151012/sqlsrv.so" >> /etc/php/7.0/cli/php.ini
        #RUN echo "extension=/usr/lib/php/20151012/pdo_sqlsrv.so" >> /etc/php/7.0/cli/php.ini
    # Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

		# Add cake and composer command to system path
		ENV PATH="${PATH}:/var/www/html/lib/Cake/Console"
		ENV PATH="${PATH}:/var/www/html/app/Vendor/bin"

                #COPY ./docker/phpinfo.php /var/www/html/app
                #RUN ln -s /var/www/app/public/phpinfo.php /var/www/app/public/index.php

		# COPY apache site.conf file
		COPY ./docker/apache/site.conf /etc/apache2/sites-available/000-default.conf

		# Copy the source code into /var/www/html/ inside the image
		COPY . .

		# Set default working directory
		WORKDIR ./app
