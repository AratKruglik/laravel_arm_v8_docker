FROM arm64v8/nginx:alpine

# Remove default files
RUN set -xe \
    && rm /etc/nginx/fastcgi_params \
    && rm /etc/nginx/koi-utf \
    && rm /etc/nginx/koi-win \
    && rm /etc/nginx/scgi_params \
    && rm /etc/nginx/uwsgi_params \
    && rm /etc/nginx/win-utf \
    && rm -rf /etc/nginx/conf.d

RUN apk --update upgrade --no-cache -q; \
    apk add --no-cache \
        jq \
        sudo \
        curl \
        wget \
        bash \
        tzdata \
        openssl \
        openssh \
        coreutils \
        moreutils \
        ca-certificates \
    && rm -rf /var/cache/apk/*

ARG USER_NAME=www-data

ARG GROUP_NAME=www-data

RUN set -xe; \
    addgroup --gid ${USER_GID:-1000} $GROUP_NAME; \
    addgroup root $GROUP_NAME; \
    adduser --uid ${USER_UID:-1000} -h /home/$USER_NAME -s /bin/bash -D -G $GROUP_NAME $USER_NAME; \
    echo "$USER_NAME ALL = (ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN set -xe; \
    mkdir -p /var/cache/nginx/app; \
    touch /var/run/nginx.pid;

ARG TZ=Europe/Kiev

COPY ./certs/ /etc/nginx/certs
COPY ./configs/ /etc/nginx/

RUN set -xe; \
    chown -R ${USER_NAME}:${GROUP_NAME} $SITE_SRC $ADMIN_SRC; \
    chown -R ${USER_NAME}:${GROUP_NAME} /etc/nginx /var/log /var/cache/nginx/app; \
    chown ${USER_NAME}:${GROUP_NAME} /var/run/nginx.pid

# Expose webserver port
EXPOSE 80 443

COPY ./docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

WORKDIR /etc/nginx/

ENTRYPOINT ["/entrypoint.sh"]

# @see https://ubuntu.com/blog/avoiding-dropped-connections-in-nginx-containers-with-stopsignal-sigquit
STOPSIGNAL SIGQUIT

CMD ["nginx", "-g", "daemon off;"]

# vim:set ft=dockerfile: