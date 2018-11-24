FROM node:8

WORKDIR /var/www/html/dev/tools/gulp

ARG uid=999

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb http://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
  && apt-get update \
  && apt-get install -y git yarn \
  && apt-get -y autoremove \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
  && echo "deb http://ppa.launchpad.net/nginx/development/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_nginx_mainline.list \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C \
  && apt-get update \
  && apt-get install -y curl zip unzip git supervisor sqlite3


RUN apt-get install -y php7.0-cli \
  php7.0-pgsql php7.0-sqlite3 \
  php7.0-curl php7.0-mysql php7.0-mbstring \
  php7.0-mcrypt \
  php7.0-xml php7.0-zip php7.0-bcmath php7.0-soap

# RUN apt-cache search libicu
RUN apt-get install software-properties-common -y

RUN add-apt-repository -y "deb http://security.ubuntu.com/ubuntu xenial-security main"

RUN apt-get update

RUN apt-get install bzip2 libicu55 -y --force-yes

RUN apt-get install -y php7.0-intl

RUN apt-get install -y php7.0-readline php7.0-xdebug php7.0-imagick \
  php-msgpack php-igbinary

RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
  && mkdir /run/php \
  && apt-get -y autoremove \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#RUN apt-get update && apt-get install -y mysql-client && rm -rf /var/lib/apt

RUN usermod -u $uid node
