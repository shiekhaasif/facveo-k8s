# Use the official Ubuntu base image
FROM ubuntu:jammy
ENV TZ=Asia/Kolkata DEBIAN_FRONTEND=noninteractive

# Install Apache2 and update the package index
RUN apt-get update && apt-get install tzdata
RUN apt-get update && apt-get install software-properties-common -y
RUN add-apt-repository ppa:ondrej/apache2
RUN add-apt-repository ppa:ondrej/php
RUN apt update && apt install -y apache2 git wget curl unzip nano zip composer software-properties-common
RUN apt install -y php8.2 libapache2-mod-php8.2 php8.2-mysql php8.2-cli php8.2-common php8.2-fpm php8.2-soap php8.2-gd php8.2-opcache  php8.2-mbstring php8.2-zip php8.2-bcmath php8.2-intl php8.2-xml php8.2-curl php8.2-imap php8.2-ldap php8.2-gmp php8.2-redis libapache2-mod-php8.2 php8.2-memcached  &&  rm -r /var/lib/apt/lists/*
COPY ioncube_loader_lin_8.2.so /usr/lib/php/20220829
RUN sed -i '2 a zend_extension = "/usr/lib/php/20220829/ioncube_loader_lin_8.2.so"' /etc/php/8.2/apache2/php.ini
RUN sed -i '2 a zend_extension = "/usr/lib/php/20220829/ioncube_loader_lin_8.2.so"' /etc/php/8.2/cli/php.ini
RUN sed -i '2 a zend_extension = "/usr/lib/php/20220829/ioncube_loader_lin_8.2.so"' /etc/php/8.2/fpm/php.ini
RUN apt update && apt-get -y install wkhtmltopdf
RUN apt-get clean
RUN sed -i -e 's/^file_uploads = .*/file_uploads = On/' \
           -e 's/^allow_url_fopen = .*/allow_url_fopen = On/' \
           -e 's/^short_open_tag = .*/short_open_tag = On/' \
           -e 's/^memory_limit = .*/memory_limit = 256M/' \
           -e 's/^cgi.fix_pathinfo = .*/cgi.fix_pathinfo = 0/' \
           -e 's/^upload_max_filesize = .*/upload_max_filesize = 100M/' \
           -e 's/^post_max_size = .*/post_max_size = 100M/' \
           -e 's/^max_execution_time = .*/max_execution_time = 360/' /etc/php/8.2/fpm/php.ini

WORKDIR /var/www/html
RUN a2enmod proxy_fcgi setenvif
RUN a2enconf php8.2-fpm
RUN service php8.2-fpm restart && service apache2 restart
RUN a2enmod rewrite
RUN a2enmod ssl

# Expose ports 80 and 443 for HTTP and HTTPS traffic
EXPOSE 80
EXPOSE 443

CMD /bin/sh -c "service php8.2-fpm start && apache2ctl -D FOREGROUND"