#
# php-app
#

FROM ubuntu:16.04
MAINTAINER KUBO Atsuhiro <kubo@iteman.jp>

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y apt-utils
RUN apt-get install -y locales curl software-properties-common python-software-properties git 
RUN locale-gen en_US.UTF-8
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
RUN apt-get update
RUN apt-get install -y supervisor
RUN apt-get install -y apache2
RUN apt-get install -y mysql-client-core-5.7
RUN apt-get install -y memcached
RUN apt-get install -y imagemagick
RUN apt-get install -y libapache2-mod-php7.1 php7.1-bcmath php7.1-bz2 php7.1-cli php7.1-common php7.1-curl php7.1-dba php7.1-gd php7.1-gmp php7.1-imap php7.1-intl php7.1-ldap php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-odbc php7.1-pgsql php7.1-recode php7.1-snmp php7.1-soap php7.1-sqlite php7.1-tidy php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip php7.1-sqlite3
RUN apt-get install -y php-gnupg php-imagick php-streams php-fxsl
RUN apt-get install -y git
RUN apt-get install -y less vim-tiny
RUN apt-get install -y apg
RUN apt-get install -y sudo
RUN apt-get install -y unzip
RUN apt-get autoremove --purge -y
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Apache2
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APP_DOCUMENT_ROOT /var/www
EXPOSE 80
ADD apache2/app.conf /etc/apache2/sites-available/000-default.conf
ADD apache2/apache2 /usr/local/sbin/apache2
RUN chmod 755 /usr/local/sbin/apache2
RUN a2enmod rewrite
RUN sed -i "s/^\\( *export \+LANG.*\\)/#\\1/" /etc/apache2/envvars

# PHP7
ENV PHP_INI ""
ENV XDEBUG_REMOTE_PORT ""
ADD php7/app.ini /etc/php/7.1/apache2/conf.d/90-app.ini
ADD php7/app.ini /etc/php/7.1/cli/conf.d/90-app.ini
ADD php7/timezone.sh /usr/local/sbin/php7-timezone.sh

# PHP application
ADD app/init /usr/local/sbin/app-init
RUN chmod 755 /usr/local/sbin/app-init
ADD app/composer /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer
RUN chown root.root /usr/local/bin/composer

# System
ENV TZ ""
ENV LANG ""
ADD system/init /usr/local/sbin/system-init
RUN chmod 755 /usr/local/sbin/system-init
ADD system/locale.sh /usr/local/sbin/system-locale.sh
ADD system/timezone.sh /usr/local/sbin/system-timezone.sh

# Others
ENV HOME /root
WORKDIR /root
RUN mkdir /var/app
RUN echo "This file is a placeholder to expose /var/app directory after creating a new container from Kitematic." > /var/app/.placeholder
VOLUME /var/app

# Command
CMD ["/usr/local/sbin/system-init"]
