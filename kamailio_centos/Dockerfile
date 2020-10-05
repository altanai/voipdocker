FROM centos:7
LABEL maintainer='altanai'
RUN export VARIABLE=$(date)

RUN bash -c 'echo -e ...... starting Kamailio setup process .......'

RUN yum install -y \
     wget \
     gcc \
     make \
     bison \
     flex \
     libcurl \
     libcurl-devel \
     libunistring-devel \
     openssl \
     openssl-devel \
     pcre-devel \
     zlib-devel \
     lua-5.1.4-14.el7 \
     lua-devel-5.1.4-14.el7 \
     mysql-community-devel-5.6.26-2.el7 \
     libxml2-devel \
     perl-ExtUtils-Embed \
     net-snmp-devel \
     memcached \
     cyrus-sasl-devel && \
 yum clean all

# Developer tools - NOT FOR PRODUCTION
# RUN yum install -y telnet
RUN yum install -y net-tools

#  add the Kamailio 5.3 Open Build Service repository to our yum.repos.d list.
RUN cd /etc/yum.repos.d/ \
    && wget -O /etc/yum.repos.d/kamailio.repo \
    http://download.opensuse.org/repositories/home:/kamailio:/v5.3.x-rpms/CentOS_7/home:kamailio:v5.3.x-rpms.repo
RUN yum -y update

# Kamailio mysql
# RUN rpm -Uvh http://repo.okay.com.mx/centos/6/x86_64/release/kamailio-mysql-5.0.3-10.el6.x86_64.rpm

RUN yum-config-manager --add-repo https://rpm.kamailio.org/centos/kamailio.repo
RUN yum install -y --disablerepo=kamailio --enablerepo=kamailio-5.3 kamailio

RUN  yum clean all

# RUN mkdir /etc/kamailio
WORKDIR /etc/kamailio/

COPY config/* /etc/kamailio/

# logs
RUN touch /var/log/kamailio.log
RUN chmod 777 /var/log/kamailio.log

# Change User access for user Kamailio's
RUN chown -R kamailio:kamailio /var/run/kamailio
RUN chown -R kamailio:kamailio /etc/kamailio/

RUN bash -c "cat /etc/kamailio/kamailio.cfg"

RUN bash -c "ls -lt  /usr/lib64/kamailio/modules/"

RUN bash -c 'echo -e ...... starting Kamailio  .......'

USER kamailio
EXPOSE 5060
EXPOSE 5060/udp

# CMD ["/usr/sbin/kamailio", "-m 64", "-M 8", "-D" ]
ENTRYPOINT ["kamailio", "-DD", "-E", "-e"]