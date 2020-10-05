# kamailio Docker container

Build 
```
➜  kamailio_centos git:(main) ✗ docker build -t altanai/kamailio .
[+] Building 2.8s (24/24) FINISHED                                                                                                                                             
 => [internal] load build definition from Dockerfile                                                                                                                      0.0s
 => => transferring dockerfile: 1.89kB                                                                                                                                    0.0s
 => [internal] load .dockerignore                                                                                                                                         0.0s
 => => transferring context: 2B                                                                                                                                           0.0s
 => [internal] load metadata for docker.io/library/centos:7                                                                                                               2.7s
 => [1/19] FROM docker.io/library/centos:7@sha256:19a79828ca2e505eaee0ff38c2f3fd9901f4826737295157cc5212b7a372cd2b                                                        0.0s
 => [internal] load build context                                                                                                                                         0.0s
 => => transferring context: 28B                                                                                                                                          0.0s
 => CACHED [2/19] RUN export VARIABLE=$(date)                                                                                                                             0.0s
 => CACHED [3/19] RUN bash -c 'echo -e ...... starting Kamailio setup process .......'                                                                                    0.0s
 => CACHED [4/19] RUN yum install -y      wget      gcc      make      bison      flex      libcurl      libcurl-devel      libunistring-devel      openssl      openssl  0.0s
 => CACHED [5/19] RUN yum install -y net-tools                                                                                                                            0.0s
 => CACHED [6/19] RUN cd /etc/yum.repos.d/     && wget -O /etc/yum.repos.d/kamailio.repo     http://download.opensuse.org/repositories/home:/kamailio:/v5.3.x-rpms/CentO  0.0s
 => CACHED [7/19] RUN yum -y update                                                                                                                                       0.0s
 => CACHED [8/19] RUN yum-config-manager --add-repo https://rpm.kamailio.org/centos/kamailio.repo                                                                         0.0s
 => CACHED [9/19] RUN yum install -y --disablerepo=kamailio --enablerepo=kamailio-5.3 kamailio                                                                            0.0s
 => CACHED [10/19] RUN  yum clean all                                                                                                                                     0.0s
 => CACHED [11/19] WORKDIR /etc/kamailio/                                                                                                                                 0.0s
 => CACHED [12/19] COPY config/* /etc/kamailio/                                                                                                                           0.0s
 => CACHED [13/19] RUN touch /var/log/kamailio.log                                                                                                                        0.0s
 => CACHED [14/19] RUN chmod 777 /var/log/kamailio.log                                                                                                                    0.0s
 => CACHED [15/19] RUN chown -R kamailio:kamailio /var/run/kamailio                                                                                                       0.0s
 => CACHED [16/19] RUN chown -R kamailio:kamailio /etc/kamailio/                                                                                                          0.0s
 => CACHED [17/19] RUN bash -c "cat /etc/kamailio/kamailio.cfg"                                                                                                           0.0s
 => CACHED [18/19] RUN bash -c "ls -lt  /usr/lib64/kamailio/modules/"                                                                                                     0.0s
 => CACHED [19/19] RUN bash -c 'echo -e ...... starting Kamailio  .......'                                                                                                0.0s
 => exporting to image                                                                                                                                                    0.0s
 => => exporting layers                                                                                                                                                   0.0s
 => => writing image sha256:6469c49acdba97821d7dbc43f2601ad30c20cc79502cbfc70a3836453f6ade42                                                                              0.0s
 => => naming to docker.io/altanai/kamailio        
```

Run
```
➜  kamailio_centos git:(main) ✗ docker run  altanai/kamailio
 0(1) INFO: <core> [core/sctp_core.c:75]: sctp_core_check_support(): SCTP API not enabled - if you want to use it, load sctp module
Listening on 
             udp: 127.0.0.1:5060
             udp: 172.17.0.2:5060
             tcp: 127.0.0.1:5060
             tcp: 172.17.0.2:5060
Aliases: 
             tcp: 7a3434873a68:5060
             tcp: localhost:5060
             udp: 7a3434873a68:5060
             udp: localhost:5060

 0(1) INFO: <core> [core/tcp_main.c:5044]: init_tcp(): using epoll_lt as the io watch method (auto detected)
 0(1) INFO: rr [../outbound/api.h:52]: ob_load_api(): unable to import bind_ob - maybe module is not loaded
 0(1) INFO: rr [rr_mod.c:177]: mod_init(): outbound module not available
 0(1) INFO: <core> [main.c:2780]: main(): processes (at least): 32 - shm size: 67108864 - pkg size: 8388608
 0(1) INFO: <core> [core/udp_server.c:154]: probe_max_receive_buffer(): SO_RCVBUF is initially 212992
 0(1) INFO: <core> [core/udp_server.c:206]: probe_max_receive_buffer(): SO_RCVBUF is finally 425984
 0(1) INFO: <core> [core/udp_server.c:154]: probe_max_receive_buffer(): SO_RCVBUF is initially 212992
 0(1) INFO: <core> [core/udp_server.c:206]: probe_max_receive_buffer(): SO_RCVBUF is finally 425984
21(26) INFO: jsonrpcs [jsonrpcs_sock.c:443]: jsonrpc_dgram_process(): a new child 0/26
22(27) INFO: ctl [io_listener.c:214]: io_listen_loop(): io_listen_loop:  using epoll_lt io watch method (config)
```