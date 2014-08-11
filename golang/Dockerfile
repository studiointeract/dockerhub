# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# golang
#
# Test
#   docker run cmfatih/golang go version
#   docker run -v `pwd`:/mnt/test cmfatih/golang /bin/bash -c "source /golang-crosscompile/crosscompile.bash && go-build-all /mnt/test/test.go"

# VERSION 1.0.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Commands
RUN \
  apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y vim zip wget curl git mercurial build-essential && \
  mkdir /go && \
  mkdir /golang-crosscompile && \
  mkdir -p /usr/local/go && \
  cd /usr/local/go && \
  hg clone https://code.google.com/p/go -r release-branch.go1.3 /usr/local/go && \
  cd /usr/local/go/src && \
  bash -c './all.bash' && \
  export PATH=/usr/local/go/bin:/go/bin:$PATH && \
  export GOPATH=/go && \
  export GOARM=5 && \
  cd /golang-crosscompile && \
  wget -qO /golang-crosscompile/crosscompile.bash https://raw.githubusercontent.com/davecheney/golang-crosscompile/e39557f537aaead8936bde9b5ebdb30e0bd94bf6/crosscompile.bash && \
  bash -c 'source /golang-crosscompile/crosscompile.bash && go-crosscompile-build-all' && \
  echo "source /golang-crosscompile/crosscompile.bash" >> /root/.bashrc && \
  echo "export PATH=/usr/local/go/bin:/go/bin:$PATH" >> /root/.bashrc && \
  echo "export GOPATH=/go" >> /root/.bashrc && \
  echo "export GOARM=5" >> /root/.bashrc && \
  cp -f /root/.bashrc /.bashrc && \
  apt-get autoremove -y && \
  apt-get clean all

# Env
ENV PATH /usr/local/go/bin:/go/bin:$PATH
ENV GOPATH /go
ENV GOARM 5

# Default command
CMD ["/usr/local/go/bin/go"]
WORKDIR /go