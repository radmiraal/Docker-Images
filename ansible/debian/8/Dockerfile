FROM debian:jessie-slim

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        apt-utils gcc git python python-dev python-pip libssl-dev sudo; \
    apt-get remove --purge -y \
        apt-transport-https \
        ca-certificates; \
    apt-get autoremove -y; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/*;

ENV SHELL /bin/bash
