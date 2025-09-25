FROM debian:stable-slim

ARG USER_UID
ARG USER_GID

RUN groupadd -g ${USER_GID} flexdock && useradd -u ${USER_UID} -g flexdock -m flexdock && usermod -p "*" flexdock

RUN apt update && apt install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash
RUN apt-get update && apt-get install -y nodejs && apt-get install -y nano cron && apt-get clean

RUN npm install -g sequelize-cli
RUN apt install -y git

RUN rm -rf /var/www/html

WORKDIR /var/www
