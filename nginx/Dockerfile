FROM debian:buster-slim AS base

ARG DEBIAN_FRONTEND=noninteractive

ADD https://nginx.org/keys/nginx_signing.key /nginx_signing.key

RUN set -eux; \
    apt-get update; \
    apt-get install --no-install-recommends --no-install-suggests -y \
        apt-transport-https \
        ca-certificates \
        gnupg; \
    apt-key add /nginx_signing.key; \
    rm -rf /nginx_signing.key; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/*;

FROM base AS brotli-compile-image

ARG DEBIAN_FRONTEND=noninteractive

RUN set -eux; \
    apt-get update; \
    apt-get install -y \
        git \
        lsb-release; \
    echo "deb https://nginx.org/packages/mainline/debian/ `lsb_release -sc` nginx \ndeb-src https://nginx.org/packages/mainline/debian/ `lsb_release -sc` nginx \n" >> /etc/apt/sources.list.d/nginx_mainline.list ; \
    echo "deb http://deb.debian.org/debian experimental main" >> /etc/apt/sources.list.d/experimental.list; \
    apt-get update; \
    apt-get install -y \
        nginx \
        brotli \
        libbrotli-dev \
        dpkg-dev \
        libpcre3 \
        libpcre3-dev \
        zlib1g \
        zlib1g-dev \
        openssl \
        libssl-dev

RUN set -eux; \
    mkdir -p /opt; \
    git clone https://github.com/google/ngx_brotli /opt/ngx_brotli; \
    mkdir -p /usr/src/nginx; \
    cd /usr/src/nginx/; \
    apt-get source nginx;

RUN set -eux; \
    cd /usr/src/nginx/nginx-*; \
    ./configure --with-compat --add-dynamic-module=/opt/ngx_brotli;

RUN set -eux; \
    cd /usr/src/nginx/nginx-*; \
    make modules; \
    chmod 644 objs/*.so; \
    cp objs/*.so /etc/nginx/modules; \
    rm -rf /usr/src/nginx;


FROM base

ARG DEBIAN_FRONTEND=noninteractive

COPY --from=brotli-compile-image /etc/apt/sources.list.d/nginx_mainline.list /etc/apt/sources.list.d/

RUN set -eux; \
    apt-get update; \
    apt-get install -y \
        nginx \
        brotli \
        libbrotli1; \
    apt-get remove --purge -y \
        apt-transport-https \
        ca-certificates; \
    apt-get autoremove -y; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/*;

COPY --from=brotli-compile-image /etc/nginx/modules /etc/nginx/modules/

RUN set -eux; \
    echo "load_module modules/ngx_http_brotli_filter_module.so;\n$(cat /etc/nginx/nginx.conf)" > /etc/nginx/nginx.conf; \
    echo "load_module modules/ngx_http_brotli_static_module.so;\n$(cat /etc/nginx/nginx.conf)" > /etc/nginx/nginx.conf;

# forward request and error logs to docker log collector
RUN set -eux; \
    ln -sf /dev/stdout /var/log/nginx/access.log; \
    ln -sf /dev/stderr /var/log/nginx/error.log;

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
