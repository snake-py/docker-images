FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

RUN rm /bin/sh && ln -s /bin/bash /bin/sh


RUN apt-get clean
RUN apt-get update
RUN apt-get install -y --no-install-recommends apt-utils
RUN apt-get -y install systemctl
RUN apt update
RUN apt -y install systemctl

# Set the locale
RUN apt-get install -y locales
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8

# Install Dev tools
RUN apt-get install -y \
    git \
    curl \
    vim \
    nano


# Add the "PHP 7" ppa
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:ondrej/php

RUN apt-get update
RUN apt-get install -y \
    php7.4 \
    php7.4-cli \
    php7.4-fpm \
    php7.4-common \
    php7.4-curl \
    php7.4-json \
    php7.4-xml \
    php7.4-mbstring \
    php7.4-mcrypt \
    php7.4-mysql \
    php7.4-zip \
    php7.4-memcached \
    php7.4-gd \
    php7.4-dev \
    php7.4-soap \
    pkg-config \
    libcurl4-openssl-dev \
    libedit-dev \
    libssl-dev \
    libxml2-dev \
    php7.4-xdebug \
    php7.4-redis 

RUN apt-get install -y supervisor
RUN apt-get install htop -y


# install nvm
# Install Supervisord
RUN apt-get install supervisor -y
RUN apt-get install htop -y

SHELL ["/bin/bash", "--login", "-i", "-c"]

# ENV NVM_DIR /usr/local/nvm 
ENV NODE_VERSION=14.17.5
RUN apt install -y curl
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
RUN node --version
RUN npm --version

# Install composer
RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

# Cleanup
RUN apt-get clean
RUN rm -rf /etc/nginx/sites-enabled
