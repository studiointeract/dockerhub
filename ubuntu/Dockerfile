# dockerhub - A repository for various dockerfiles
# For more information; http://github.com/cmfatih/dockerhub
#
# Ubuntu
#
# Test
#   docker run cmfatih/ubuntu /bin/echo dockerized!

# VERSION 0.2.0

FROM ubuntu:12.04

MAINTAINER cmfatih

# Update package repository
RUN apt-get update
RUN apt-get upgrade -y

# Required packages
RUN apt-get install -y software-properties-common vim htop zip bzip2 wget curl git

# bashrc
RUN cp -f /root/.bashrc /.bashrc

# htop
RUN mkdir -p /.config/ | mkdir -p /.config/htop/
RUN echo "fields=0 48 17 18 38 39 40 2 46 47 49 1\nsort_key=46\nsort_direction=1\nhide_threads=0\nhide_kernel_threads=1\nhide_userland_threads=1\nshadow_other_users=0\nshow_thread_names=0\nhighlight_base_name=1\nhighlight_megabytes=1\nhighlight_threads=1\ntree_view=1\nheader_margin=1\ndetailed_cpu_time=0\ncpu_count_from_zero=0\ncolor_scheme=0\ndelay=15\nleft_meters=Hostname Clock Memory CPU Swap\nleft_meter_modes=2 2 2 1 1\nright_meters=Uptime Tasks LoadAverage AllCPUs\nright_meter_modes=2 2 2 1\n" > /.config/htop/htoprc
RUN mkdir -p /root/.config/ | mkdir -p /root/.config/htop/
RUN cp -f /.config/htop/htoprc /root/.config/htop/htoprc

# Removed unnecessary packages
RUN apt-get autoremove -y

# Clear package repository cache
RUN apt-get clean all