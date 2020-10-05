FROM centos:centos7
LABEL maintainer='altanai'
RUN export VARIABLE=$(date)

RUN bash -c 'echo -e ...... starting Asterisk setup process .......'
RUN yum update -y \
    && yum install -y \
                kernel-headers \
                wget \
                gcc \
                gcc-c++ \
                cpp \
                ncurses \
                ncurses-devel \
                libxml2 \
                libxml2-devel \
                sqlite \
                sqlite-devel \
                openssl-devel \
                newt-devel \
                kernel-devel \
                libuuid-devel \
                net-snmp-devel \
                xinetd \
                tar \
                make \
                git \
                bzip2 \
                patch \
    && yum clean all \
    && cd /tmp

# Developer tools
#RUN yum install -y telnet
#RUN yum install -y net-tools

# Download libedit dependency
RUN bash -c "wget http://mirror.centos.org/centos/7/os/x86_64/Packages/libedit-devel-3.0-12.20121213cvs.el7.x86_64.rpm" \
    && rpm -i libedit-devel-3.0-12.20121213cvs.el7.x86_64.rpm \
    # Download asterisk.
    && wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz  \
    && mkdir asterisksrc \
    && tar -zxvf asterisk-*.tar.gz -C asterisksrc --strip-components=1 \
    && cd asterisksrc \
    # make asterisk.
    # Configure
    && ./configure --libdir=/usr/lib64 --with-jansson-bundled 1> /dev/null \
    # Remove the native build option
    && make -j$(nproc) menuselect.makeopts \
    && menuselect/menuselect \
                          --disable BUILD_NATIVE \
                          --enable cdr_csv \
                          --enable chan_sip \
                          --enable res_snmp \
                          --enable res_http_websocket \
                      menuselect.makeopts \
    # Continue with a standard make.
    && make -j$(nproc) 1> /dev/null \
    && make -j$(nproc) install 1> /dev/null \
    && make -j$(nproc) samples 1> /dev/null \
    # clean up the cached make files
    && make dist-clean \
    # Create and configure asterisk for running asterisk user.
    && useradd -m asterisk -s /sbin/nologin

# copy the config file from repo
WORKDIR /etc/asterisk/

COPY config/* /etc/asterisk/

# set runuser and rungroup
RUN sed -i -E 's/^;(run)(user|group)/\1\2/' /etc/asterisk/asterisk.conf

RUN chown -R asterisk:asterisk /var/run/asterisk \
                                  /etc/asterisk/ \
                                  /var/lib/asterisk/ \
                                  /var/log/asterisk/ \
                                  /var/spool/asterisk/ \
                                  /var/spool/asterisk/recording/ \
                                  /usr/lib64/asterisk/

RUN chmod -R 777 /var/spool/asterisk/ \
                 /var/spool/asterisk/recording

RUN bash -c 'echo -e ...... spool'
RUN bash -c 'ls -lt /var/spool/asterisk/'

RUN bash -c 'echo -e ...... configurations'
RUN bash -c 'ls -lt /etc/asterisk'

RUN bash -c 'echo -e ...... modules'
RUN bash -c 'cat /etc/asterisk/modules.conf'

RUN bash -c 'echo -e ...... sip conf'
RUN bash -c 'cat /etc/asterisk/sip.conf'

RUN bash -c 'echo -e ...... Dialplan'
RUN bash -c 'cat /etc/asterisk/extensions.conf'

RUN bash -c 'asterisk module show like rtp'

# Running asterisk with user asterisk.
USER asterisk

CMD ["/usr/sbin/asterisk"," -fv -f"]