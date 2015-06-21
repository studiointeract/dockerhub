# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# elasticsearch
#
# Usage
#   docker run cmfatih/elasticsearch elasticsearch -v
#   docker run -p 9200:9200 -p 9300:9300 cmfatih/elasticsearch
#   docker run -p 9200:9200 -p 9300:9300 -v `pwd`:/mnt/data cmfatih/elasticsearch

# VERSION 0.1.1

FROM ubuntu:12.04

MAINTAINER cmfatih

# Commands
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y vim wget software-properties-common python-software-properties && \
    add-apt-repository -y ppa:webupd8team/java && \
    apt-get update && \
    echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && \
    apt-get install -y oracle-java7-installer && \
    wget -qO - http://packages.elasticsearch.org/GPG-KEY-elasticsearch | apt-key add - && \
    echo 'deb http://packages.elasticsearch.org/elasticsearch/1.3/debian stable main' > /etc/apt/sources.list.d/elasticsearch.list && \
    apt-get update && \
    apt-get install -y elasticsearch && \
    mkdir -p /usr/share/elasticsearch/config && \
    ln -s /etc/elasticsearch/elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml && \
    ln -s /etc/elasticsearch/logging.yml /usr/share/elasticsearch/config/logging.yml && \
    mkdir -p /mnt/data && \
    echo "path.data: /mnt/data/data" >> /etc/elasticsearch/elasticsearch.yml && \
    echo "export PATH=$PATH:/usr/share/elasticsearch/bin/" >> /root/.bashrc && \
    cp -f /root/.bashrc /.bashrc && \
    apt-get autoremove -y && \
    apt-get clean all

# Env
ENV PATH $PATH:/usr/share/elasticsearch/bin/

# Default command
CMD ["/usr/share/elasticsearch/bin/elasticsearch"]

# Ports
EXPOSE 9200
EXPOSE 9300

# Volumes
VOLUME ["/mnt/data"]