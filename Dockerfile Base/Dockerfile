FROM alpine:edge


# LABEL

LABEL org.opencontainers.image.source="https://github.com/BiswajyotiRay/mltb-alpine"
LABEL org.opencontainers.image.description="Docker for MLTB based on Alpine EDGE docker image"


ARG TARGETPLATFORM BUILDPLATFORM

# Setup Working Directory

WORKDIR /usr/src/app
RUN chmod -R 777 /usr/src/app && \
    chmod -R +x /usr/src/app && \
    chmod -R 705 /usr/src/app

ENV TZ Asia/Kolkata


# Installing basic packages

RUN echo -e "\e[32m[INFO]: Installing basic packages.\e[0m" && \
    apk update && apk upgrade && \
    apk add --upgrade --no-cache \
    sudo py3-wheel musl-dev musl python3 \
    python3-dev busybox musl-locales github-cli lshw \
    qbittorrent-nox py3-pip py3-lxml aria2 p7zip \
    xz curl pv jq ffmpeg parallel \
    neofetch git make g++ gcc automake zip unzip \
    autoconf speedtest-cli mediainfo bash \
    musl-utils tzdata gcompat libmagic \
    libffi-dev py3-virtualenv libffi \
    dpkg cmake icu-data-full apk-tools \
    nodejs coreutils npm bash-completion bash-doc \
    && npm install -g localtunnel kill-port && \
    sed -i -e "s/bin\/ash/bin\/bash/" /etc/passwd

SHELL ["/bin/bash", "-c"]


# Installing Build Tools

RUN echo -e "\e[32m[INFO]: Installing Building tools.\e[0m" && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
    sudo apk add --upgrade --no-cache \
    libtool curl-dev libsodium-dev c-ares-dev sqlite-dev freeimage-dev swig boost-dev \
    libpthread-stubs zlib zlib-dev libpq-dev clang clang-dev ccache gettext gettext-dev \
    gawk crypto++ crypto++-dev libjpeg-turbo-dev 


# Install Cloudflared Tunnel

RUN echo -e "\e[32m[INFO]: Installing Cloudflared Tunnel.\e[0m" && \
    case ${TARGETPLATFORM} in \
         "linux/amd64")  ARCH=amd64  ;; \
         "linux/arm64")  ARCH=arm64  ;; \
         "linux/arm/v7") ARCH=arm    ;; \
    esac && \
    wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-${ARCH}.deb -O cloudflared-linux-${ARCH}.deb && \
    sudo dpkg -i --force-architecture cloudflared-linux-${ARCH}.deb

ENV PYTHONWARNINGS=ignore
# install megasdkrest
RUN curl -L https://github.com/BiswajyotiRay/megasdkrest/releases/download/v0.1.2/megasdkrest-amd64 \
-o home/bin/megasdkrest && chmod +x home/bin/megasdkrest

RUN echo -e "\e[32m[INFO]: Caching Pip Requirements.\e[0m" && \  
    wget https://raw.githubusercontent.com/BiswajyotiRay/MlX-Docker/main/requirements.txt -O requirements.txt && \
    pip3 install -U -r requirements.txt

# Running Final Apk Update

RUN echo -e "\e[32m[INFO]: Running Final Apk Update.\e[0m" && \
    sudo apk update && apk upgrade


# Setup Language Environments
ENV LANG="en_US.UTF-8" LANGUAGE="en_US:en" LC_ALL="en_US.UTF-8"
RUN echo 'export LC_ALL=en_US.UTF-8' >> /etc/profile.d/locale.sh && \
    sed -i 's|LANG=C.UTF-8|LANG=en_US.UTF-8|' /etc/profile.d/locale.sh && \
    cp /usr/share/zoneinfo/Asia/Kolkata /etc/localtime && \
    rm -rf *


SHELL ["/bin/bash", "-c"]
