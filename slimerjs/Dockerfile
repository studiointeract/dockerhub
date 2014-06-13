# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# SlimerJS
#
# Test
#   docker run cmfatih/slimerjs /usr/bin/slimerjs -v
#   docker run cmfatih/slimerjs /usr/bin/casperjs | head -n 1
#   docker run -v `pwd`:/mnt/test cmfatih/slimerjs /usr/bin/casperjs /mnt/test/test.js

# VERSION 0.1.1

FROM ubuntu:12.04

MAINTAINER cmfatih

# Env
ENV SLIMERJS_VERSION_F 0.9.1

# Update package repository
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe multiverse" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y vim git wget xvfb libxrender-dev libasound2 libdbus-glib-1-2 libgtk2.0-0

# Required directories
RUN mkdir -p /srv/var

# SlimerJS
RUN wget -O /tmp/slimerjs-$SLIMERJS_VERSION_F-linux-x86_64.tar.bz2 http://download.slimerjs.org/releases/$SLIMERJS_VERSION_F/slimerjs-$SLIMERJS_VERSION_F-linux-x86_64.tar.bz2
RUN tar -xjf /tmp/slimerjs-$SLIMERJS_VERSION_F-linux-x86_64.tar.bz2 -C /tmp
RUN rm -f /tmp/slimerjs-$SLIMERJS_VERSION_F-linux-x86_64.tar.bz2
RUN mv /tmp/slimerjs-$SLIMERJS_VERSION_F/ /srv/var/slimerjs
RUN echo '#!/bin/bash\nxvfb-run /srv/var/slimerjs/slimerjs $*' > /srv/var/slimerjs/slimerjs.sh
RUN chmod 755 /srv/var/slimerjs/slimerjs.sh
RUN ln -s /srv/var/slimerjs/slimerjs.sh /usr/bin/slimerjs

# CasperJS
RUN git clone https://github.com/n1k0/casperjs.git /srv/var/casperjs
RUN echo '#!/bin/bash\n/srv/var/casperjs/bin/casperjs --engine=slimerjs $*' >> /srv/var/casperjs/casperjs.sh
RUN chmod 755 /srv/var/casperjs/casperjs.sh
RUN ln -s /srv/var/casperjs/casperjs.sh /usr/bin/casperjs

# Removed unnecessary packages
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all
