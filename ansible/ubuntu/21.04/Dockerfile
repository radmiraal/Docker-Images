FROM ubuntu:21.04

ENV TZ=Antarctica/Troll

RUN set -eux; \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone; \
    apt-get update; \
    apt-get install -y \
        apt-utils gcc git python3 python3-dev python3-pip libssl-dev sudo; \
    apt-get remove --purge -y \
        apt-transport-https \
        ca-certificates; \
    apt-get autoremove -y; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/*;

ENV SHELL /bin/bash
