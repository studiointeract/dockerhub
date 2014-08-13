# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# v8
#
# Test
#   docker run cmfatih/v8 /srv/var/v8/d8 -v
#   docker run cmfatih/v8 /srv/var/v8/d8 --help
#   docker run -v `pwd`:/mnt/test cmfatih/v8 /srv/var/v8/d8 /mnt/test/test.js
#   docker run -v `pwd`:/mnt/test cmfatih/v8 /srv/var/v8/d8 --print_code /mnt/test/test.js

# VERSION 1.0.1

FROM ubuntu:12.04

MAINTAINER cmfatih

# Commands
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y vim git git-svn subversion make g++ libreadline-dev && \
    mkdir -p /srv/var/v8 && \
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git /usr/local/depot_tools && \
    export PATH=/usr/local/depot_tools:$PATH && \
    git clone https://github.com/v8/v8.git --branch bleeding_edge /usr/local/v8 && \
    cd /usr/local/v8 && \ 
    make builddeps && \
    make x64.release -j2 console=readline disassembler=on gdbjit=on liveobjectlist=on && \
    mkdir -p /srv/var/v8 && \
    cp -r /usr/local/v8/out/x64.release/* /srv/var/v8 && \
    cd /srv/var/v8 && \
    rm -rf /usr/local/v8 && \
    rm -rf /usr/local/depot_tools && \
    echo "export PATH=$PATH:/srv/var/v8/" >> /root/.bashrc && \
    cp -f /root/.bashrc /.bashrc && \
    apt-get purge -y vim git git-svn subversion make g++ libreadline-dev && \
    apt-get autoremove -y && \
    apt-get clean all

# Env
ENV PATH $PATH:/srv/var/v8/

# Default command
CMD ["/srv/var/v8/d8"]