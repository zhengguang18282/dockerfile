FROM ubuntu:16.04

MAINTAINER Advantech

# update
RUN apt-get update
RUN apt-get install -y apt-utils sudo
RUN apt-get install -y locales curl cpio git build-essential python diffstat texinfo gawk chrpath dos2unix wget unzip socat doxygen lib32stdc++6 lib32z1 lib32ncurses5 libc6-dev-i386 gcc-multilib g++-multilib

# jenkins
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

# openjdk
RUN apt-get update
RUN apt-get install -y openjdk-8-jdk
# crc32
RUN apt-get install -y libarchive-zip-perl

# make /bin/sh symlink to bash instead of dash:
# To set this particular option to false:
RUN echo "dash dash/sh boolean false" | debconf-set-selections
# and to actually reconfigure the package:
RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

# adv account (password: adv)
RUN useradd -m -k /home/adv adv -p ajLGz61mdCP76 -s /bin/bash -G sudo
RUN chmod +s /usr/sbin/sshd

# set up adv as sudo
RUN echo "adv ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
WORKDIR /home/adv
USER adv

# git config
RUN git config --global user.email "you@example.com"
RUN git config --global user.name "Your Name"
RUN git config --global color.ui "auto"

# locale
RUN sudo locale-gen en_US.UTF-8
RUN sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG="en_US.UTF-8"

RUN git clone https://github.com/zhengguang18282/linux-devkit.git && cd /home/adv/linux-devkit && git checkout arago-2018.10 
