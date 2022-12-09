FROM ubuntu:20.04

### Prepare the system
ENV DEBIAN_FRONTEND=noninteractive
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN apt-get clean
RUN apt-get update && apt-get upgrade -y
### End Prepare the system


### Set the locale
RUN apt update
RUN apt-get install -y locales
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8
### End Set the locale


### Install apt dependencies
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
COPY apt-packages.txt /packages.txt
RUN apt update && xargs -a /packages.txt apt-get -y install 
RUN apt update
### End apt dependencies


### Install Docker
RUN apt-get update 
RUN apt-get install -y \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update
RUN apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
### End Install Docker


### Install PHP and Plugins
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:ondrej/php

RUN apt-get update
COPY php-packages.txt /php-packages.txt
RUN apt update && xargs -a /php-packages.txt apt-get -y install 
### End Install PHP and Plugins


### Install Node and packages via NVM 
SHELL ["/bin/bash", "--login", "-i", "-c"]

ENV NODE_VERSION=16.17.0
RUN apt install -y curl
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
RUN node --version
RUN npm --version

COPY npm-packages.txt /npm-packages.txt
RUN xargs -a /npm-packages.txt npm install -g 
### End Install Node and packages via NVM


### Install Composer and packages
RUN curl -s http://getcomposer.org/installer | php && \
  echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
  mv composer.phar /usr/local/bin/composer

RUN composer global require laravel/vapor-cli squizlabs/php_codesniffer
### End Install Composer and packages

### Cleanup
RUN apt-get clean
RUN rm -rf /etc/nginx/sites-enabled
### End Cleanup