# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# Node.js
#
# Test
#   docker run cmfatih/nodejs /usr/bin/node -v
#   docker run cmfatih/nodejs /usr/bin/node -p 'console.log("dockerized!");process.exit(0);'
#   docker run -v `pwd`:/mnt/test cmfatih/nodejs /usr/bin/node /mnt/test/test.js

# VERSION 1.0.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Commands
RUN \
  apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y wget apt-transport-https software-properties-common python python-software-properties g++ make && \
  wget -qO- https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
  echo 'deb https://deb.nodesource.com/node precise main' > /etc/apt/sources.list.d/nodesource.list && \
  echo 'deb-src https://deb.nodesource.com/node precise main' >> /etc/apt/sources.list.d/nodesource.list && \
  apt-get update && \
  apt-get install -y nodejs && \
  apt-get purge -y wget apt-transport-https software-properties-common python python-software-properties g++ make && \
  apt-get autoremove -y && \
  apt-get clean all

# Default command
CMD ["/usr/bin/node"]