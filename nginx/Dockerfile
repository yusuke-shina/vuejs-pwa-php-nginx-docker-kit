ARG NGINX_VERSION=alpine
FROM nginx:${NGINX_VERSION}

RUN apk upgrade && \
    apk update && \
    apk --update add \
        vim \
        openssl

WORKDIR /var/www/html