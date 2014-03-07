# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# golang
#
# Test
#   docker run cmfatih/golang go version

# VERSION 0.1.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Update package repository
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe multiverse" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y vim zip wget curl git mercurial build-essential

# bashrc
RUN cp -f /root/.bashrc /.bashrc

# golang
RUN mkdir -p /go
RUN mkdir -p /usr/local/go
RUN cd /usr/local/go

RUN hg clone https://code.google.com/p/go /usr/local/go
RUN cd /usr/local/go/src && bash -c './all.bash'

# env
ENV PATH /usr/local/go/bin:/go/bin:$PATH
ENV GOPATH /go
ENV GOARM 5

# golang crosscompile
RUN cd /
RUN git clone git://github.com/davecheney/golang-crosscompile.git
RUN bash -c 'source golang-crosscompile/crosscompile.bash && go-crosscompile-build-all'

# bashrc
RUN echo "source /golang-crosscompile/crosscompile.bash" >> /root/.bashrc
RUN cp -f /root/.bashrc /.bashrc

# misc
WORKDIR /go

# Removed unnecessary packages
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all