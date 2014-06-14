# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# Node.js
#
# Test
#   docker run cmfatih/nodejs /usr/bin/node -v
#   docker run cmfatih/nodejs /usr/bin/node -p 'console.log("dockerized!");process.exit(0);'
#   docker run -v `pwd`:/mnt/test cmfatih/nodejs /usr/bin/node /mnt/test/test.js

# VERSION 0.2.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Update package repository
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y software-properties-common python python-software-properties g++ make

# Init node.js
RUN add-apt-repository -y ppa:chris-lea/node.js
RUN apt-get update
RUN apt-get install -y nodejs

# Removed unnecessary packages
RUN apt-get purge -y software-properties-common python python-software-properties g++ make
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all