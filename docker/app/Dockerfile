FROM ubuntu:16.04

RUN useradd -ms /bin/bash -u 1337 magento
WORKDIR /var/www/html

ENV GOSU_VERSION 1.7
RUN set -x \
  && apt-get update && apt-get install -y --no-install-recommends ca-certificates wget && rm -rf /var/lib/apt/lists/* \
  && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
  && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
  && export GNUPGHOME="$(mktemp -d)" \
  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
  && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
  && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
  && chmod +x /usr/local/bin/gosu \
  && gosu nobody true \
  && apt-get purge -y --auto-remove ca-certificates wget

RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
  && echo "deb http://ppa.launchpad.net/nginx/development/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_nginx_mainline.list \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C \
  && apt-get update \
  && apt-get install -y curl zip unzip git supervisor sqlite3 \
  && apt-get install -y nginx php7.0-fpm php7.0-cli \
  php7.0-pgsql php7.0-sqlite3 php7.0-gd \
  php7.0-curl php7.0-memcached \
  php7.0-imap php7.0-mysql php7.0-mbstring \
  php7.0-mcrypt \
  php7.0-xml php7.0-zip php7.0-bcmath php7.0-soap \
  php7.0-intl php7.0-readline php7.0-xdebug php7.0-imagick \
  php-msgpack php-igbinary  \
  && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
  && mkdir /run/php \
  && apt-get -y autoremove \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && echo "daemon off;" >> /etc/nginx/nginx.conf

RUN apt-get update && apt-get install -y mysql-client && rm -rf /var/lib/apt

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log

COPY h5bp /etc/nginx/h5bp
COPY magentonginx.conf /etc/nginx/
COPY default /etc/nginx/sites-available/default
COPY php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
COPY xdebug.ini /etc/php/7.0/mods-available/xdebug.ini
COPY crontab /etc/cron.d/magento

EXPOSE 80
EXPOSE 8080

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container

# Give execution rights on the cron job
RUN chmod a+x /etc/cron.d/magento

ENTRYPOINT ["start-container"]
