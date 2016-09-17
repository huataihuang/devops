FROM centos:7
MAINTAINER "Huatai Huang" <huataihuang@gmail.com>
ENV container docker

RUN yum clean all;yum update -y
RUN yum -y install which nc openssh-clients openssh-server mlocate net-tools rsyslog file ntp \
wget tar bzip2 screen sysstat unzip nfs-utils parted lsof man mlocate bind-utils \
gcc gcc-c++ make telnet flex autoconf automake ncurses-devel crontabs \
zlib-devel nmap-ncat rsync git sudo

# phabricator
RUN yum install libxml2-devel libcurl-devel -y

RUN rm /etc/localtime;ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]

# generate sshd key and disable PAM (allow no-root login)
RUN sshd-keygen
RUN sed 's/UsePAM yes/UsePAM no/' -i /etc/ssh/sshd_config

# add normal user, the /home directory will map to host
RUN useradd -g 10 -u 1001 -d /home/huatai huatai
RUN echo 'huatai   ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

# active php
RUN echo 'PATH=$PAHT:/home/huatai/php' >> /etc/profile
RUN echo 'export PATH' >> /etc/profile

# install phabricator
# ~~~~~~~~~~~~~~~~~~~~
#wget http://cn2.php.net/distributions/php-5.6.25.tar.bz2
#tar xfj php-5.6.25.tar.bz2
#cd php-5.6.25/
#./configure --prefix=/home/huatai/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-fpm-user=huatai --with-fpm-group=staff
#make
#make install
#cp /home/huatai/php/etc/php-fpm.conf.default /home/huatai/php/etc/php-fpm.conf
#cp ./php.ini-development /home/huatai/php/etc/php.ini

#cd ext/curl/
#/home/huatai/php/bin/phpize
#./configure --with-curl=/usr/local/curl --with-php-config=/home/huatai/php/bin/php-config
#make
#make install
#echo "add 'extension=no-debug-non-zts-20131226/curl.so' to /huatai/php/etc/php.ini"

#RUN [ -d /root/.ssh ] || (mkdir /root/.ssh;chmod 700 /root/.ssh)
#RUN echo 'RSA Public Key, please replace it' >> /root/.ssh/authorized_keys;chmod 600 /root/.ssh/authorized_keys

#RUN [ -d /home/huatai/.ssh ] || (mkdir /home/huatai/.ssh;chmod 700 /home/huatai/.ssh)
#RUN echo 'RSA Public Key, please replace it' >> /home/huatai/.ssh/authorized_keys;chown huatai:staff /home/huatai/.ssh/authorized_keys;chmod 600 /home/huatai/.ssh/authorized_keys

# enable sshd and rsyslog
RUN ln -s /usr/lib/systemd/system/sshd.service /etc/systemd/system/multi-user.target.wants/sshd.service
RUN ln -s /usr/lib/systemd/system/rsyslog.service /etc/systemd/system/multi-user.target.wants/rsyslog.service

CMD ["/usr/sbin/init"]
