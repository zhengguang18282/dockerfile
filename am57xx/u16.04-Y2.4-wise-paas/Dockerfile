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


RUN wget -O "/home/adv/linux-devkit.sh" "https://sh-ctfs.ftn.qq.com/ftn_handler/f31ada45e5e29dc9c6c7c23978cf09abee8c720fffa5a32a24e1ee79d073b26591724a7c1bcaddc60beaadb9dc126f3b16c08560b75a755ffb62290557333e43/?fname=linux-devkit.sh&k=22626433cc4b608d822dde111f31504c54565d0a085053524b5400000c1c5602520149515d57574e07515c510f55575556525251393e620f0f0c114b145507150d0b101d4a59625e&code=fbd391bc&fr=00&&txf_fid=5db6f683a69c8373cf61a1f825e81c10cd7440a1&xffz=301827339"


#RUN ls -al
#RUN echo pwd=$PWD

RUN chmod 777 /home/adv/linux-devkit.sh
RUN sh -c '/bin/echo -e "\n\n" | sh /home/adv/linux-devkit.sh'
RUN rm /home/adv/linux-devkit.sh

