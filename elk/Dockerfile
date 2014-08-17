# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# elk
#
# Test
#   docker run cmfatih/elk elasticsearch -v
#   docker run cmfatih/elk logstash -V
#
#   See http://localhost:12080/ 
#
#   docker run -it -p 12080:80 cmfatih/elk
#   docker run -it -p 12080:80 cmfatih/elk /usr/bin/elk-test
#   docker run -it -p 12080:80 -v `pwd`:/mnt/data cmfatih/elk
#
#   docker run -it -p 12080:80 cmfatih/elk logstash -e 'input { stdin { } } output { stdout { } }'
#   docker run -it -p 12080:80 cmfatih/elk /usr/bin/logstash-test

# VERSION 0.1.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Env
ENV ELASTICSEARCH_VERSION 1.1.1
ENV KIBANA_VERSION 3.1.0
ENV LOGSTASH_VERSION 1.4.2

# Commands
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y vim wget curl software-properties-common python-software-properties nginx apache2-utils && \
    mkdir -p /mnt/data && \
    cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak && \

    add-apt-repository -y ppa:webupd8team/java && \
    apt-get update && \
    echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && \
    apt-get install -y oracle-java7-installer && \

    wget -qO /tmp/elasticsearch.tar.gz https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-$ELASTICSEARCH_VERSION.tar.gz && \
    tar -zxf /tmp/elasticsearch.tar.gz -C /tmp && \
    mv /tmp/elasticsearch-$ELASTICSEARCH_VERSION/ /usr/share/elasticsearch && \
    rm -f /tmp/elasticsearch.tar.gz && \
    echo "cluster.name: elk" >> /usr/share/elasticsearch/config/elasticsearch.yml && \
    echo "node.name: \"elk.test\"" >> /usr/share/elasticsearch/config/elasticsearch.yml && \
    echo "path.data: /mnt/data/data" >> /usr/share/elasticsearch/config/elasticsearch.yml && \
    echo "script.disable_dynamic: true" >> /usr/share/elasticsearch/config/elasticsearch.yml && \
    echo "network.host: localhost" >> /usr/share/elasticsearch/config/elasticsearch.yml && \

    wget -qO /tmp/kibana.tar.gz https://download.elasticsearch.org/kibana/kibana/kibana-$KIBANA_VERSION.tar.gz && \
    tar -zxf /tmp/kibana.tar.gz -C /tmp && \
    mv /tmp/kibana-$KIBANA_VERSION/ /usr/share/kibana && \
    rm -f /tmp/kibana.tar.gz && \
    sed -i 's@elasticsearch: "http://"+window.location.hostname+":9200",@elasticsearch: { server: "http://"+window.location.hostname+":12080", withCredentials: true },@g' /usr/share/kibana/config.js && \
    wget -qO /tmp/kibana-nginx.conf https://raw.githubusercontent.com/elasticsearch/kibana/1baebe1d54bb0dce20b47b85f2f0564b945a6c29/sample/nginx.conf && \
    sed -i 's@kibana.myhost.org.htpasswd@localhost.htpasswd@g' /tmp/kibana-nginx.conf && \
    sed -i 's@kibana.myhost.org@localhost@g' /tmp/kibana-nginx.conf && \
    sed -i 's@/usr/share/kibana3;@/usr/share/kibana;@g' /tmp/kibana-nginx.conf && \
    htpasswd -bc /etc/nginx/conf.d/localhost.htpasswd kibana kibana && \
    mv /tmp/kibana-nginx.conf /etc/nginx/sites-available/default && \

    wget -qO /tmp/logstash.tar.gz https://download.elasticsearch.org/logstash/logstash/logstash-$LOGSTASH_VERSION.tar.gz && \
    tar -zxf /tmp/logstash.tar.gz -C /tmp && \
    mv /tmp/logstash-$LOGSTASH_VERSION/ /usr/share/logstash && \
    rm -f /tmp/logstash.tar.gz && \
    echo 'input {file {type => "nginx_access" path => [ "/var/log/nginx/**"] exclude => ["*.gz", "error.*"] }}' > /etc/logstash.conf && \
    echo 'output {elasticsearch {host => "localhost" cluster => "elk"}}' >> /etc/logstash.conf && \
    echo '#!/bin/bash' > /usr/bin/logstash-test && \
    echo '/usr/share/elasticsearch/bin/elasticsearch -d -p /mnt/data/elk.pid' >> /usr/bin/logstash-test && \
    echo 'echo "Please wait..."' >> /usr/bin/logstash-test && \
    echo 'sleep 10s' >> /usr/bin/logstash-test && \
    echo '/etc/init.d/nginx start' >> /usr/bin/logstash-test && \
    echo '/usr/share/logstash/bin/logstash --config /etc/logstash.conf' >> /usr/bin/logstash-test && \
    chmod +x /usr/bin/logstash-test && \

    echo '#!/bin/bash' > /usr/bin/elk && \
    echo '/etc/init.d/nginx start' >> /usr/bin/elk && \
    echo '/usr/share/elasticsearch/bin/elasticsearch -p /mnt/data/elk.pid' >> /usr/bin/elk && \
    chmod +x /usr/bin/elk && \

    echo '#!/bin/bash' > /usr/bin/elk-test && \
    echo '/usr/share/elasticsearch/bin/elasticsearch -d -p /mnt/data/elk.pid' >> /usr/bin/elk-test && \
    echo 'echo "Please wait..."' >> /usr/bin/elk-test && \
    echo 'sleep 10s' >> /usr/bin/elk-test && \
    echo "curl -XPUT http://localhost:9200/shakespeare -d '{\"mappings\" : {\"_default_\" : {\"properties\" : {\"speaker\" : {\"type\": \"string\", \"index\" : \"not_analyzed\" },\"play_name\" : {\"type\": \"string\", \"index\" : \"not_analyzed\" },\"line_id\" : { \"type\" : \"integer\" },\"speech_number\" : { \"type\" : \"integer\" }}}}}'" >> /usr/bin/elk-test && \
    echo 'echo "Loading data..."' >> /usr/bin/elk-test && \
    echo 'curl -XPUT http://localhost:9200/_bulk --data-binary @/tmp/shakespeare.json' >> /usr/bin/elk-test && \
    echo 'curl -XPOST http://localhost:9200/_cluster/nodes/_local/_shutdown' >> /usr/bin/elk-test && \
    echo 'echo "Re-starting servers..."' >> /usr/bin/elk-test && \
    echo 'sleep 10s' >> /usr/bin/elk-test && \
    echo '/etc/init.d/nginx start' >> /usr/bin/elk-test && \
    echo '/usr/share/elasticsearch/bin/elasticsearch -p /mnt/data/elk.pid' >> /usr/bin/elk-test && \
    echo '#read -p "Press any key for exit..."' >> /usr/bin/elk-test && \
    chmod +x /usr/bin/elk-test && \
    wget -qO /tmp/shakespeare.json http://www.elasticsearch.org/guide/en/kibana/current/snippets/shakespeare.json && \

    echo "export PATH=$PATH:/usr/share/elasticsearch/bin/:/usr/share/logstash/bin/" >> /root/.bashrc && \
    cp -f /root/.bashrc /.bashrc && \
    apt-get autoremove -y && \
    apt-get clean all

# Env
ENV PATH $PATH:/usr/share/elasticsearch/bin/:/usr/share/logstash/bin/

# Default command
CMD ["/usr/bin/elk"]

# Ports
EXPOSE 12080

# Volumes
VOLUME ["/mnt/data"]