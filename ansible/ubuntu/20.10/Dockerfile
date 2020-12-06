FROM ubuntu:20.10

RUN set -eux; \
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
