FROM docker.io/centos:7
MAINTAINER vincent huatai <vincent@huatai.me>

RUN yum clean all
RUN yum -y update

RUN yum -y install which sudo nmap-ncat mlocate net-tools rsyslog file ntp ntpdate \
wget tar bzip2 xz screen sysstat unzip nfs-utils lsof man bind-utils \
crontabs openssl openssh-clients openssh-server initscripts

# some python dependcies need c compiler
RUN yum groupinstall -y "development tools"

# install pip + virtualenv
RUN curl https://bootstrap.pypa.io/get-pip.py | python2.7
RUN pip2.7 install virtualenv

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

# deploy python virtualenv, sudo to admin
USER admin
RUN virtualenv /home/admin/venv2

# deploy PerfKit Benchmarker
RUN source /home/admin/venv2/bin/activate;cd /home/admin;\
wget https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/archive/v1.13.0.zip;unzip v1.13.0.zip;\
cd /home/admin/PerfKitBenchmarker-1.13.0;pip install -r requirements.txt

# alicloud
USER root
RUN yum -y install python-devel libffi-devel.x86_64 openssl-devel.x86_64
USER admin
RUN source /home/admin/venv2/bin/activate;pip install 'colorama<=0.3.3';cd /home/admin/PerfKitBenchmarker-1.13.0;\
pip install -r perfkitbenchmarker/providers/alicloud/requirements.txt

# run service sshd & bash console
USER root
EXPOSE 22
ENTRYPOINT /usr/sbin/sshd && /bin/bash
