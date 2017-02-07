FROM ubuntu:14.04

MAINTAINER Daniele Venzano <venza@brownhat.org>

RUN apt-get update && apt-get install -y openssh-server openssh-client ldap-auth-client libnss-ldap sudo vim software-properties-common python-software-properties git && apt-get clean
RUN mkdir /var/run/sshd

COPY ldap.conf /etc/ldap.conf

RUN auth-client-config -t nss -p lac_ldap

#RUN pam-auth-update

RUN echo "session required        pam_mkhomedir.so skel=/etc/skel umask=0022" >> /etc/pam.d/common-session

RUN echo "bigfoot_admin ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN locale-gen en_US.utf8


RUN apt-add-repository -y ppa:webupd8team/java
RUN /bin/echo debconf shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get update && apt-get -y install oracle-java7-installer oracle-java7-set-default curl && apt-get clean

WORKDIR /opt

ENV HADOOP_VERSION 2.6.4
RUN curl -s http://apache.mirrors.ovh.net/ftp.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz | tar -xz -C /opt/

RUN ln -s hadoop-${HADOOP_VERSION} hadoop

ENV JAVA_HOME /usr/lib/jvm/java-7-oracle/
ENV HADOOP_HOME /opt/hadoop

COPY core-site.xml ${HADOOP_HOME}/etc/hadoop/core-site.xml
COPY hdfs-site.xml ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
COPY hadoop.sh /etc/profile.d/hadoop.sh

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D", "-e"]


