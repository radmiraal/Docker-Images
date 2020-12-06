FROM centos:8

RUN set -eux ; \
    yum -y update ; \
    yum -y install python38 ; \
    yum clean all ; \
    rm -rf /var/log/* ; \
    rm -rf /tmp/* ;
