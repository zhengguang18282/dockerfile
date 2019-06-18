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

RUN wget https://releases.linaro.org/components/toolchain/binaries/7.2-2017.11/arm-linux-gnueabihf/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf.tar.xz
RUN tar -Jxvf gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf.tar.xz -C $HOME
RUN rm gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf.tar.xz
RUN wget https://releases.linaro.org/components/toolchain/binaries/7.2-2017.11/aarch64-linux-gnu/gcc-linaro-7.2.1-2017.11-x86_64_aarch64-linux-gnu.tar.xz
RUN tar -Jxvf gcc-linaro-7.2.1-2017.11-x86_64_aarch64-linux-gnu.tar.xz -C $HOME
RUN rm gcc-linaro-7.2.1-2017.11-x86_64_aarch64-linux-gnu.tar.xz

ENV TOOLCHAIN_PATH_ARMV7=/home/adv/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf
ENV TOOLCHAIN_PATH_ARMV8=/home/adv/gcc-linaro-7.2.1-2017.11-x86_64_aarch64-linux-gnu

# locale
RUN sudo locale-gen en_US.UTF-8
RUN sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG="en_US.UTF-8"

RUN git clone git://arago-project.org/git/projects/oe-layersetup.git tisdk
WORKDIR /home/adv/tisdk
RUN ./oe-layertool-setup.sh -f configs/processor-sdk/processor-sdk-05.03.00.07-config.txt 
WORKDIR /home/adv/tisdk/build 
ENV BB_ENV_EXTRAWHITE="MACHINE DISTRO TCMODE TCLIBC http_proxy ftp_proxy https_proxy all_proxy ALL_PROXY no_proxy SSH_AGENT_PID SSH_AUTH_SOCK BB_SRCREV_POLICY SDKMACHINE BB_NUMBER_THREADS PARALLEL_MAKE GIT_PROXY_COMMAND GIT_PROXY_IGNORE SOCKS5_PASSWD SOCKS5_USER OEBASE META_SDK_PATH TOOLCHAIN_TYPE TOOLCHAIN_BRAND TOOLCHAIN_BASE TOOLCHAIN_PATH TOOLCHAIN_PATH_ARMV5 TOOLCHAIN_PATH_ARMV7 TOOLCHAIN_PATH_ARMV8 EXTRA_TISDK_FILES TISDK_VERSION ARAGO_BRAND ARAGO_RT_ENABLE ARAGO_SYSTEST_ENABLE ARAGO_KERNEL_SUFFIX TI_SECURE_DEV_PKG_CAT TI_SECURE_DEV_PKG_AUTO TI_SECURE_DEV_PKG_K3 ARAGO_SYSVINIT SYSFW_FILE"
ENV BUILDDIR="/home/adv/tisdk/build" 
ENV OEBASE="/home/adv/tisdk"
ENV OLDPWD="/home/adv/tisdk"
ENV PATH="/home/adv/tisdk/sources/meta-ros/scripts:/home/adv/tisdk/sources/meta-browser/scripts:/home/adv/tisdk/sources/oe-core/scripts:/home/adv/tisdk/sources/bitbake/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
RUN MACHINE=am335x-evm bitbake arago-core-tisdk-image

RUN cp /home/adv/tisdk/build/arago-tmp-external-linaro-toolchain/deploy/images/am335x-evm/processor-sdk-linux-image-am335x-evm.tar.xz /home/adv
RUN rm -r /home/adv/gcc-linaro-7.2.1-2017.11-x86_64_aarch64-linux-gnu  /home/adv/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabihf tisdk

