# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# Emscripten
#
# Test
#   docker run cmfatih/emscripten /srv/var/emscripten/emcc -v
#   docker run -v `pwd`:/mnt/test cmfatih/emscripten /srv/var/emscripten/emcc /mnt/test/test.cpp -o /mnt/test/test.cpp.js

# VERSION 0.1.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# ENV
ENV EMSCRIPTEN_VERSION 1.20.0

# Update packages
RUN apt-get update
RUN apt-get upgrade -y

# Install packages
RUN apt-get install -y software-properties-common vim htop zip bzip2 wget curl git python python-software-properties g++ make cmake

# Check directories
RUN mkdir -p /srv/var

# Emscripten
RUN wget -O /tmp/emscripten.tar.gz https://github.com/kripken/emscripten/archive/$EMSCRIPTEN_VERSION.tar.gz
RUN tar -zxf /tmp/emscripten.tar.gz -C /tmp
RUN mv /tmp/emscripten-$EMSCRIPTEN_VERSION/ /srv/var/emscripten
RUN rm -f /tmp/emscripten.tar.gz

# Fastcomp
RUN wget -O /tmp/emscripten-fastcomp.tar.gz https://github.com/kripken/emscripten-fastcomp/archive/$EMSCRIPTEN_VERSION.tar.gz
RUN tar -zxf /tmp/emscripten-fastcomp.tar.gz -C /tmp
RUN mv /tmp/emscripten-fastcomp-$EMSCRIPTEN_VERSION/ /srv/var/emscripten-fastcomp
RUN rm -f /tmp/emscripten-fastcomp.tar.gz

# Clang
RUN wget -O /tmp/emscripten-fastcomp-clang.tar.gz https://github.com/kripken/emscripten-fastcomp-clang/archive/$EMSCRIPTEN_VERSION.tar.gz
RUN tar -zxf /tmp/emscripten-fastcomp-clang.tar.gz -C /tmp
RUN mv /tmp/emscripten-fastcomp-clang-$EMSCRIPTEN_VERSION/ /srv/var/emscripten-fastcomp/tools/clang
RUN rm -rf /tmp/emscripten-fastcomp-clang.tar.gz

# Compile fastcomp
RUN mkdir -p /srv/var/emscripten-fastcomp/build
RUN cd /srv/var/emscripten-fastcomp/build && /srv/var/emscripten-fastcomp/configure --enable-optimized --disable-assertions --enable-targets=host,js
RUN cd /srv/var/emscripten-fastcomp/build && make -j 2

# Node.js
RUN add-apt-repository -y ppa:chris-lea/node.js
RUN apt-get update
RUN apt-get install -y nodejs

# ENV
ENV PATH $PATH:/srv/var/emscripten:/srv/var/emscripten-fastcomp/build/Release/bin/
ENV LLVM /srv/var/emscripten-fastcomp/build/Release/bin/
RUN /srv/var/emscripten/emcc -v

# bashrc
RUN echo "export PATH=$PATH:/srv/var/emscripten:/srv/var/emscripten-fastcomp/build/Release/bin/" >> /root/.bashrc
RUN echo "export LLVM=/srv/var/emscripten-fastcomp/build/Release/bin/" >> /root/.bashrc
RUN cp -f /root/.bashrc /.bashrc

# Cleanup
RUN apt-get autoremove -y
RUN apt-get clean all