FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN apt-get clean

# Set the locale
RUN apt update
RUN apt-get install -y locales
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8


# Install apt dependencies
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get -y install curl wget jq git nano vim unzip zip

# Install Node.js
ENV NODE_VERSION=18.19.0
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
RUN node --version
RUN npm --version

# install node packages
RUN npm install -g nodemon
RUN npm i -g cross-env


# install pnpm
RUN wget -qO- https://get.pnpm.io/install.sh | ENV="$HOME/.bashrc" SHELL="$(which bash)" bash -
