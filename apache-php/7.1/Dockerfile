FROM library/centos:centos6

MAINTAINER pica9

# add additional repos
RUN rpm -Uvh http://archives.fedoraproject.org/pub/archive/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
RUN curl -sL https://rpm.nodesource.com/setup_8.x | bash -
RUN rpm -Uvh http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm

# install apache, php5
RUN sed -i \
    's/#baseurl=http:\/\/mirror\.centos\.org\/centos\/\$releasever/baseurl=http:\/\/vault\.centos\.org\/6\.10/g' \
    /etc/yum.repos.d/CentOS-Base.repo
RUN yum install -y --enablerepo=remi --enablerepo=remi-php71 \
    httpd \
    gcc \
    php \
    php-common \
    php-bcmath \
    php-cli \
    php-pear \
    php-pdo \
    php-pgsql \
    php-gd \
    php-mbstring \
    php-mcrypt \
    php-xml \
    php-devel \
    php-soap \
    php-intl \
    php-redis \
    php-zip \
    zlib-devel \
    curl \
    libcurl-devel \
    git \
    java-1.7.0-openjdk \
    nodejs \
    mod_ssl \
    tar \
    libssh2 \
    libssh2-devel \
    make \
    wget \
    unzip \
    python-pip \
    awscli \
    perl-Test-Simple \
    perl-JSON \
    perl-CPAN \
    nginx \
    php71-php-fpm \
    php71-php-pgsql

RUN rm /etc/nginx/conf.d/default.conf;

RUN cpan YAML::XS

RUN pip install awscli==1.16.314

RUN mkdir -p /etc/httpd/ssl

RUN wget https://www.libssh2.org/download/libssh2-1.8.0.tar.gz
RUN tar -xpzf libssh2-1.8.0.tar.gz
RUN cd libssh2-1.8.0 && ./configure --prefix=/usr &&  make && make install

#install ssh2
RUN wget http://pecl.php.net/get/ssh2-1.1.1.tgz
RUN tar -zxf ssh2-1.1.1.tgz
RUN cd ssh2-1.1.1 && phpize && ./configure && make && make install
RUN echo "extension=ssh2.so" > /etc/php.d/20-ssh2.ini

RUN ldconfig

RUN npm install -g bower

#install composer
RUN curl -sS https://getcomposer.org/installer | php -- --version=1.4.1 \
    && mv composer.phar /usr/bin/composer

RUN cd ~ \
  && curl -O https://mirrors.kernel.org/gnu/bash/bash-5.1.tar.gz \
  && tar zxvf bash-5.1.tar.gz \
  && cd bash-5.1 \
  && ./configure --prefix=/opt \
  && make install \
  && rm ~/bash-5.1.tar.gz

