# HowTo use this dockerfile?
# ------ for example:
# 1. create image which name "local:centos7"
# 2. create container which name "dev7"
# --------------------------------------
# docker build -t local:centos7 .
# docker run -itd --hostname dev7 --name dev7 local:centos7

FROM docker.io/centos:7
MAINTAINER vincent huatai <vincent@huatai.me>

RUN yum clean all
RUN yum -y update

RUN yum -y install which sudo openssh-clients openssh-server initscripts

# ----------
# If need more utilities, as like follow:
# ----------
#yum install which mlocate net-tools rsyslog file ntp ntpdate \
#wget tar bzip2 xz screen sysstat unzip nfs-utils lsof man bind-utils \
#crontabs openssh-clients openssh-server initscripts

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

# run service when container started - sshd
EXPOSE 22
#CMD ["/usr/sbin/sshd", "-D"]

# ----------
# WANT run sshd and get a bash
# ENTRYPOINT will not be override by commandline
# ----------
ENTRYPOINT /usr/sbin/sshd && /bin/bash
