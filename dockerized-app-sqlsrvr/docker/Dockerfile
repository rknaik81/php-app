FROM php:7.2-apache

RUN apt-get update -yqq \
  && apt-get install -yqq --no-install-recommends \
      git \
          zip \
	      unzip \
	        && rm -rf /var/lib/apt/lists

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version


#Ubuntu 16.04
RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list


RUN exit
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
RUN ACCEPT_EULA=Y apt-get install mssql-tools
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
RUN source ~/.bashrc
# optional: for unixODBC development headers
RUN apt-get install unixodbc-dev

		# Enable PHP extensions
		#RUN docker-php-ext-install pdo_mysql mysqli
                RUN pecl install sqlsrv pdo_sqlsrv
		
                # Install composer
		RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

		# Add cake and composer command to system path
		ENV PATH="${PATH}:/var/www/html/lib/Cake/Console"
		ENV PATH="${PATH}:/var/www/html/app/Vendor/bin"

		# COPY apache site.conf file
		COPY ./docker/apache/site.conf /etc/apache2/sites-available/000-default.conf

		# Copy the source code into /var/www/html/ inside the image
		COPY . .

		# Set default working directory
		WORKDIR ./app
