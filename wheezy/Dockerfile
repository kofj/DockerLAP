FROM debian:wheezy

MAINTAINER Frank Kung,"kfanjian@gmail.com"


#Update source list and install package
RUN echo "deb http://mirrors.163.com/debian/ wheezy contrib main non-free"> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install -y libxml2 pkg-config apache2 apache2.2-common php5-mysql php5 php-pear \
	&& apt-get clean && rm -r /var/lib/apt/lists/*
RUN a2enmod rewrite \
	&& rm -rf /var/www/html \
	&& mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html \
	&& chown -R www-data:www-data /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html

COPY apache2.conf /etc/apache2/apache2.conf

ENV APACHE_RUN_USER www-data

ENV APACHE_RUN_GROUP www-data

#WORKDIR /var/www/html

EXPOSE 80
CMD ["apache2", "-DFOREGROUND"]
