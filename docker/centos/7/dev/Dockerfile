FROM docker.io/centos:7
MAINTAINER vincent huatai <vincent@huatai.me>

RUN yum clean all
RUN yum -y update

RUN yum -y install which sudo nmap-ncat mlocate net-tools rsyslog file ntp ntpdate \
wget tar bzip2 xz screen sysstat unzip nfs-utils lsof man bind-utils \
crontabs openssl openssh-clients openssh-server initscripts

# install develop env
RUN yum groupinstall -y "development tools"

# Prepare sshd host key
RUN ssh-keygen -A

# add account "admin" and give sudo privilege
RUN groupadd -g 505 admin
RUN useradd -g 505 -u 505 -d /home/admin -m admin
RUN usermod -aG wheel admin
RUN echo "%wheel        ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers

# Add ssh public key for login
RUN mkdir -p /home/admin/.ssh
ADD authorized_keys /home/admin/.ssh/authorized_keys
RUN chown -R admin:admin /home/admin/.ssh
RUN chmod 600 /home/admin/.ssh/authorized_keys
RUN chmod 700 /home/admin/.ssh

# run service sshd & bash console
EXPOSE 22
ENTRYPOINT /usr/sbin/sshd && /bin/bash
