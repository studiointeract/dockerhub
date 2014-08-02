# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# Node.js
#
# Test
#   docker run cmfatih/nodejs /usr/bin/node -v
#   docker run cmfatih/nodejs /usr/bin/node -p 'console.log("dockerized!");process.exit(0);'
#   docker run -v `pwd`:/mnt/test cmfatih/nodejs /usr/bin/node /mnt/test/test.js

# VERSION 0.3.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Update package repository
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y wget apt-transport-https software-properties-common python python-software-properties g++ make

# Init node.js
RUN wget -qO- https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
RUN echo 'deb https://deb.nodesource.com/node precise main' > /etc/apt/sources.list.d/nodesource.list
RUN echo 'deb-src https://deb.nodesource.com/node precise main' >> /etc/apt/sources.list.d/nodesource.list
RUN apt-get update
RUN apt-get install -y nodejs

# Removed unnecessary packages
RUN apt-get purge -y wget apt-transport-https software-properties-common python python-software-properties g++ make
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all