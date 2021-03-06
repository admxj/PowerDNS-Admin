FROM ubuntu:16.04
MAINTAINER Khanh Ngo "k@ndk.name"
ARG ENVIRONMENT=development
ENV ENVIRONMENT=${ENVIRONMENT}
WORKDIR /powerdns-admin

RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN  apt-get clean

RUN apt-get update -y
RUN apt-get install -y apt-transport-https

RUN apt-get install -y locales locales-all
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

RUN apt-get install -y python3-pip python3-dev supervisor curl mysql-client

# Install node 10.x
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install -y nodejs

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list

# Install yarn
RUN apt-get update -y
RUN apt-get install -y yarn

# Install Netcat for DB healthcheck
RUN apt-get install -y netcat

# lib for building mysql db driver
RUN apt-get install -y libmysqlclient-dev

# lib for building ldap and ssl-based application
RUN apt-get install -y libsasl2-dev libldap2-dev libssl-dev

# lib for building python3-saml
RUN apt-get install -y libxml2-dev libxslt1-dev libxmlsec1-dev libffi-dev pkg-config

COPY ./requirements.txt /powerdns-admin/requirements.txt
RUN pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

ADD ./supervisord.conf /etc/supervisord.conf
ADD . /powerdns-admin/
COPY ./configs/${ENVIRONMENT}.py /powerdns-admin/config.py
COPY ./docker/PowerDNS-Admin/entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
