# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# PhantomJS
#
# Test
#   docker run cmfatih/phantomjs /usr/bin/phantomjs -v
#   docker run cmfatih/phantomjs /usr/bin/casperjs --version
#   docker run -v `pwd`:/mnt/test cmfatih/phantomjs /usr/bin/casperjs /mnt/test/test.js

# VERSION 0.2.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Env
ENV PHANTOMJS_VERSION 1.9.7

# Update package repository
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y vim git wget libfreetype6 libfontconfig bzip2

# Required directories
RUN mkdir -p /srv/var

# PhantomJS
RUN wget --no-check-certificate -O /tmp/phantomjs-$PHANTOMJS_VERSION-linux-x86_64.tar.bz2 https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-$PHANTOMJS_VERSION-linux-x86_64.tar.bz2
RUN tar -xjf /tmp/phantomjs-$PHANTOMJS_VERSION-linux-x86_64.tar.bz2 -C /tmp
RUN rm -f /tmp/phantomjs-$PHANTOMJS_VERSION-linux-x86_64.tar.bz2
RUN mv /tmp/phantomjs-$PHANTOMJS_VERSION-linux-x86_64/ /srv/var/phantomjs
RUN ln -s /srv/var/phantomjs/bin/phantomjs /usr/bin/phantomjs

# CasperJS
RUN git clone https://github.com/n1k0/casperjs.git /srv/var/casperjs
RUN ln -s /srv/var/casperjs/bin/casperjs /usr/bin/casperjs

# Removed unnecessary packages
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all