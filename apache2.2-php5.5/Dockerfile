FROM centos:centos6.9

# Maintener
MAINTAINER Gladys Ronquillo <gladysronquillo@outlook.com>

# update & clean
RUN yum update -y; yum clean all;

# Install varioius utilities
RUN yum -y install curl wget nano unzip 7za git vim rpm \
yum-utils which epel-release

# Set root password
RUN echo root:docker | chpasswd && yum install -y passwd

# Install Apache & EXIM
RUN yum -y install httpd mod_ssl

# PHP5.5 install & more apps
RUN rpm --import http://ftp.iij.ad.jp/pub/linux/fedora/epel/RPM-GPG-KEY-EPEL-6 \
&& rpm --import http://rpms.remirepo.net/RPM-GPG-KEY-remi \
&& rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm \
&& yum-config-manager --enable remi-php55 \
&& yum clean all \
&& yum -y install php php-mcrypt php-gd php-curl php-mysql php-pdo php-mbstring php-pecl-xdebug php-xml php-common php-bcmath \
&& yum -y update && echo '<?php phpinfo(); ?>' > /var/www/html/phpinfo.php \
&& yum clean all

# Reconfigure Apache
RUN sed -i 's/AllowOverride None/AllowOverride All/g' /etc/httpd/conf/httpd.conf \
&& sed -i 's/AddDefaultCharset UTF-8/#AddDefaultCharset UTF-8/g' /etc/httpd/conf/httpd.conf \
&& chown root:apache /var/www/html \
&& chmod g+s /var/www/html \
&& chkconfig httpd on

# Config php
RUN sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 512M/g' /etc/php.ini \
&& sed -i 's/post_max_size = 8M/post_max_size = 512M/g' /etc/php.ini \
&& sed -i 's/memory_limit = 128M/memory_limit = 512M/g' /etc/php.ini

# Timezone & Networking
RUN cp -p /usr/share/zoneinfo/America/Guayaquil /etc/localtime \
    && echo "NETWORKING=yes" > /etc/sysconfig/network 

# data.timezone
RUN sed -ri 's/;date.timezone =/date.timezone = America\/Guayaquil/g' /etc/php.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

EXPOSE 22 80 443

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
