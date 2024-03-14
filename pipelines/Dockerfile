FROM --platform=linux/amd64 centos:centos7

LABEL maintainer="Adrián Zúñiga"

# Installing utilities
RUN yum install -y unzip gettext epel-release yum-utils git

# Install SSH
RUN yum install -y openssh-server openssh-clients

# Installing Node, NPM & packages
RUN yum install -y nodejs npm
RUN npm install -g pm2 grunt-cli n
RUN n 16.20.2
RUN npm install -g npm@8.19.4

# Installing JSCS and JSHint
RUN npm install -g jscs
RUN npm install -g jshint

# Installing PHP 7.4
RUN yum install -y \
	https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
	http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN	yum-config-manager --enable remi-php74
RUN yum install -y php \
	php-pecl-redis \
	php-fpm \
	php-gd \
	php-pecl-gearman \
	php-mbstring \
	php-pecl-yaml \
	php-mcrypt \
	php-mysql \
	php-pdo \
	php-opcache \
	php-pecl-xdebug \
	php-xml \
	php-zip

RUN sed -ie "s/;date.timezone =/date.timezone = America\/Mexico_City/" /etc/php.ini && \
	sed -ie "s/upload_max_filesize = 2M/upload_max_filesize = 5M/" /etc/php.ini

# Installing Composer
ENV PATH "/composer/vendor/bin:$PATH"

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV COMPOSER_VERSION 2.1.1

RUN curl -s -f -L -o /tmp/installer.php https://raw.githubusercontent.com/composer/getcomposer.org/da290238de6d63faace0343efbdd5aa9354332c5/web/installer \
 && php -r " \
    \$signature = '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410'; \
    \$hash = hash('SHA384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
        unlink('/tmp/installer.php'); \
        echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
        exit(1); \
    }" \
 && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} \
 && rm /tmp/installer.php \
 && composer --ansi --version --no-interaction

# Installing Allman CodeSniffer
RUN composer global require linnk/allman-codesniffer:dev-master

RUN phpcs --config-set installed_paths /composer/vendor/linnk/allman-codesniffer/
RUN phpcs --config-set default_standard Allman
RUN phpcs --config-set report_width auto
RUN phpcs --config-set show_progress 1
RUN phpcs --config-set colors 1

# Clearing
RUN rm -rf /var/cache/yum/*

# Setting develop enviroment
ENV ENV_NODE development

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

CMD ["node"]

################################# Uncomment the following to test OS in Docker #################################
# CMD ["/usr/sbin/init"]
