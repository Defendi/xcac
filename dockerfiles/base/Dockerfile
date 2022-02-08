FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive
ENV TERM=xterm
# Avoid ERROR: invoke-rc.d: policy-rc.d denied execution of start.
RUN echo "#!/bin/sh\nexit 0" > /usr/sbin/policy-rc.d

ADD conf/apt-requirements /opt/sources/
ADD conf/pip-requirements /opt/sources/

RUN apt update && apt upgrade

WORKDIR /opt/sources/

RUN apt-get install -y --no-install-recommends $(grep -v '^#' apt-requirements)

# Set the locale
RUN sed -i '/pt_BR.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG pt_BR.UTF-8
ENV LANGUAGE pt_BR:pt
ENV LC_ALL pt_BR.UTF-8

RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt update && apt install --fix-missing

RUN apt install -y python3.8
RUN apt install -y python3-pip
RUN apt install -y python3-dev

##### Latest Postgresql-client
RUN apt install -y gpg-agent

# install latest postgresql-client
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main" | tee  /etc/apt/sources.list.d/pgdg.list
RUN apt-get update -qq && apt-get install -y postgresql-client-12

##### Instalacao dos requerimentos python

RUN pip3 install --no-cache-dir -r pip-requirements

##### Limpeza da Instalação #####

RUN apt-get autoremove -y && \
    apt-get autoclean



