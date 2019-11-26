FROM library/centos:centos6

RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
RUN rpm -Uvh http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm
RUN yum install -y --enablerepo=remi --enablerepo=remi-php70 php php-xml php-zip git
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/bin/composer

ARG SSH_PRIVATE_KEY
RUN mkdir /root/.ssh/
RUN echo "${SSH_PRIVATE_KEY}" > /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts
RUN git clone git@github.com:Pica9/composer.git
RUN rm -rf /root/.ssh/
RUN cd composer && composer require hirak/prestissimo
RUN cd composer && composer install



FROM library/centos:centos6

COPY --from=0 /composer /composer
RUN ln -s /composer/bin/composer /usr/bin/composer

MAINTAINER pica9

# add additional repos
RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
run rpm -Uvh http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm
RUN curl -sL https://rpm.nodesource.com/setup_8.x | bash -

# install apache, php5
RUN yum install -y --enablerepo=remi --enablerepo=remi-php70 \
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
    php-pecl-redis-3.1.6-1.el6.remi.7.0.x86_64 \
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
    unzip

RUN mkdir -p /etc/httpd/ssl

RUN npm install -g bower

#install ssh2
RUN wget https://codeload.github.com/Sean-Der/pecl-networking-ssh2/zip/php7
RUN unzip php7
RUN cd pecl-networking-ssh2-php7 && phpize && ./configure && make && make install
RUN echo "extension=ssh2.so" > /etc/php.d/20-ssh2.ini

RUN echo "memory_limit = 4096M" >> /etc/php.ini

