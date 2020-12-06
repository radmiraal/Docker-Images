FROM centos:7

RUN set -eux ; \
    yum -y update ; \
    yum -y install python ; \
    yum clean all ; \
    rm -rf /var/log/* ; \
    rm -rf /tmp/* ;
